## JVM的类加载机制

什么是类加载机制，在Java中我们编写的都是Java类（.java）文件，通过JVM进行编译成字节码（.class）文件，在启动时候字节码文件（.class）需要加载到jvm内存中，这个过程就是类加载过程。

> JVM 的类加载机制中有一个非常重要的⻆色叫做类加载器(ClassLoader)，类加载器有自己的体系， Jvm内置了几种类加载器，包括:引导类加载器、扩展类加载器、系统类加载器，他们之间形成父子关 系，通过 Parent 属性来定义这种关系，最终可以形成树形结构。   

![](https://elgchat-oss.oss-accelerate.aliyuncs.com/elgchat/2021_03_22/F448A9B6-0C40-4DE0-B0EB-2708B7411A27.png)

* **Boostarp ClassLoader（引导类加载器）**
由C++编写，加载java核心库，java.* ，构造Ext ClassLoader（扩展类加载器）和App ClassLoader（应用程序类加载器）
* **Extension ClassLoader（扩展类加载器）**
由Java编写，加载扩展库JAVA_HOME/lib/ext目录下的jar包
* **Appliction ClassLoader（应用程序类加载器）**
默认的类加载器，搜索环境变量classpath中指定的路径
* **User ClassLoader（自定义类加载器）**
用户自定义的类加载器，可加载指定路径的class文件

## 双亲委派机制
### 什么是双亲委派机制
当某个类加载器需要加载某个.class文件时，它首先把这个任务委托给他的上级类加载器，递归这个操 
作，如果上级的类加载器没有加载，自己才会去加载这个类。

### 双亲委派机制的作用
* 防止重复加载同一个.class。通过委托去向上面问一问，加载过了，就不用再加载一遍。保证数据 
安全。 
* 保证核心.class不能被篡改。通过委托方式，不会去篡改核心.class，即使篡改也不会去加载，即使 加载也不会是同一个.class对象了。不同的加载器加载同一个.class也不是同一个.class对象。这样 保证了class执行安全(如果子类加载器先加载，那么我们可以写一些与java.lang包中基础类同名的类，然后再定义一个子类加载器，这样整个应用使用的基础类就都变成我们自己定义的类了。 ) 

