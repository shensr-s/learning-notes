# Java 学习/面试指南笔记

**项目地址：**[https://snailclimb.gitee.io/javaguide/](https://snailclimb.gitee.io/javaguide/)

## Java

###  基础

**原文地址：**[Java基本功](https://snailclimb.gitee.io/javaguide/#/docs/java/Java%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86?id=_11-java-%e5%85%a5%e9%97%a8%ef%bc%88%e5%9f%ba%e7%a1%80%e6%a6%82%e5%bf%b5%e4%b8%8e%e5%b8%b8%e8%af%86%ef%bc%89)

1. 在 Java 中，JVM 可以理解的代码就叫做`字节码`（即扩展名为 `.class` 的文件），它不面向任何特定的处理器，只面向虚拟机。Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。所以 Java 程序运行时比较高效，而且，由于字节码并不针对一种特定的机器，因此，Java 程序无须重新编译便可在多种不同操作系统的计算机上运行。

2. Java 虚拟机（JVM）是运行 Java 字节码的虚拟机。JVM 有针对不同系统的特定实现（Windows，Linux，macOS），目的是使用相同的字节码，它们都会给出相同的结果。字节码和不同系统的 JVM 实现是 Java 语言“一次编译，随处可以运行”的关键所在。

3. 一个程序中可以有多个类，但只能有一个类是主类。在 Java 应用程序中，这个主类是指包含 main（）方法的类。而在 Java 小程序中，这个主类是一个继承自系统类 JApplet 或 Applet 的子类。应用程序的主类不一定要求是 public 类，但小程序的主类要求必须是 public 类。主类是 Java 程序执行的入口点。

4. **字符型常量和字符串常量的区别：**

   - 形式上: 字符常量是单引号引起的**一个字符**; 字符串常量是双引号引起的**若干个字符**

   - 含义上: 字符常量相当于一个整型值( ASCII 值),可以参加表达式运算; 字符串常量代表一个地址值(该字符串在内存中存放位置)

   - 占内存大小 字符常量只占 2 个字节; 字符串常量占若干个字节 (**注意： char 在 Java 中占两个字节**)
   - ![img](http://my-blog-to-use.oss-cn-beijing.aliyuncs.com/18-9-15/86735519.jpg)

5. 代码的注释不是越详细越好。实际上好的代码本身就是注释，我们要尽量规范和美化自己的代码来减少不必要的注释。

   若编程语言足够有表达力，就不需要注释，尽量通过代码来阐述。

6. continue ：指跳出当前的这一次循环，继续下一次循环。

   break ：指跳出整个循环体，继续执行循环下面的语句。

   return 用于跳出所在方法，结束该方法的运行。return 一般有两种用法：

   - `return;` ：直接使用 return 结束方法执行，用于没有返回值函数的方法
   - `return value;` ：return 一个特定值，用于有返回值函数的方法

7. **常用的通配符为： T，E，K，V，？**
   - ？ 表示不确定的 java 类型
   - T (type) 表示具体的一个java类型
   - K V (key value) 分别代表java键值中的Key Value
   - E (element) 代表Element