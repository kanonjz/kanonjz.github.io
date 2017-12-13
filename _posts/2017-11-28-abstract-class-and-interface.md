---
layout:     post
title:      "Java中的抽象类与接口对比"
subtitle:   ""
date:       2017-11-28
author:     "Kanon"
header-img: "https://images.pexels.com/photos/175680/pexels-photo-175680.jpeg?w=940&h=650&auto=compress&cs=tinysrgb"
tags:
    - Java
---

## 抽象类与接口

### 抽象方法
要讲抽象类与接口，我们得先从抽象方法说起。  
来看官方文档的定义:  
An abstract method is a method that is declared without an implementation (without braces, and followed by a semicolon), like this:
```
abstract void moveTo(double deltaX, double deltaY);
```
翻译过来就是一个抽象方法是一个没有被实现的方法，没有大括号，用分号结尾。

### 抽象类
再来看官方定义：

An abstract class is a class that is declared abstract—it may or may not include abstract methods. Abstract classes cannot be instantiated, but they can be subclassed.  
If a class includes abstract methods, then the class itself must be declared abstract, as in:
```
public abstract class GraphicObject {
   // declare fields
   // declare nonabstract methods
   abstract void draw();
}
```
When an abstract class is subclassed, the subclass usually provides implementations for all of the abstract methods in its parent class. However, if it does not, then the subclass must also be declared abstract.

总结几点：
1. 抽象类用`abstract`关键字来声明
2. 抽象类中可以没有抽象方法，但如果一个类中有抽象方法那这个类一定要声明为抽象类
3. 抽象类不能被实例化，但能被继承
4. 如果一个抽象类被继承了，在派生类中一般会去实现抽象方法，而未被实现的抽象方法仍需要用`abstract`来修饰
5. 一个类只能继承（`extends`）一个抽象类
6. 抽象方法可以有public、protected这些修饰符，而接口里只能是public
7. 抽象类可以有构造器，接口不能有构造器

### 接口
看官方定义：  

an interface is a group of related methods with empty bodies. A bicycle's behavior, if specified as an interface, might appear as follows:
```
interface Bicycle {

    //  wheel revolutions per minute
    void changeCadence(int newValue);

    void changeGear(int newValue);

    void speedUp(int increment);

    void applyBrakes(int decrement);
}
```
To implement this interface, the name of your class would change (to a particular brand of bicycle, for example, such as ACMEBicycle), and you'd use the implements keyword in the class declaration:
```
class ACMEBicycle implements Bicycle {

    int cadence = 0;
    int speed = 0;
    int gear = 1;

   // The compiler will now require that methods
   // changeCadence, changeGear, speedUp, and applyBrakes
   // all be implemented. Compilation will fail if those
   // methods are missing from this class.

    void changeCadence(int newValue) {
         cadence = newValue;
    }

    void changeGear(int newValue) {
         gear = newValue;
    }

    void speedUp(int increment) {
         speed = speed + increment;   
    }

    void applyBrakes(int decrement) {
         speed = speed - decrement;
    }

    void printStates() {
         System.out.println("cadence:" +
             cadence + " speed:" + 
             speed + " gear:" + gear);
    }
}
```
Interfaces form a contract between the class and the outside world, and this contract is enforced at build time by the compiler. If your class claims to implement an interface, all methods defined by that interface must appear in its source code before the class will successfully compile.

总结几点：
1. 接口定义了一套契约
2. 接口包含一个或多个抽象方法
4. 接口中所有的成员（`field`）都是public、static、final，接口中的方法都是public
3. 如果一个类继承了某个接口，那它就得实现接口里的所有的方法
4. 一个类可以继承（`implements`）多个接口

## 什么时候使用抽象类和接口
1. 如果你拥有一些方法并且想让它们中的一些有默认实现，那么使用抽象类。  
2. 想实现多继承必须使用接口  
3. 基本功能经常改变的话使用抽象类，因为如果使用接口那么就需要改变所有实现了该接口的类（这里的改变针对的是抽象类中被继承的默认实现）  


## 参考
importNew：[Java抽象类与接口的区别](http://www.importnew.com/12399.html)  

stackoverflow：[Why are all fields in an interface implicitly static and final?](https://stackoverflow.com/questions/1513520/why-are-all-fields-in-an-interface-implicitly-static-and-final)

<br><br><br><br>
