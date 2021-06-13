---
title: "认识JVM"
date: 2021-02-11T23:56:13+08:00
description: "认识JVM"
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
image: images/JVM/JVM_STRUCTURE.png
---
## JVM的整体结构
- HotSpot VM是目前市面上高性能虚拟机的代表作之一。
- 它采用解释器与即时编译器并存的架构。
- 在今天，Java程序的运行性能早已脱胎换骨，已经达到了可以和c/c++程序一较高下的地步。

![JVM的整体结构](/images/JVM/JVM_STRUCTURE.png)

## JVN的架构模型
- Java编译器输入的指令流基本上是一种基于`栈的指令集架构`,另外一种指令集架构则是基于`寄存器的指令集架构`。

- 区别:
    - 基于栈式架构的特点：
        1. 设计和实现更加简单，适用于资源受限的系统；
        2. 避开了寄存器的分配难题，使用零地址指令方式分配；
        3. 指令流中的指令大部分是零地址指令，其执行过程依赖于操作栈。指令集更小，编译器容易实现；
        4. 不需要硬件支持，可移植性更好，更好实现跨平台。

    - 基于寄存器架构的特点：
        1. 典型的应用是x86的二进制指令集：比如传统的PC以及Andriod的Davlik虚拟机。
        2. 指令集架构则完全依赖硬件，可移植性差。
        3. 性能优秀和执行更高效。
        4. 花费更少的指令去完成一项操作。
        5. 在大部分情况下，基于寄存器架构的指令集往往都以一地址指令、二地址指令和三地址指令为主，而基于栈式架构的指令集却是以零地址指令为主。

## JVM的生命周期
### 虚拟机的启动
- Java虚拟机的启动是通过引导类加载器（bootstrap class loader）创建一个初始类（initail class）来完成的，这个类是由虚拟及的具体实现指定的。

### 虚拟机的执行
- 一个运行中的Java虚拟机有一个清晰的任务：执行Java程序；
- 程序开始执行时它才运行，程序结束时它就停止；
- 执行一个所谓的Java程序的时候，真真正正在执行的是一个叫做Java虚拟机的进程。

### 虚拟机的退出
- 有如下几种情况：
    - 程序正常执行结束
    - 程序在执行过程中遇到了异常或者错误而异常终止
    - 由于操作系统出现错误而导致Java虚拟机进程终止
    - 某线程调用Runtime类或者System类的exit方法，或Runtime类的halt方法，并且Java安全管理器也允许这次exit或halt操作
    - 除此之外，JNI（Java Native Interface）规范描述了用JNI Invocation API来加载或者卸载Java虚拟机时，Java虚拟机的退出情况。

## JVM发展历程
### Sun Classic VM
- 早在1996年Java1.0版本的时候，Sun公司发布了一款名叫Sun Classic VM的Java虚拟机，它同时也是`世界上第一款商用Java虚拟机`,JKD1.4时完全被淘汰。
- 这款虚拟机内部只提供解释器
    - 虚拟机在解释运行字节吗文件的时候，可以只使用`解释器`或者`JIT（即时编译器）`
- 如果使用JIT编译器，就需要进行外挂。但是一旦使用了JIT编译器，JIT就会接管虚拟机的执行系统。解释器就不再工作。解释器和编译器不能配合工作。
- 现在hotspot内置了此虚拟机。

### Exact VM
- 为了解决上一个虚拟机问题，在jdk1.2时，sun 提供了此虚拟机
- Exact Memory Management: 准确式内存管理
    - 也可以叫Non-Conservative/Accurate Memory Management
    - 虚拟机可以知道内存中某个为之的数据具体是什么类型。
- 具体被现代高性能虚拟机的雏形
    - 热点探测
    - 编译器与解释器混合工作模式
- 只在Solaris平台短暂使用，其他平台还是classic VM
    - 英雄气短，后被Hotspot虚拟机代替

### HotSpot VM
- HotSpot历史
    - 最初由一家名为“Longview Technologies”的小公司设计
    - 1997年，此公司被Sun收购；2009年，Sun公司被Oracle收购。
    - JDK1.3时，HotSpot VM成为默认虚拟机。

- 目前Hotspot占有`绝对`的市场地位，称霸武林
    - 不管是现在仍在广泛使用的JDK6，还是使用比例较多的JDK8中，默认的虚拟机都是Hotspot.
    - Sun/Oracle JDK 和 OpenJDK的默认虚拟机
- 从服务器、桌面到移动端、嵌入式都有应用。
- 名称中的HotSpot指的是它的热点代码探测技术。
    - 通过计数器找到最具编译价值的代码，触发即时编译或栈上替换
    - 通过编译器与解释器协同工作，在最优化的程序响应时间与最佳执行性能中取得平衡。

### BEA的JRockit
- 专注于服务器端应用
    - 它可以不太关注程序启动速度，因此JRockit内部不包含解释器实现，全部代码都靠即时编译器编译后执行。
- 大量的行业基准测试显示，`JRockit是世界上最快的JVM`.
    - 使用JRockit产品，客户已经体验到了显著的性能提高（一些超过了70%）和硬件成本的减少（达50%）。
- 优势：全面的Java运行时解决方案组合。
    - JRockit 面向延迟敏感型应用的解决方案 JRockit Real Time 提供以毫秒或微妙级别的 JVM 响应时间，适合财务、军事指挥、电信网络的需要。
    - MissionControl 服务套件，它是一组以极低的开销来监控、管理和分析生产环境中的应用程序的工具。
- 2008年，BEA 被 Oracle 收购。
- Oracle表达了整合两大优秀虚拟机的工作，大致在JDK8中完成，整合的方式是在 HotSpot 的基础上，移植 JRockit 的优秀特性。
- 高斯林：目前就职于谷歌，研究人工智能和水下机器人。

### IBM 的 J9
- 全称：IBM Technology for Java Virtual Machine,简称IT4J，内部代号：J9。
- 市场定位与 HotSpot 接近，服务器端、桌面应用、嵌入式等多用途 VM
- 广泛用于 IBM 的各种 Java 产品。
- 目前， `有影响力的三大商用虚拟机之一`, 也号称是世界上最快的 Java 虚拟机。
- 2017年左右，IBM 发布了开源 J9 VM，命名为 OpenJ9，交给 Eclipse基金会管理，也称为 Eclipse OpenJ9
