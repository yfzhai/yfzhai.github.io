---
layout: posts
title: "Java 使用ThreadLocal创建局部线程变量"
---

## Java ThreadLocal使用
----------------------------------
Java中ThreadLocal类是用来创建线程局部变量，我们知道，如果创建一个类对象，实现Runnable接口，然后多个Thread对象使用同样的Runnable对象，那么全部的线程都将共享同样的属性，因此，如果该属性不是线程安全的，那么我们可以使用同步机制，但是如果我们想避免使用同步机制，则可以使用ThreadLocal变量。      
每个线程都有自身的ThreadLocal变量，通过使用该变量的get()和set()方法，可以获得和改变线程的局部变量的属性。通常ThreadLocal的实例是private static类型的成员，以便和线程的状态关联。     
接下来，通过一个使用ThreadLocal的例子，我们说明每个线程都有一个ThreadLocal变量的拷贝：
<font size=4px>
<xmp class="prettyprint linenums">
import java.text.SimpleDateFormat;
import java.util.Random;

class ThreadLocalExample implements Runnable {
	//创建ThreadLocal<SimpleDateFormat>类对象，此催下有隐含实现initialValue()方法
	private static final ThreadLocal<SimpleDateFormat> format = new ThreadLocal<SimpleDateFormat>() {
		protected SimpleDateFormat initialValue() {
			return new SimpleDateFormat("yyyyMMdd HHmm");
		}
	};

	@Override
	public void run() {
		System.out.println("Thread Name=" + Thread.currentThread().getName()
				+ " default Formatter=" + format.get().toPattern());
		try {
			Thread.sleep(new Random().nextInt(1000));
		} catch (InterruptedException e) {
		}
		format.set(new SimpleDateFormat());
		System.out.println("Thread Name=" + Thread.currentThread().getName()
				+ " Formatter=" + format.get().toPattern());
	}
}

public class Main {
	public static void main(String[] args) throws InterruptedException {
		ThreadLocalExample obj = new ThreadLocalExample();
		for (int i = 0; i < 10; i++) {
			Thread thread = new Thread(obj, "" + i);
			Thread.sleep(new Random().nextInt(1000));
			thread.start();
		}
	}
}
</xmp>
</font>
程序输出结果如下：     
![ThreadLocal](/images/Java/threadLocal.jpg)     
本地线程变量为每个使用这些变量的线程存储属性值。可以用get()方法读取值和使用set()方法改变值。如果第一次你访问本地线程变量的值，如果没有值给当前的线程对象，那么本地线程变量会调用initialValue()方法来设置值给线程并返回初始值。   
从运行结果可以看出，Thread-0改变了format格式，但是Thread-2的默认format依然是初始值。
**说明：**ThreadLocal类在Java8中通过引入一个函数`withInitial()`被延伸了，因此我们可以通过Lambda表达式很方便的创建ThreadLocal实例，例如，上面代码中创建ThreadLocal实例的地方可以修改为：
<font size=4px>
<xmp class="prettyprint linenums">
private static final ThreadLocal<SimpleDateFormat> format = 
	ThreadLocal.<SimpleDateFormat> withInitial
	(() -> {return new SimpleDateFormat("yyyyMMdd HHmm");});
</xmp>
</font>