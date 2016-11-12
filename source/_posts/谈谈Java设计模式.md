title: 谈谈 Java 设计模式
date: 2015-12-24 13:18:20
description: 学习Java设计模式
categories: Back End
tag: Java
---

## 前言

### 为什么要使用

一个程序员对设计模式的理解: 不懂为什么要把很简单的东西搞得那么复杂。后来才开始明白我所看到的“复杂”恰恰就是设计模式的精髓所在，我所理解的“简单”就是一把钥匙开一把锁的模式，目的仅仅是着眼于解决现在的问题，而设计模式的“复杂”就在真正理解设计模式之前我一直在编写“简单”的代码。

### 目录

单例模式（Singleton）
适配器模式 （Adapter）
<!-- more -->
## 单例模式（Singleton）

### 是什么

Singleton 是一种创建性模型，它用来确保只产生一个实例，并提供一个访问它的全局访问点。对一些类来说，保证只有一个实例是很重要的，比如有的时候，数据库连接或 `Socket` 连接要受到一定的限制，必须保持同一时间只能有一个连接的存在。再举个例子，集合中的 `Set` 中不能包含重复的元素，添加到 `Set` 里的对象必须是唯一的，如果重复的值添加到 `Set` ，它只接受一个实例。JDK中正式运用了单例模式来实现 `Set` 的这一特性，大家可以查看 `java.util.Collections` 里的内部静态类 `SingletonSet` 的原代码。其实单例是最简单但也是应用最广泛的模式之一，在 JDK 中随处可见。

### 饿汉式单例

```java
public class Singleton{
	private Singleton(){};
	private static Singleton single = new Singleton();
	public static Singleton getInstance(){
		return single;
	}
}
```

### 懒汉式单例

```java
public class Singleton{
	private Singleton(){};
	private Singleton single = null;
	public static Singleton getInstance(){
		if(single == null){
			single = new Singleton();
		}
		return single;
	}
}
```

### 区别

都是通过使用 `private` 访问修饰符，将类的构造器指定为 `private` ，从而无法创建该类的对象了，除非是使用 `Singleton.getInstance()` .

可以看到懒汉式是等到要使用这个对象时再创建的；而饿汉式在类加载时对象就被实例化，从资源利用来看，效率比懒汉更低。

懒汉是线程不安全的，在并发环境下可能出现多个实例；而饿汉是天生安全的。

## 适配器模式（Adapter）

> 更多参考 http://www.cnblogs.com/java-my-life/archive/2012/04/13/2442795.html

### 是什么

适配器模式把一个类的接口变换成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起工作的两个类能够在一起工作。

### 类适配器模式

![类适配器模式](http://7xoxnz.com1.z0.glb.clouddn.com/classAdapter.png)

```java
interface Target {
    //这是源类Adaptee也有的方法
    public void sampleOperation1(); 

    //这是源类Adapteee没有的方法
    public void sampleOperation2(); 
}

class Adaptee {
    
    public void sampleOperation1(){}

}

public class Adapter extends Adaptee implements Target {
    /**
     * 由于源类Adaptee没有方法sampleOperation2()
     * 因此适配器补充上这个方法
     */
    @Override
    public void sampleOperation2() {
        //写相关的代码
    }

}
```

### 对象适配器模式

![对象适配器模式](http://7xoxnz.com1.z0.glb.clouddn.com/objAdapter.png)

```java
interface Target {
    //这是源类Adaptee也有的方法
    public void sampleOperation1();

    //这是源类Adapteee没有的方法
    public void sampleOperation2(); 
}

class Adaptee {

    public void sampleOperation1(){}
    
}

public class Adapter {
    private Adaptee adaptee;
    
    public Adapter(Adaptee adaptee){
        this.adaptee = adaptee;
    }
    /**
     * 源类Adaptee有方法sampleOperation1
     * 因此适配器类直接委派即可
     */
    public void sampleOperation1(){
        this.adaptee.sampleOperation1();
    }
    /**
     * 源类Adaptee没有方法sampleOperation2
     * 因此由适配器类需要补充此方法
     */
    public void sampleOperation2(){
        //写相关的代码
    }
}
```

### 区别

类适配器使用继承，是静态的；对象适配器使用组合，是动态的。

类适配器不能与Adaptee的子类一起工作；对象适配器可以。

类适配器可以覆盖Adaptee的一些接口；对象适配器则需要定义Adaptee的子类，然后使用组合。

建议尽量使用对象适配器，在继承前先考虑组合。具体问题具体分析也很重要，最适合的才是最好的。

