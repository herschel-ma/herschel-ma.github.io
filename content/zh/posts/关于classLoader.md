---
title: "关于classLoader"
date: 2021-02-13T10:39:35+08:00
description: "classLoader 的常用方法及获取方法"
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Herschel
authorEmoji: 🎅
pinned: false
tags:
- JVM
- Java
series:
-
categories:
- Java
image: images/JVM/双亲委派机制.png
---
## 关于classLoader
- ClassLoader 类，它是一个抽象类，其后所有的类加载器都继承自 ClassLoader (不包括启动类加载器)。
- 概览

| 方法名称                                            | 描述                                                                         |
| -------------                                       | -------------                                                                |
| getParent()                                         | 返回该类加载器的超类加载器                                                   |
| loadClass(String name)                              | 加载名为 name 的类，返回结果为 java.lang.Class 类的实例                      |
| findClass(String name)                              | 查找名为 name 的类，返回结果为 java.lang.Class 类的实例                      |
| findLoadedClass(String name)                        | 查找名为 name 的已经被加载过的类，返回结果为 java.lang.Class 类的实例        |
| defineClass(String name,byte[] b, int off, int len) | 把字节数组中 b 的内容转换为一个 Java 类，返回结果为 java.lang.Class 类的实例 |
| findLoadedClass(String name)                        | 查找名为 name 的已经被加载过的类，返回结果为 java.lang.Class 类的实例        |

## 获取 classLoader 的途径
1. 获取当前类的 classLoader
`class.getClassLoader()`

2. 获取当前线程上下文的 ClassLoader
`Thread.currentThread().getContextClassLoader()`

3. 获取系统的 ClassLoader
`ClassLoader.getSystemClassLoader()`

4. 获取调用者的 ClassLoader
`DriverManager.getCallerClassLoader()`

```java
public classLoaderTest {
    public static void main (String[] args) {
        try{
            ClassLoader classLoader = Class.forName("java.lang.String").getClassLoader();
            System.out.println(classLoader);

            ClassLoader classLoader1 = Thread.currentThread.getContextClassLoader();
            System.out.println(classLoader1);

            ClassLoader classLoader2 = ClassLoader.getSystemClassLoader.getParent();
            System.out.println(classLoader2);
        }catch(ClassNotFountException e){
            e.printStackTrace();
        }
    }
}

```

## 双亲委派机制
### 简介
- Java虚拟机对 class 文件采取的是`按需加载`的方式，也就是说当需要使用该类时才将它的 class 文件加载到内存中生成 class 对象。而且加载某个 class 文件时，Java虚拟机采用的是`双亲委派模式`，即把请求交给父类处理，它是一种任务委派模式。

### 工作原理
![双亲委派机制](/images/JVM/双亲委派机制.png)

1. 如果一个类加载器收到了类加载请求，他不会自己先去加载，而是把这个请求委托给父类的加载器去执行；
2. 如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归；最终请求将会到达顶层的启动类加载器；
3. 如果父类加载器可以完成类加载任务，就成功返回，否则才尝试自己加载，这就是双亲委派机制。

### 优势
1. 避免类的重复加载
2. 保护程序安全，防止核心 API 被篡改
    - 自定义类 java.lang.String
    - 自定义类 java,lang.ShkStart

### 沙箱安全机制
- 自定义 String 类，但是在加载自定义 String 类的时候会率先使用引导类加载器加载，而引导类加载器在加载的过程中会先加载jdk自带的文件（rt.jar包中的 java/lang/String.class），报错信息说没有main方法，就是因为加载的是 rt.jar 包中的 String 类。这样可以保证对 java 核心源代码的保护，这就是`沙箱安全机制`。

## 其他
- 在 JVM 中表示两个 class 对象是否是同一个类存在两个必要条件：
    1. 类的完整类名必须一致，包括包名。
    2. 加载这个类的 ClassLoader（指 ClassLoader 实例对象）必须相同。

- 换句话说，在 JVM 中，即使这两个类对象来源于同一个Class文件，被同一个虚拟机所加载，但只要加载他们的 ClassLoader 实例对象不同，那么这两个类对象也是不相等的。

## 类加载器的引用
- JVM 必须知道一个类型是由启动加载器加载的还是由用户类加载器加载的。如果一个类型是由用户类加载器加载的，那么 JVM 会将这个类加载器的一个引用作为类型信息的一部分包存在方法区中。当解析一个类型到另一个类型的引用的时候，JVM 需要保证这两个类型的类加载器是相同的。

## 类的主动使用和被动使用
**Java 程序对类的使用方式分为：主动使用和被动使用。**

- 主动使用，又分为7种情况：
    - 创建类的实例
    - 访问某个类或接口的静态变量，或者对该静态变量赋值
    - 调用类的静态方法
    - 反射（比如：Class.forName('java.lang.String')
    - 初始化一个类的子类
    - Java 虚拟机启动时被标明为启动类的类
    - JDK7 开始提供的动态语言支持：java.lang.invoke.MethodHandle实例的解析结果、REF_getStatic、REF_invokeStatic 句柄对应的类没有初始化，则初始化

- 除了以上七种情况，其他使用 Java 类的方式都被看作是对**类的被动使用**，都**不会导致类的初始化**。
