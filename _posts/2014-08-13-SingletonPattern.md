---
layout: posts
title: "单例(Singleton)模式"
---

## 单例(Singleton)模式
----------------------------------
所谓单例模式，就是确保某个类只有一个实例，而且自行实例化并向整个系统提供这个实例。这个类称为单例类。因此，单例模式的要点有三个：一是某个类只能有一个实例；二是它必须自行创建这个实例；三是它必须自行向整个系统提供这个实例。     
然而，由于Java语言的特点，使得单例模式在Java语言的实现上有自己的特点。这些特点主要表现在单例类如何将自己实例化上。在这篇文章中，我们将讨论单例模式的不同实现方法。     
**饿汉式单例类**     
该模式的类加载时就已经被实例化，而不管其他类是否需要它被实例化。     
<font size=4px>
<xmp class="prettyprint linenums">
public class EagerSingleton {
	private static final EagerSingleton m_Instance = new EagerSingleton();

	private EagerSingleton() {
	}

	public static EagerSingleton getInstance() {
		return m_Instance;
	}
}
</xmp>
</font>
可以看出，这个类被加载时，静态变量m_Instance会被初始化，此时类的私有构造方法会被调用，这时候单例类的唯一实例就被创建出来了。由于构造方法是私有的，因此避免外界利用构造方法直接创建出任意多实例，并且不能被继承。     
**懒汉式单例类**     
与饿汉式单例类相同之处是类的构造方法是私有的，不同之处是懒汉式单例类不会在被加载时实例化，而是在第一次被引用时才将自己实例化。
<font size=4px>
<xmp class="prettyprint linenums">
public class LazySingleton {
	private static volatile LazySingleton m_Instance = null;

	private LazySingleton() {
	}

	public static LazySingleton getInstance() {
		if (m_Instance == null) {
			synchronized (LazySingleton.class) {
				m_Instance = new LazySingleton();
			}
		}
		return m_Instance;
	}
}
</xmp>
</font>
第一次调用时，上面方法会检查实例是否已经被创建，如果没有创建，将会创建实例并返回引用。如果已经被创建，那么仅仅返回实例的引用。但是该方法仍有其缺点，比如说有两个线程T1和T2，都来创建类实例并且都执行到instance==null，即两个线程变量都有相同的null实例变量，假如两个线程都要创建实例，那么它们将顺序进入同步块并且创建实例，最终，我们的应用程序中将会有两个实例变量。     
这个错误可以用双重检查来解决。
<font size=4px>
<xmp class="prettyprint linenums">
public class LazySingleton {
	private static volatile LazySingleton m_Instance = null;

	private LazySingleton() {
	}

	public static LazySingleton getInstance() {
		if (m_Instance == null) {
			synchronized (LazySingleton.class) {
				//Double check
				if(m_Instance == null)
					m_Instance = new LazySingleton();
			}
		}
		return m_Instance;
	}
}
</xmp>
</font>
令人吃惊的是，在C语言里得到普遍应用的双重检查成例在多数的Java语言编译器里面并不成立。上面使用了双重检查成例的“懒汉式”单例类，不能工作的基本原因在于，在Java编译器中，LazySingleton类的初始化与m_instance变量赋值的顺序不可预料。如果一个线程在没有同步化的条件下读取m_instance引用，并调用这个对象的方法的话，可能会发现对象的初始化过程尚未完成，从而造成崩溃。    
一般而言，双重检查成例对Java语言来说是不成立的。在一般情况下，使用饿汉式单例模式或者对整个静态工厂方法同步化的懒汉式单例模式足以解决在实际设计工作中遇到的问题。    
详情请看[The "Double-Checked Locking is Broken" Declaration](http://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html)     
**注意**    
Please ensure to use “volatile” keyword with instance variable otherwise you can run into out of order write error scenario, where reference of instance is returned before actually the object is constructed i.e. JVM has only allocated the memory and constructor code is still not executed. In this case, your other thread, which refer to uninitialized object may throw null pointer exception and can even crash the whole application.    

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>

<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>
参考      
[http://www.javaworld.com/article/2073352/core-java/simply-singleton.html?nsdr=true](http://www.javaworld.com/article/2073352/core-java/simply-singleton.html?nsdr=true)        
[http://howtodoinjava.com/2012/10/22/singleton-design-pattern-in-java/](http://howtodoinjava.com/2012/10/22/singleton-design-pattern-in-java/)