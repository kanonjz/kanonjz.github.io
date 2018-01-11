---
layout:     post
title:      "浅谈C++中的复制构造函数"
subtitle:   ""
date:       2018-01-11
author:     "Kanon"
header-img: "https://images.pexels.com/photos/754339/pexels-photo-754339.jpeg?w=940&h=650&auto=compress&cs=tinysrgb"
tags:
    - C++
---

### 什么是复制构造函数？
- 使用已存在的对象作为新对象的复制模板
- 如果不自己定义复制构造函数，编译器会自动为该类生成`隐式缺省复制构造函数`
- `隐式缺省复制构造函数`采用逐成员的方式，将参数对象的成员（除了静态成员）一个个复制到发起复制的对象的对应成员中。这样一来，两个对象的成员内容是一模一样的。

### 为什么需要自己定义复制构造函数
因为`隐式缺省复制构造函数`往往不能满足我们的实际需求。  
举个例子，比如说有个静态变量counter，记录被创建的对象个数。每调用一次构造函数都会将counter加一。如果使用复制的方法来创建对象，构造函数是不会被调用的，而是使用`隐式缺省复制构造函数`，因而counter也不会加一。

### 什么时候会调用复制构造函数
- 显示定义复制对象，如下所示：
```
array a1;
array a2(a1);   //调用array的复制构造函数
array a3 = a2;  //注意这种情况，同样也是复制
```
- 实参和形参相结合时
- 函数返回一个临时对象时
<br>

根据复制时对资源的处理情况，复制可以分为“浅复制”和“深复制”。
### 浅复制
使两个对象的指针指向相同的资源。例子：
```
class array {
    public:
        //capacity和len都是int类型，head是int*类型
        array(const array& a):capacity(a.capacity), len(a.len), head(a.head){}
}
```
从例子中可以看出，罪魁祸首是在head的复制，这里仅仅让两个指针指向了相同的资源。

### 深复制
为复制目标制作一份资源副本，两个对象不会共享资源。例子：
```
class array {
    public:
        arrray(const array& a):capacity(a.capacity), len(0), head(new int[a.capacity]){
            for(int i = 0;i < a.len;i++) {
                push_back(a.head[i]);
            }
        }
}
```
从例子中可以看出，深复制是复制了一份资源副本。
<br><br><br><br>
