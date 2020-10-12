# **Java 23种设计模式**

# 一、七项基本原则

## 1.开闭原则

封闭修改，开放扩展

## 2.里氏替换原则

并非所有的子类都适合重写父类或者超类的方法

例子：

父类：鸟

子类：几维鸟（由于退化，几维鸟已经不能飞了）

子类：燕子

这个时候我们不再适用继承父类鸟

而应该继承动物类animal

它们都有一个共性方法，移动速度 runSpeed

## 3.依赖倒置原则

0.0没啥好记的

## 4.单一职责原则

又称为单一功能原则

例子：

学生工作=》

生活辅导=》辅导员

学业指导=》学业导师

​	辅导员:	班委建设/出勤统计/心理辅导/费用催缴/班级管理

学业导师： 专业指导/学习辅导/科研指导/学习总结

## 5.接口隔离原则

定义三个模块接口

//输入

InputModule=》insert(); delete(); modify();

//统计

CountModule

打印

PrintModule

实现多个接口用**逗号**隔开

实现方法的返回值： return (InputModule)  new StuScoreList();

这样一来，创建对象就可以调用相应模块的方法了

## 6.迪米特法则

降低耦合度

## 7.合成复用原则

优先使用组合或者聚合关系复用，少用继承关系服用。

总结：**开闭原则是总纲**，它告诉我们要对扩展开放，对修改关闭，**里氏替换原则**告诉我们不要破坏继承体系；**依赖倒置原则**告诉我们要面向接口编程；**单一职责原则**告诉我们实现类要职责单一；**接口隔离原则**告诉我们在设计接口的时候要精简单一；**迪米特法则（最少知识）**告诉我们要降低耦合度；**合成复用原则**告诉我们要优先使用组合或者聚合关系复用，少用继承关系。

# UML类图

![image-20200712114423884](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200712114423884.png)

# 单例模式（1）

**1 懒汉模式（类加载时不初始化）**

```java
public class LazySingleton {
    //懒汉式单例模式
    //比较懒，在类加载时，不创建实例，因此类加载速度快，但运行时获取对象的速度慢

    private static LazySingleton intance = null;//静态私用成员，没有初始化
    
    private LazySingleton()
    {
        //私有构造函数
    }
    
    public static synchronized LazySingleton getInstance()    //静态，同步，公开访问点
    {
        if(intance == null)
        {
            intance = new LazySingleton();
        }
        return intance;
    }
}
```

1）构造函数定义为私有----不能在别的类中来获取该类的对象，只能在类自身中得到自己的对象

2）成员变量为static的，没有初始化----类加载快，但访问类的唯一实例慢，static保证在自身类中获取自身对象

3）公开访问点getInstance： public和synchronized的-----public保证对外公开，同步保证多线程时的正确性（因为类变量不是在加载时初始化的）

优缺点见代码注释。

**2 饿汉式单例模式（在类加载时就完成了初始化，所以类加载较慢，但获取对象的速度快）**

```java
public class EagerSingleton {
    //饿汉单例模式
    //在类加载时就完成了初始化，所以类加载较慢，但获取对象的速度快
    //保证 istance 在线程中同步
    
    private static volatile EagerSingleton instance = new EagerSingleton();//静态私有成员，已初始化
    
    private EagerSingleton() 
    {
        //私有构造函数
    }
    
    public static EagerSingleton getInstance()    //静态，不用同步（类加载时已初始化，不会有多线程的问题）
    {
        return instance;
    }
}
```

1）私有构造函数

2）静态私有成员--在类加载时已初始化

3）公开访问点getInstance-----不需要同步，因为在类加载时已经初始化完毕，也不需要判断null，直接返回

优缺点见代码注释。

# 原型模式（2）

```java
/**
* 具体原型类
*/
public class Realizetype implements Prototype{
    Realizetype(){
        sout("具体原型创建成功");
    }
    
    public Object clone () throws CloneNotSupportedException{
        Sout("具体原型赋值成功");
        return (Realizetype) super.clone();//这里是super.clone()浅克隆，由于没有成员变量，这里也算是个深克隆
    }
    
}

//原型类接口
public interface Prototype extends Cloneable{
    public Object clone() throws CloneNotSupportedException;
}
```

```java
1）用法
implements Cloneable

重写父类方法

public Object clone() throws CloneNotSupportException{

	return (Realizetype) super.clone();

}
```

# Builder Pattern

![Builder Pattern](D:\Typera\img\建造者模式结构图.png)

# 适配器模式

类适配器：继承适配者类实现目标接口，重写接口方法

创建目标接口，实例化适配者类