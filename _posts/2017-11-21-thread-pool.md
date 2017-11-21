---
layout:     post
title: "Java线程池"
subtitle:   ""
date:       2017-11-21
author:     "Kanon"
header-img: "https://images.pexels.com/photos/656195/pexels-photo-656195.jpeg?w=1260&h=750&auto=compress&cs=tinysrgb"
catalog: false
tags:
    - Java
---

# 线程池
顾名思义，就是存放线程的地方。如果没有线程池，我们在使用线程时需要去创建，使用完后还得销毁，整个过程是很耗费时间和资源的。有了线程池之后，就可以直接从线程池中获得线程。简单地理解，ExecutorService的实例就是线程池。

## 关键接口和类
#### Executor
是Java线程池的超级接口，提供一个execute(Runnable command)方法，我们一般用它的继承接口ExecutorService。

#### ExecutorService
ExecutorService的实例就是线程池。它是线程池定义的一个接口，继承Executor。我们可以很方便地使用Executors工厂类来帮我们创建ExecutorService的实例。  
`ExecutorService exe=Executors.newCachedThreadPool();`

#### 关系
Executor接口 <—— ExecutorService接口 <—— AbstractExecutorService 抽象类 <—— ThreadPoolExecutor  
```
public class ThreadPoolExecutor extends AbstractExecutorService
```

## 两种方式创建线程池
1. 使用`ThreadPoolExecutor`来创建
```
ThreadPoolExecutor exec = new ThreadPoolExecutor(5, 10, 200, TimeUnit.MILLISECONDS, new ArrayBlockingQueue<Runnable>(5));
```

2. 使用`Executors`工厂类来创建(java doc提倡)
```
ExecutorService exec = Executors.newFixedThreadPool(5);
```

## 使用`ThreadPoolExecutor`来创建
#### 构造参数
- `corePoolSize`：核心池的大小，默认情况下，在创建了线程池后，线程池中的线程数为0，当有任务来之后，就会创建一个线程去执行任务，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中

- `maximumPoolSize`：线程池能创建的最大线程数

- `keepAliveTime`：表示线程没有任务执行时最多保持多久时间会终止。默认情况下，只有当线程池中的线程数大于corePoolSize时，keepAliveTime才会起作用，直到线程池中的线程数不大于corePoolSize

- `unit`：参数keepAliveTime的时间单位

- `workQueue`：一个阻塞队列，用来存储等待执行的任务，有3种常见阻塞队列

- `threadFactory`：线程工厂，用来创建线程

- `handler`：当拒绝处理任务时采取的策略


## 使用`Executors`工厂类来创建
#### Executors工厂类
是java.util.concurrent包下的一个工厂类，提供各种静态方法，可以用来创建不同类型的线程池。

#### Executors可以创建以下四种不同类型的线程池
 - `newCachedThreadPool`：创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。
 - `newFixedThreadPool`：创建一个定长的线程池，可控制线程最大并发数，超出的线程会在队列中等待。
 - `newSingleThreadExecutor`：相当于创建一个线程数量为1的`newFixedThreadPool`，每次只能执行一个任务，多的任务将进行排队，
 - `newScheduledThreadPool`：创建一个线程池，可以在延迟后执行命令，或者定期地执行

## 重要方法
#### execute()
向线程池提交一个任务，交由线程池去执行。

#### submit()
这个方法也是用来向线程池提交任务的，但是它和execute()方法不同，它能够返回任务执行的结果

#### shutdown()
不再接受新任务，等待已有的任务执行完毕，所有在shutdown()执行之前提交的任务都会被执行。

#### shutdownNow()
不再接受新的任务，并且会尝试终止正在执行的任务。 

## 线程池的状态
- 当线程池创建后处于`running`状态
- 调用shutdown之后处于`shutdown`状态
- 调用shutdownNow后处于`stop`状态

## 当有任务提交到线程池之后的一些操作
1. 若当前线程池中线程数<corepoolsize，则每来一个任务就创建一个线程去执行。 

2. 若当前线程池中线程数>=corepoolsize，会尝试将任务添加到任务缓存队列中去，若添 加成功，则任务会等待空闲线程将其取出执行，若添加失败，则尝试创建线程去执行这个任 务。 

3. 若当前线程池中线程数>= Maximumpoolsize，则采取拒绝策略（有 4 种）  
    1）`abortpolicy` 丢 弃任务，抛出 RejectedExecutionException  
    2）`discardpolicy` 拒绝执行，不抛异常  
    3）`discardoldestpolicy` 丢弃任务缓存队列中最老的任务，并且尝试重新提交新的任务  
    4）`callerrunspolicy` 有反馈机制，使任务提交的速度变慢  

## 参考
ImportNew:[深入理解Java之线程池](http://www.importnew.com/19011.html)






