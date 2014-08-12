---
layout: posts
title: "Java Dynamic Proxy from Internet"
---

## Java Dynamic Proxy
----------------------------------
This article explains one of the important and powerful concept introduced in Java 1.3 called "Java Dynamic Proxy". We will explain this using a simple calculator example.
### Simple Calculator Example
Consider a simple calculator class having add method which adds two numbers. For simplicity we have only one operation ADD. Below is the interface and implementation for the same.
<font size=4px>
<xmp class="prettyprint linenums">
interface Calculator{
	public int add(int a, int b);
}
public class CalculatorImpl implements Calculator{
	public int add(int a, int b) {
		int c = a + b;
		System.out.println("[TARGET METHOD CALL] Result is "+c);
		return c;
	}
}
</xmp>
</font>
Suppose we had to implement new calculator service with the folowing requirements.     
1、Should not allow addition of -ve numbers.     
2、Should log the input and output.      
3、Should reuse the core addition logic.      
4、Changes should not affect the classes which are already using the old service.      
In short, we will have two services, one which will allow addition of only +ve numbers and another will allow addition of both +ve and -ve numbers. Note that the changes are only in the validation and logging and the core addition logic remains the same.     
We cannot make changes in the current services as our requirements does not allow us to do so. We can create a replica of the current service and code above requirements in this new service. But this solution is also not feasible because of following reasons.      
1、Hard to maintain. Changes to core implementation needs to be replicated to new service.       
2、We lose code reusability. It does not reuse the core implementation.        
3、New methods (subtract , divide, multiply etc) added to CalculatorImpl needs to be replicated with the new requirements in the new service.      
No solutions is working. So are we stuck? No, we have Java Dynamic Proxy for our rescue.
### Understanding Java Dynamic Proxy
A dynamic proxy class is a class that implements a list of interfaces specified at runtime when the class is created. A proxy interface is such an interface that is implemented by a proxy class. A proxy instance is an instance of a proxy class. Each proxy instance has an associated invocation handler object, which implements the interface InvocationHandler. A method invocation on a proxy instance through one of its proxy interfaces will be dispatched to the invoke method of the instance’s invocation handler, passing the proxy instance, a java.lang.reflect.Method object identifying the method that was invoked, and an array of type Object containing the arguments. The invocation handler invokes the appropriate method and the result that it returns will be returned as the result of the method invocation on the proxy instance.The following is a diagrammatic representation of Java Dynamic Proxy.      
![Diagrammatic representation of Java Dynamic Proxy](/images/Java/Proxy_Diagram.jpg)     
A dynamic proxy is an instance that acts as a pass through to the real object. This powerful pattern let you change the real behavior from a caller point of view by intercepting the method calls so that we can add our code before and after the target method call.      
In short, there are two things that we need to do in order to implement java dynamic proxy.     
1、Implementing the proxy behavior.     
2、Creation of proxy instance.     
We will see all this is coming sections.
### Creating InvocationHandler
Implementation of proxy behavior is done in an implementation of java.lang.reflect.InvocationHandler. It has single method to implement as below.     
`public Object invoke(Object proxy, Method method, Object[] args)`     
It processes a method invocation on a proxy instance and returns the result. It will be invoked on an invocation handler when a method is invoked on a proxy instance associated with it.     
**proxy:**The proxy instance that the method was invoked on.     
**method:**The Method instance corresponding to the interface method invoked on the proxy instance. The declaring class of the Method object will be the interface that the method was declared in, which may be a super interface of the proxy interface that the proxy class inherits the method through.     
**args:**An array of objects containing the arguments passed in the method invocation on the proxy instance, or null if interface method takes no arguments. Primitive arguments are wrapped in instances of appropriate primitive wrapper class, such as java.lang.Integer or java.lang.Boolean
<font size=4px>
<xmp class="prettyprint linenums">
class LogginValidationHandler implements InvocationHandler {
	private Object target;

	public LogginValidationHandler(Object target) {
		this.target = target;
	}

	@Override
	public Object invoke(Object proxy, Method method, Object[] args)
			throws Throwable {
		// Prints the input
		System.out.println("[BEFORE METHOD CALL] The method "
				+ method.getName() + "() begins with " + Arrays.toString(args));
		// Negative check for input
		Integer a = (Integer) args[0];
		Integer b = (Integer) args[1];
		if (a < 0 || b < 0) {
			throw new IllegalArgumentException("Positive numbers only");
		}
		Object result = method.invoke(target, args);
		// Prints the output
		System.out.println("[AFTER METHOD CALL] The method " + method.getName()
				+ "() ends with " + result.toString());
		return result;
	}
}
</xmp>
</font>
### Creating Dynamic Proxy
In the previous section we saw how to create the InvoctionHandler and code our requirements in it. In this section we will see how to use InvoctionHandler, creating and using proxy instance.
<font size=4px>
<xmp class="prettyprint linenums">
public class ProxyTest {
	public static void main(String[] args) {
		// Plain business object
		Calculator plainCalculator = new CalculatorImpl();
		// Proxied business object
		Calculator proxiedCalculator = (Calculator) Proxy.newProxyInstance(
				plainCalculator.getClass().getClassLoader(), plainCalculator
						.getClass().getInterfaces(),
				new LogginValidationHandler(plainCalculator));
		System.out.println("Call to add method without using the proxy.");
		plainCalculator.add(1, 2);
		System.out.println("---------------------------------------------");
		System.out.println("Call to add method with +ve arguments using the proxy.");
		proxiedCalculator.add(1, 2);
		System.out.println("---------------------------------------------");
		System.out.println("Call to add method with -ve arguments using the proxy.");
		proxiedCalculator.add(-1, 2);
	}
}
</xmp>
</font>
**Line 5:**Create an instance of the core calculator implementation CalculatorImpl.    
**Line 7 - 10:**We create dynamic proxies using the Proxy.newProxyInstance() which is a static method. The newProxyInstance() methods takes 3 parameters.     
**`public static Object newProxyInstance(ClassLoader loader, Class[] interfaces, InvocationHandler h)`**     
Returns an instance of a proxy class for the specified interfaces that dispatches method invocations to the specified invocation handler.     
**loader:**ClassLoader in which to load the dynamic proxy class.      
**interfaces:**An array of interfaces to implement.      
**h:**An InvocationHandler to forward all methods calls on the proxy to.       
<font color="red">
Note: Java Dynamic Proxy can only create proxy instance of classes which implement interfaces. Proxy API uses Java Reflection to create dynamic implementations of interfaces at runtime. This is why we call it java DYNAMIC proxy.      
</font>
Output of ProxyTest    
<font size=4px>
<xmp class="prettyprint linenums">
Call to add method without using the proxy.
[TARGET METHOD CALL] Result is 3
---------------------------------------------
Call to add method with +ve arguments using the proxy.
[BEFORE METHOD CALL] The method add() begins with [1, 2]
[TARGET METHOD CALL] Result is 3
[AFTER METHOD CALL] The method add() ends with 3
---------------------------------------------
Call to add method with -ve arguments using the proxy.
[BEFORE METHOD CALL] The method add() begins with [-1, 2]
Exception in thread "main" java.lang.IllegalArgumentException: Positive numbers only
	at LogginValidationHandler.invoke(ProxyTest.java:36)
	at $Proxy0.add(Unknown Source)
	at ProxyTest.main(ProxyTest.java:62)
</xmp>
</font>
