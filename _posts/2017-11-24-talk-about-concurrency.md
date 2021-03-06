---
layout:     post
title:      "浅谈并发控制"
subtitle:   ""
date:       2017-11-24
author:     "Kanon"
header-img: "https://images.pexels.com/photos/681637/pexels-photo-681637.jpeg?w=940&h=650&auto=compress&cs=tinysrgb"
tags:
    - Java
    - 并发
---

并发控制：既可以在数据库层面上进行控制，也可以在java代码里进行控制

### 事务并发控制
在数据库层面上的控制就是事务的并发控制。事务并发执行如果不加控制，可能会遇到各种问题，比如脏读、不可重复读、幻象读，事务并发控制的手段有锁机制（共享锁、排它锁、行锁、表锁、乐观锁），也可以设置事务的隔离级别，或者采用加锁协议（两阶段加锁协议可以实现事务的串行化调度）。在Spring里还可以设置传播行为（Propagation），传播行为是指事务的调用策略。

### 线程安全控制
在Java代码层面上的控制就是线程安全的控制。主要有三种线程安全的实现方法：

第一种方法是`互斥同步`，也就是所谓的通过加锁来控制，synchronized关键字和Lock都是常见的“锁”，再搭配上volatile来使用，volatile可以保证变量的可见性和禁止指令重排序（当然synchronized也具有可见性）。而Lock与synchronized相比主要有三点优势：1）等待可中断 2）可实现公平锁 3）锁可绑定多个条件。


第二种方法是`非阻塞同步`，这是一种乐观的策略，就是先进行操作，如果没有其它线程进行争用共享数据，那操作就成功了；如果共享数据有争用，产生了冲突，那就采取补偿措施（最常见的补偿措施是不断地重试，直到成功为止），这种乐观的并发策略的实现不需要把线程挂起，因此称为非阻塞同步。典型的例子，`java.util.concurrent.atomic`包下面的`AotmicInteger`有一个方法`incrementAndGet`，它采取的就是非阻塞同步的策略，代码如下:
```
public final int incrementAndGet() {
    for (;;) {
        int current = get();
        int next = current + 1;
        if (compareAndSet(current, next))
            return next;
    }
}
```

第三种是`无同步方案`。这种方案不涉及共享数据，典型的例子是通过`java.lang.ThreadLocal`类来实现线程本地存储的功能，本质上就是为每个线程分配单独的对象。

### 延伸
- volatile使用场景：在两个或者更多的线程访问的成员变量上使用volatile，表明这个变量是易变的。当要访问的变量已在synchronized代码块中，或者为常量时，则不必使用。
- 非阻塞同步的实现本质上基于 CAS + volatile（参照AotmicInteger）

<br><br><br><br>
