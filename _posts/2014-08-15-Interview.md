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
<font size=4px>
<xmp class="prettyprint linenums">

</xmp>
</font>
