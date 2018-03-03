---
layout:     post
title:      "Java虚拟机中的新生代、老年代、永久代"
subtitle:   ""
date:       2018-03-03
author:     "Kanon"
header-img: "https://images.pexels.com/photos/12628/pexels-photo-12628.jpeg?w=940&h=650&auto=compress&cs=tinysrgb"
tags:
    - Java
---

## 堆为什么要划分新生代老年代？
提高GC的性能。如果没有划分代，所有的对象都在一块，GC的时候就要对所有对象进行扫描，效率及其低下。

## 新生代
新生代分为Eden和Survivor，Survivor又分为From Survivor和 To Survivor。  
JVM 每次只会使用 Eden 和其中的一块 Survivor 区域来为对象服务，所以无论什么时候，总是有一块Survivor区域是空闲着的。  
新生代几乎是所有Java对象出生的地方，就是Java对象申请的内存以及存放的内存都是在这个地方。  
大部分Java对象不需要长久存活，具有朝生夕灭的性质。新生代是GC垃圾收集的频繁区域。 

## 老年代
老年代的生命周期较长，一般都在新生代经过多轮Minor GC的洗礼，默认是15次。  
![新生代老年代](http://ojydvou4n.bkt.clouddn.com/%E6%96%B0%E7%94%9F%E4%BB%A3%E8%80%81%E5%B9%B4%E4%BB%A3.png)

## 永久代
永久代存放的主要是类的信息，在类被加载的时候会被放入永久代，在主程序运行期间永久代是不会被GC清理。  
在Java8中没有永久代这个概念了，取而代之的是元空间。

## GC
GC分为两种：Minor GC、Full GC  
**Minor GC** 是发生在新生代的垃圾收集动作，所采用的是复制算法（内存一分为二，每次只使用其一）。  
**Full GC** 是发生在老年代的垃圾收集动作，所采用的是标记-清除算法或标记整理算法。

## 过程
当对象在Eden出生后，在经过一次Minor GC后，如果还存活着并且能够被To Survivor容纳，则使用复制算法将这些仍然还存活的对象复制到另外一块To Survivor 区域中，然后清理所使用过的 Eden 以及 From Survivor 区域，并且将这些对象的年龄设置为1。这时候Eden和From Survivor都被清理过了。最后，将From和To互换以保证每次To Survivor都是空的。以后对象在 Survivor 区每熬过一次 Minor GC，就将对象的年龄 + 1，当对象的年龄达到某个值时（默认是15岁）就会进入老年代。  
**特殊情况**：对于比较大的对象（即需要分配一块较大的连续内存空间）则是直接进入老年代。  
![复制算法](http://ojydvou4n.bkt.clouddn.com/%E5%A4%8D%E5%88%B6%E7%AE%97%E6%B3%95.png)
