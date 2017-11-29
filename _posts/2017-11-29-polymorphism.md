---
layout:     post
title:      "浅谈多态"
subtitle:   ""
date:       2017-11-29
author:     "Kanon"
header-img: "https://images.pexels.com/photos/147357/pexels-photo-147357.jpeg?w=1260&h=750&auto=compress&cs=tinysrgb"
tags:
    - Java
    - C++
---

## 多态性
> **本质**：一个接口（名字），多种实现

**C++中的体现：一个基类的指针（或引用）在调用基类的虚函数时，最终调用的却是派生类中的同名虚函数**  
```
#include<iostream>

using namespace std;

class animal{
	public:
		virtual void what(){
			cout<<"this is animal"<<endl;
		}
};

class tiger:public animal{
	public:
		void what() override{
			cout<<"this is tiger"<<endl;
		}
};

int main()
{
	tiger t;
	animal& a=t;
	a.what();
	return 0;
}
```
output
```
this is tiger
```

**JAVA中的体现：某一对象在调用它的方法的时候，最终调用的却是派生类中的同名方法**
```
public class TestBikes {
  public static void main(String[] args){
    Bicycle bike01, bike02, bike03;

    bike01 = new Bicycle(20, 10, 1);
    bike02 = new MountainBike(20, 10, 5, "Dual");
    bike03 = new RoadBike(40, 20, 8, 23);

    bike01.printDescription();
    bike02.printDescription();
    bike03.printDescription();
  }
}
```
output
```
Bike is in gear 1 with a cadence of 20 and travelling at a speed of 10. 

Bike is in gear 5 with a cadence of 20 and travelling at a speed of 10. 
The MountainBike has a Dual suspension.

Bike is in gear 8 with a cadence of 40 and travelling at a speed of 20. 
The RoadBike has 23 MM tires.
```
<hr>
多态性包括静态多态性和动态多态性

### 静态多态性  
也就是普通的函数重载（包括运算符函数重载），在编译时完成  
举例：有如下函数声明
```
void f();
void f(int x);
void f(double x);
void f(int x, int y);
```
对应的函数调用
```
f();
f(1);
f(2.0);
f(1,2);
```

### 动态多态性
函数的调用被推迟到运行时确定

### 注意
- 在代码中必须使用基类的指针/引用去访问虚函数，否则多态性将失去意义
- 多态性必须建立在继承的基础上，静态多态性是通过重载来实现，动态多态性是通过重写（覆盖）来实现。
- fianl关键字表明是最终覆盖版本，其派生类不能再去覆盖
- override关键字表明成员是用于覆盖，不是强制的，但便于编译器发现错误

### 参考资料
[polymorphism](https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html)
