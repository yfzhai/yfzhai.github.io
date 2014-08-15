---
layout: posts
title: "Java 在线程里处理非检查异常"
---

## Java 线程中处理非检查异常
----------------------------------
Java中有两种异常：    
1、检查异常(Checked exceptions):这些异常必须强制捕获它们或在一个方法里的throws子句中，例如：IOException或ClassNotFoundException。     
2、非检查异常(Unchecked exceptions):这些异常不用强制捕获它们，例如：NumberFormatException。      
在一个线程对象的run()方法里抛出一个检查异常，我们必须捕获并处理它们。因为run()方法不接受throws子句。当一个非检查异常被抛出，默认的行为是在控制台写下stack trace并推出程序。幸运的是，Java提供给我们一种机制可以捕获和处理线程对象抛出的未检测异常来避免程序终结。     
接下来，我们通过示例讲解这种机制：     
1、首先，我们需要实现一个用于处理非检查异常的类，这个类必须实现UncaughtExceptionHandler类，实现在该接口中声明的uncaughtException()方法。
<font size=4px>
<xmp class="prettyprint linenums">
import java.lang.Thread.UncaughtExceptionHandler;

public class ExceptionHandler implements UncaughtExceptionHandler {
	@Override
	public void uncaughtException(Thread arg0, Throwable arg1) {
		System.out.println("An Exception has been captured");
		System.out.println("Thread:" + arg0.getId());
		System.out.println("Exception:" + arg1.getClass().getName() + ":"
				+ arg1.getMessage());
		System.out.println("Stack Trace:");
		arg1.printStackTrace(System.out);
		System.out.println("Thread status:" + arg0.getState());
	}
}
</xmp>
</font>
2、实现一个可以抛出非检查异常的类，称为Task，实现Runnable接口，实现run方法
<font size=4px>
<xmp class="prettyprint linenums">
public class Task implements Runnable {
	@Override
	public void run() {
		int num = Integer.parseInt("diag");
	}
}
</xmp>
</font>
3、创建主类
<font size=4px>
<xmp class="prettyprint linenums">
public class Main {
	public static void main(String[] args) {
		Task task = new Task();
		Thread thread = new Thread(task);
		//设置非检查异常的处理类
		thread.setUncaughtExceptionHandler(new ExceptionHandler());
		thread.start();
	}
}
</xmp>
</font>
执行结果：
<font size=4px>
<xmp class="prettyprint linenums">
An Exception has been captured
Thread:10
Exception:java.lang.NumberFormatException:For input string: "diag"
Stack Trace:
java.lang.NumberFormatException: For input string: "diag"
	at java.lang.NumberFormatException.forInputString(Unknown Source)
	at java.lang.Integer.parseInt(Unknown Source)
	at java.lang.Integer.parseInt(Unknown Source)
	at Task.run(Task.java:4)
	at java.lang.Thread.run(Unknown Source)
Thread status:RUNNABLE
</xmp>
</font>
从上面的输出片段可以看出异常执行的结果。异常被抛出，然后被处理类捕获并将异常信息打印到了控制台。     
当一个线程抛出一个异常，并且该异常（这里特指非受检异常）没有捕获时，Java虚拟机会检查是否通过相应方法设置非受检异常处理类，如果以已经设置过，则调用uncaughtException()方法，并将线程和异常作为参数传递给方法。如果没有设置处理类，Java虚拟机就会在控制台将堆栈跟踪信息打印出来，然后退出程序。     
**补充**     
Thread类还有一个和非受检异常处理相关的方法。这就是静态方法`setDefaultUncaughtExceptionHandler()`，该方法可以设置程序中所有线程的非受检异常的处理类。     
当线程中抛出一个未捕获的异常时，Java虚拟机会从三个地方寻找异常处理类：      
首先，从线程对象中查找异常处理类，这就是我们本节所学内容。如果不存在，则从线程所在的线程组中查找异常处理类。如果还不存在，则查找上面刚提到的程序默认异常处理类。   