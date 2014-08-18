---
layout: posts
title: "Core Java Interview Questions and Answers"
---

## Core Java Interview Questions and Answers
----------------------------------
Whether you are a fresher or highly experienced professional, core java plays a vital role in any Java/JEE interview. Core Java is the favorite area in most of the interviews and plays a crucial role in deciding the outcome of your interview.     
In this paper, I will providing some of the important core java interview questions with answers that you should know. All these questions are from internet. Please visit:[http://www.journaldev.com/2366/core-java-interview-questions-and-answers](http://www.journaldev.com/2366/core-java-interview-questions-and-answers)      
**1、What are the important features of Java 8 release?**           
Java 8 has been released in March 2014, so it’s one of the hot topic in java interview questions. If you answer this question clearly, it will show that you like to keep yourself up-to-date with the latest technologies.    
Java 8 has been one of the biggest release after Java 5 annotations and generics. Some of the important features of Java 8 are:     
A.[Interface changes with default and static methods](http://www.journaldev.com/2752/java-8-interface-changes-static-methods-default-methods-functional-interfaces)         
B.[Functional interfaces and Lambda Expressions](http://www.journaldev.com/2763/java-8-lambda-expressions-and-functional-interfaces-example-tutorial)      
C.[Java Stream API for collection classes](http://www.journaldev.com/2774/java-8-stream-api-example-tutorial)     
D.[Java Date Time API](http://www.journaldev.com/2800/java-8-date-time-api-example-tutorial-localdate-instant-localdatetime-parse-and-format)      
I strongly recommend to go through above links to get proper understanding of each one of them, also read [Java 8 Features](http://www.journaldev.com/2389/java-8-features-for-developers-lambdas-functional-interface-stream-and-time-api).      
**2、What do you mean by platform independence of Java?**     
Platform independence means that you can run the same Java Program in any Operating System. For example, you can write java program in Windows and run it in Mac OS.     
**3、What is JVM and is it platform independent?**     
Java Virtual Machine (JVM) is the heart of java programming language. JVM is responsible for converting byte code into machine readable code. JVM is not platform independent, thats why you have different JVM for different operating systems. We can customize JVM with Java Options, such as allocating minimum and maximum memory to JVM. It’s called virtual because it provides an interface that doesn’t depend on the underlying OS.      
**4、What is the difference between JDK and JVM?**     
Java Development Kit (JDK) is for development purpose and JVM is a part of it to execute the java programs.JDK provides all the tools, executables and binaries required to compile, debug and execute a Java Program. The execution part is handled by JVM to provide machine independence.     
**5、What is the difference between JVM and JRE?**     
Java Runtime Environment (JRE) is the implementation of JVM. JRE consists of JVM and java binaries and other classes to execute any program successfully. JRE doesn’t contain any development tools like java compiler, debugger etc. If you want to execute any java program, you should have JRE installed.     
**6、Which class is the superclass of all classes?**     
java.lang.Object is the root class for all the java classes and we don’t need to extend it.     
**7、Why Java doesn’t support multiple inheritance?**      
Java doesn’t support multiple inheritance in classes because of “Diamond Problem”. To know more about diamond problem with example, read [Multiple Inheritance in Java](http://www.journaldev.com/1775/multiple-inheritance-in-java-and-composition-vs-inheritance).     
However multiple inheritance is supported in interfaces. An interface can extend multiple interfaces because they just declare the methods and implementation will be present in the implementing class. So there is no issue of diamond problem with interfaces.     
**8、Why Java is not pure Object Oriented language?**     
Java is not said to be pure object oriented because it support primitive types such as int, byte, short, long etc. I believe it brings simplicity to the language while writing our code. Obviously java could have wrapper objects for the primitive types but just for the representation, they would not have provided any benefit.      
As we know, for all the primitive types we have wrapper classes such as Integer, Long etc that provides some additional methods.      
**9、What is difference between path and classpath variables?**     
PATH is an environment variable used by operating system to locate the executables. That’s why when we install Java or want any executable to be found by OS, we need to add the directory location in the PATH variable. If you work on Windows OS, read this post to learn [how to setup PATH variable on Windows](http://www.journaldev.com/476/java-tutorial-1-setting-up-java-environment-on-windows).      
Classpath is specific to java and used by java executables to locate class files. We can provide the classpath location while running java application and it can be a directory, ZIP files, JAR files etc.       
**10、What is the importance of main method in Java?**     
main() method is the entry point of any standalone java application. The syntax of main method is public static void main(String args[]).     
main method is public and static so that java can access it without initializing the class. The input parameter is an array of String through which we can pass runtime arguments to the java program. Check this post to learn [how to compile and run java program](http://www.journaldev.com/481/java-tutorial-2-writing-and-running-first-java-program).     
**11、What is overloading and overriding in java?**     
When we have more than one method with same name in a single class but the arguments are different, then it is called as method overloading.     
Overriding concept comes in picture with inheritance when we have two methods with same signature, one in parent class and another in child class. We can use @Override annotation in the child class overridden method to make sure if parent class method is changed, so as child class.     
**12、Can we overload main method?**     
Yes, we can have multiple methods with name “main” in a single class. However if we run the class, java runtime environment will look for main method with syntax as public static void main(String args[]).      
**13、Can we have multiple public classes in a java source file?**      
We can’t have more than one public class in a single java source file. A single source file can have multiple classes that are not public.     
**14、What is Java Package and which package is imported by default?**      
Java package is the mechanism to organize the java classes by grouping them. The grouping logic can be based on functionality or modules based. A java class fully classified name contains package and class name. For example, `java.lang.Object` is the fully classified name of Object class that is part of `java.lang package`.     
`java.lang` package is imported by default and we don’t need to import any class from this package explicitly.     
**15、What are access modifiers?**     
Java provides access control through public, private and protected access modifier keywords. When none of these are used, it’s called default access modifier.A java class can only have public or default access modifier. Read [Java Access Modifiers](http://www.journaldev.com/2345/java-access-modifiers-public-protected-and-private-keywords) to learn more about these in detail.     
**16、What is final keyword?**     
final keyword is used with Class to make sure no other class can extend it, for example String class is final and we can’t extend it.     
We can use final keyword with methods to make sure child classes can’t override it.      
final keyword can be used with variables to make sure that it can be assigned only once. However the state of the variable can be changed, for example we can assign a final variable to an object only once but the object variables can change later on.      
Java interface variables are by default final and static.     
**17、What is static keyword?**      
static keyword can be used with class level variables to make it global i.e all the objects will share the same variable.static keyword can be used with methods also. A static method can access only static variables of class and invoke only static methods of the class.Read more in detail at [java static keyword](http://www.journaldev.com/1365/static-in-java-methods-variables-block-class).         
**18、What is finally and finalize in java?**     
`finally block` is used with try-catch to put the code that you want to get executed always, even if any exception is thrown by the try-catch block. finally block is mostly used to release resources created in the try block.      
`finalize()` is a special method in Object class that we can override in our classes. This method get’s called by garbage collector when the object is getting garbage collected. This method is usually overridden to release system resources when object is garbage collected.      
**19、Can we declare a class as static?**       
We can’t declare a top-level class as static however an inner class can be declared as static. If inner class is declared as static, it’s called static nested class.Static nested class is same as any other top-level class and is nested for only packaging convenience.Read more about inner classes at [java inner class](http://www.journaldev.com/996/java-nested-classes-java-inner-class-static-nested-class-local-inner-class-and-anonymous-inner-class).     
**20、What is static import?**      
If we have to use any static variable or method from other class, usually we import the class and then use the method/variable with class name.
<font size=4px>
<xmp class="prettyprint linenums">
import java.lang.Math;
//inside class
double test = Math.PI * 5;
</xmp>
</font>
We can do the same thing by importing the static method or variable only and then use it in the class as if it belongs to it.
<font size=4px>
<xmp class="prettyprint linenums">
import static java.lang.Math.PI;
//no need to refer class now
double test = PI * 5;
</xmp>
</font>
Use of static import can cause confusion, so it’s better to avoid it. Overuse of static import can make your program unreadable and unmaintainable.     
**21、What is try-with-resources in java?**      
One of the Java 7 features is try-with-resources statement for automatic resource management. Before Java 7, there was no auto resource management and we should explicitly close the resource. Usually, it was done in the finally block of a try-catch statement. This approach used to cause memory leaks when we forgot to close the resource.      
From Java 7, we can create resources inside try block and use it. Java takes care of closing it as soon as try-catch block gets finished. Read more at [Java Automatic Resource Management](http://www.journaldev.com/592/try-with-resource-example-java-7-feature-for-automatic-resource-management).      
**22、What is multi-catch block in java?**     
Java 7 one of the improvement was multi-catch block where we can catch multiple exceptions in a single catch block. This makes are code shorter and cleaner when every catch block has similar code. If a catch block handles multiple exception, you can separate them using a pipe (|) and in this case exception parameter (ex) is final, so you can’t change it. Read more at [Java multi catch block](http://www.journaldev.com/629/catching-multiple-exceptions-in-single-catch-and-rethrowing-exceptions-with-improved-type-checking-java-7-feature).      
**23、What is static block?**      
Java static block is the group of statements that gets executed when the class is loaded into memory by Java ClassLoader. It is used to initialize static variables of the class. Mostly it’s used to create static resources when class is loaded.      
**24、What is an interface?**     
Interfaces are core part of java programming language and used a lot not only in JDK but also java design patterns, most of the frameworks and tools. Interfaces provide a way to achieve abstraction in java and used to define the contract for the subclasses to implement. Interfaces are good for starting point to define Type and create top level hierarchy in our code. Since a java class can implements multiple interfaces, it’s better to use interfaces as super class in most of the cases. Read more at [java interface](http://www.journaldev.com/1601/what-is-interface-in-java-example-tutorial).      
**25、What is an abstract class?**      
Abstract classes are used in java to create a class with some default method implementation for subclasses. An abstract class can have abstract method without body and it can have methods with implementation also. Abstract keyword is used to create a abstract class. Abstract classes can’t be instantiated and mostly used to provide base for sub-classes to extend and implement the abstract methods and override or use the implemented methods in abstract class. Read important points about abstract classes at [java abstract class](http://www.journaldev.com/1582/abstract-class-in-java-with-example).      
**26、What is the difference between abstract class and interface?**       
Abstract keyword is used to create abstract class whereas interface is the keyword for interfaces. Abstract classes can have method implementations whereas interfaces can't. A class can extend only one abstract class but it can implement multiple interfaces. We can run abstract class if it has main() method whereas we can’t run an interface. Some more differences in detail are at [Difference between Abstract Class and Interface](http://www.journaldev.com/1607/difference-between-abstract-class-and-interface-in-java).     
**27、Can an interface implement or extend another interface?**     
Interfaces don’t implement another interface, they extend it. Since interfaces can’t have method implementations, there is no issue of diamond problem. That’s why we have multiple inheritance in interfaces i.e an interface can extend multiple interfaces.     
**28、What is Marker interface?**     
A marker interface is an empty interface without any method but used to force some functionality in implementing classes by Java. Some of the well known marker interfaces are Serializable and Cloneable.      
**29、What are Wrapper classes?**      
Java wrapper classes are the Object representation of eight primitive types in java. All the wrapper classes in java are immutable and final. Java 5 autoboxing and unboxing allows easy conversion between primitive types and their corresponding wrapper classes. Read more at [Wrapper classes in Java](http://www.journaldev.com/1002/java-wrapper-classes-tutorial-with-examples).       
**30、What is Enum in Java?**      
Enum was introduced in Java 1.5 as a new type whose fields consists of fixed set of constants. For example, in Java we can create Direction as enum with fixed fields as EAST, WEST, NORTH, SOUTH. Enum is the keyword to create an enum type and similar to class. Enum constants are implicitly static and final. Read more in detail at [java enum](http://www.journaldev.com/716/java-enum-examples-with-benefits-and-class-usage).      
**31、What is Java Annotations?**      
Java Annotations provide information about the code and they have no direct effect on the code they annotate. Annotations are introduced in Java 5. Annotation is metadata about the program embedded in the program itself. It can be parsed by the annotation parsing tool or by compiler. We can also specify annotation availability to either compile time only or till runtime also. Java Built-in annotations are @Override, @Deprecated and @SuppressWarnings. Read more at [java annotations](http://www.journaldev.com/721/java-annotations-tutorial-with-custom-annotation-example-and-parsing-using-reflection).      
**32、What is Java Reflection API? Why it’s so important to have?**      
Java Reflection API provides ability to inspect and modify the runtime behavior of java application. We can inspect a java class, interface, enum and get their methods and field details. Reflection API is an advanced topic and we should avoid it in normal programming. Reflection API usage can break the design pattern such as Singleton pattern by invoking the private constructor i.e violating the rules of access modifiers.      
Even though we don’t use Reflection API in normal programming, it’s very important to have. We can’t have any frameworks such as Spring, Hibernate or servers such as Tomcat, JBoss without Reflection API. They invoke the appropriate methods and instantiate classes through reflection API and use it a lot for other processing. Read [Java Reflection Tutorial](http://www.journaldev.com/1789/java-reflection-tutorial-for-classes-methods-fields-constructors-annotations-and-much-more) to get in-depth knowledge of reflection api.      
**33、What is composition in java?**     
Composition is the design technique to implement has-a relationship in classes. We can use Object composition for code reuse. Java composition is achieved by using instance variables that refers to other objects. Benefit of using composition is that we can control the visibility of other object to client classes and reuse only what we need. Read more with example at [Java Composition](http://www.journaldev.com/1325/what-is-composition-in-java-java-composition-example) example.     
**34、What is the benefit of Composition over Inheritance?**      
One of the best practices of java programming is to “favor composition over inheritance”. Some of the possible reasons are:     
※Any change in the superclass might affect subclass even though we might not be using the superclass methods. For example, if we have a method test() in subclass and suddenly somebody introduces a method test() in superclass, we will get compilation errors in subclass. Composition will never face this issue because we are using only what methods we need.      
※Inheritance exposes all the super class methods and variables to client and if we have no control in designing superclass, it can lead to security holes. Composition allows us to provide restricted access to the methods and hence more secure.      
※We can get runtime binding in composition where inheritance binds the classes at compile time. So composition provides flexibility in invocation of methods.      
You can read more about above benefits of composition over inheritance at [java composition vs inheritance](http://www.journaldev.com/1775/multiple-inheritance-in-java-and-composition-vs-inheritance).      
**35、How to sort a collection of custom Objects in Java?**     
We need to implement Comparable interface to support sorting of custom objects in a collection. Comparable interface has compareTo(T obj) method which is used by sorting methods and by providing this method implementation, we can provide default way to sort custom objects collection. However, if you want to sort based on different criteria, such as sorting an Employees collection based on salary or age, then we can create Comparator instances and pass it as sorting methodology. For more details read [Java Comparable and Comparator](http://www.journaldev.com/780/java-comparable-and-comparator-example-to-sort-objects).       
**36、What is inner class in java?**     
We can define a class inside a class and they are called nested classes. Any non-static nested class is known as inner class. Inner classes are associated with the object of the class and they can access all the variables and methods of the outer class. Since inner classes are associated with instance, we can’t have any static variables in them. We can have local inner class or anonymous inner class inside a class. For more details read [java inner class](http://www.journaldev.com/996/java-nested-classes-java-inner-class-static-nested-class-local-inner-class-and-anonymous-inner-class).     
**37、What is anonymous inner class?**     
A local inner class without name is known as anonymous inner class. An anonymous class is defined and instantiated in a single statement. Anonymous inner class always extend a class or implement an interface. Since an anonymous class has no name, it is not possible to define a constructor for an anonymous class. Anonymous inner classes are accessible only at the point where it is defined.      
**38、What is Classloader in Java?**      
Java Classloader is the program that loads byte code program into memory when we want to access any class. We can create our own classloader by extending ClassLoader class and overriding loadClass(String name) method. Learn more at [java classloader](http://www.journaldev.com/349/java-interview-questions-understanding-and-extending-java-classloader).      
**39、What are different types of classloaders?**      
There are three types of built-in Class Loaders in Java:     
A. Bootstrap Class Loader – It loads JDK internal classes, typically loads rt.jar and other core classes.     
B. Extensions Class Loader – It loads classes from the JDK extensions directory, usually $JAVA_HOME/lib/ext directory.     
C. System Class Loader – It loads classes from the current classpath that can be set while invoking a program using -cp or -classpath command line options.      
**40、What is ternary operator in java?**     
Java ternary operator is the only conditional operator that takes three operands. It’s a one liner replacement for if-then-else statement and used a lot in java programming. We can use ternary operator if-else conditions or even switch conditions using nested ternary operators. An example can be found at [java ternary operator](http://www.journaldev.com/963/java-ternary-operator-examples).     

<font size=4px> 
<xmp class="prettyprint linenums">

</xmp>
</font>
