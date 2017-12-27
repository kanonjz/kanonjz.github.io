---
layout:     post
title:      "可重入锁与自旋锁"
subtitle:   ""
date:       2017-12-27
author:     "Kanon"
header-img: "https://images.pexels.com/photos/744974/pexels-photo-744974.jpeg?w=940&h=650&auto=compress&cs=tinysrgb"
tags:
    - Java
    - 并发
---

要讲可重入锁，还得先从不可重入锁讲起。先来看不可重入锁的例子：
### 不可重入锁
我们自己定义的不可重入锁：
```
public class Lock{
    private boolean isLocked = false;
    //加锁
    public synchronized void lock() throws InterruptedException{
        //为了防止假唤醒，保存信号的成员变量将在一个while循环里接受检查，而不是在if表达式里
        while(isLocked){    
            wait();
        }
        isLocked = true;
    }
    //释放锁
    public synchronized void unlock(){
        isLocked = false;
        notify();
    }
}
```
使用可重入锁：
```
public class TestUnreetrantLock{
    Lock lock = new Lock();
    public void outer(){
        lock.lock();
        inner();
        lock.unlock();
    }
    public void inner(){
        lock.lock();
        //do something
        lock.unlock();
    }
}
```
在这个例子中我们可以看到，一旦外层函数outer加锁之后，内层函数inner就无法再继续加锁，线程直接进入WAITING的状态，变成了自己将自己死锁的情况。

与不可重入锁相反，则是可重入锁。

### 可重入锁
含义：在同一线程中，外层函数获取了锁之后，内层函数依然可以获得相同的锁

作用：防止死锁

Java里常见的可重入锁：`synchronized`、`ReentrantLock`

`synchronize`的用法参考另一篇文章[synchronized关键字的三种用法](https://kanonjz.github.io/2017/12/09/synchronized/)

### 自旋锁
含义：在获得不到锁的情况下，会让当前线程一直做空循环，避免进入阻塞状态
```
public class SpinLock {
    private AtomicReference<Thread> owner =new AtomicReference<>();
    private int count =0;
    public void lock(){
        Thread current = Thread.currentThread();
        if(current==owner.get()) {
            count++;
            return ;
        }
        //compareAndSet(Thread expect, Thread update)
        //自旋锁的自旋就是体现在这里
        while(!owner.compareAndSet(null, current)){

        }
    }
    public void unlock (){
        Thread current = Thread.currentThread();
        if(current==owner.get()){
            if(count!=0){
                count--;
            }else{
                owner.compareAndSet(current, null);
            }

        }

    }
}
```

注意两点：
- 为了避免不可重入，在lock的时候需要判断操作的是否是同一线程
- 为了避免第一次unlock的时候就将锁释放掉了，采用计数进行统计（有点类似信号量的方式）

## 延伸知识点
`Lock`相比`synchronized`的三个优点：
1. 等待可中断
2. 支持公平锁
3. 锁可绑定多个条件

## 参考文章
cnblogs：[可重入锁 & 自旋锁 & Java里的AtomicReference和CAS操作 & Linux mutex不可重入](https://www.cnblogs.com/charlesblc/p/6188364.html)  
知乎：[java的可重入锁用在哪些场合？](https://www.zhihu.com/question/23284564)  
ifeve：[Java锁的种类以及辨析（四）：可重入锁](http://ifeve.com/java_lock_see4/)  
cnblogs：[锁的简单应用](https://www.cnblogs.com/dj3839/p/6580765.html)  
<br><br><br><br>
