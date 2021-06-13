---
title: "MinorGC_MajorGC_FullGC"
date: 2021-03-31T21:20:51+08:00
draft: false
description: ""
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: Herschel
authorEmoji: 🎅
pinned: false
tags:
- JVM Java
series:
-
categories:
- Java
image:
---

## Major GC、Minor GC、Full GC

JVM 在进行 GC 时，并非每次都对上面三个内存（新生代、老年代、方法区）区域一起回收的，大部分时候回收的都是指新生代。
针对HotSpot VM 的实现，它里面的GC按照回收区域又分为两大种类型：一种是部分收集（Partial GC），一种是整堆收集（Full GC）。

- 部分收集：不是完整收集整个 Java 堆的垃圾收集。其中又分为：
    - 新生代收集（Minor GC/Young GC）：只是新生代（Eden,s0,s1）的垃圾收集。
    - 老年代收集（Major GC/Old GC）：只是老年代的垃圾收集。
        - 目前，只有 CMS GC 会有单独收集老年代的行为。
        - **注意：很多时候 Major GC 会和 Full GC 混淆使用，需要具体分辨是老年代回收还是整堆回收。**
    - 混合收集（Mixed GC）：收集整个新生代以及部分老年代的垃圾收集。
        - 目前，只有G1 GC会有这种行为

- 整堆收集（Full GC）：收集整个 java 堆和方法区的垃圾收集。

## 最简单的分代式GC策略的触发条件

### 年轻代GC（Minor GC）的触发机制

- 当年轻代空间不足时，就会触发Minor GC，这里的年轻代指的是 Eden 代满，Survivor 满不会引发GC。（每次Minor GC 会清理年轻代的内存。）
- 因为 Java 对象大多都具备朝生夕死的特性，所以 Minor GC 非常频繁，一般回收速度也比较快。这一定义既清晰又易于理解。
- Minor GC会引发 STW ，暂停其他用户线程，等垃圾回收结束，用户线程才恢复运行。

### 老年代（Major GC/Full GC）触发机制

- 指发生在老年代的GC，对象从老年代消失时，我们说“Major GC”或者“Full GC”发生了。
- 出现了Major GC，经常会伴随至少一次的Minor GC（但非绝对的，在 Parallel Scavenge 收集器的收集策略里就有直接进行 Major GC 的策略选择过程）。
    - 也就是在老年代空间不足时，会先尝试触发Minor GC。如果之后空间还不足，则触发Major GC。
- Major GC的速度一般会比Minor GC 慢10倍以上，STW的时间更长。
- 如果Major GC后，内存还不足，就报 OOM 了。

### Full GC 触发机制

触发 Full GC 执行的情况有下面五种：
    1. 调用 System.gc() 时，系统建议执行 Full GC，但是不必然执行
    2. 老年代空间不足
    3. 方法区空间不足
    4. 通过 Minor GC 后进入老年代的平均大小大于老年代的可用内存
    5. 由 Eden 区，survivor space0（From Space）区向 survivor space1（To Space）区复制时，对象大小大于 To Space 可用内存，则把该对象转存到老年代，且老年代的可用内存小于该对象大小。
说明：full gc 是开发或调优中尽量要避免的，这样暂时时间短一些。

## 堆空间分代思想

其实不分代完全可以，分代的唯一理由就是**优化GC性能**。如果没有分代，那所有的对象都在一块，就如同把一个学校的人都关在一个教室。GC的时候要找到哪些对象没有用。这样就会对堆的所有区域进行扫描。而很多对象都是朝生夕死的，如果分代的话，把新创建的对象放到某一地方，当GC的时候先把这块存储“朝生夕死”对象的区域进行回收，这样就会腾出很大的空间来。

## 内存分配策略
### 对象提升(Promotion)规则

如果对象在 Eden 出生并经过第一次 MinorGC 之后仍然存活，并且能够被 Survivor 容纳的话，将被移动到 Survivor 空间中，并将对象年龄设为1。对象在 survivor 区中每熬过一次 MinorGC, 年龄就增加1岁，当它的年龄增加到一定程度（默认为15岁，其实每个 JVM、每个 GC 都有所不同）时，就会被晋升到老年代中。
**对象晋升到老年代的年龄阈值，可以通过选项 -XX：MaxTenuringThrehold 来设置。**
