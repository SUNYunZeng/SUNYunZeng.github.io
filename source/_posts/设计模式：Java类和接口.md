---
title: 设计模式：Java类和接口
comments: true
toc: true
date: 2019-09-23 19:52:56
categories: Java
tags: Java设计模式
---

# 使类与成员可访问性最小

## 成员（类、接口、方法、字段）的访问级别

+ <font color=#f07c82>private</font> —— 该成员只能在声明它的顶级类内访问。

+ <font color=#f07c82>package-private</font> —— 成员可以从被声明的包中的任何类中访问。从技术上讲，如果没有指定访问修饰符（接口成员除外，它默认是公共的），这是默认访问级别。

+ <font color=#f07c82>protected</font> —— 成员可以从被声明的类的子类中访问（会受一些限制 [JLS, 6.6.2]），以及它声明的包中的任何类。

+ <font color=#f07c82>public</font> —— 该成员可以从任何地方被访问。

## 公共类的实例字段很少情况下采用 public 修饰

如果需要**调用私有成员**，就**写一个方法**。

```java
private static final Thing[] PRIVATE_VALUES = { ... };

public static final Thing[] values() {
    return PRIVATE_VALUES.clone();
}
```

# 公共类中使用访问方法而不是访问属性

虽然包内访问权限，如果只包内可见是可行的，也可以，同时可以避免视觉混乱，但是不提倡这样做。

```java
// Degenerate classes like this should not be public!
class Point {
    public double x;
    public double y;
}
```

**提供提供方法与设置方法**
```java
// Encapsulation of data by accessor methods and mutators
class Point {
    private double x;
    private double y;

    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }

    public double getY() { return y; }

    public void setX(double x) { this.x = x; }

    public void setY(double y) { this.y = y; }

}
```

# 不变类

不变类中<font color=#f07c82>所有实例的信息在生命周期中都是固定的</font>，因此是安全可靠的。

## 不变类的设计规则

+ <font color=#f07c82>不要提供修改对象状态的方法</font> （也称为 mutators）。

+ <font color=#f07c82>确保这个类不能被继承。</font>  这可以防止粗心的或恶意的子类，假设对象的状态已经改变，从而破坏类的不可变行为。 防止子类化通常是通过 final 修饰类，但是我们稍后将讨论另一种方法。

+ <font color=#f07c82>把所有属性设置为 final。</font>  通过系统强制执行，清楚地表达了你的意图。 另外，如果一个新创建的实例的引用从一个线程传递到另一个线程而没有同步，就必须保证正确的行为。

+ <font color=#f07c82>把所有的属性设置为 private。</font>  这可以防止客户端获得对属性引用的可变对象的访问权限并直接修改这些对象。 虽然技术上允许不可变类具有包含基本类型数值的公共 final 属性或对不可变对象的引用，但不建议这样做，因为它不允许在以后的版本中更改内部表示。

+ <font color=#f07c82>确保对任何可变组件的互斥访问。</font>  如果你的类有任何引用可变对象的属性，请确保该类的客户端无法获得对这些对象的引用。 切勿将这样的属性初始化为客户端提供的对象引用，或从访问方法返回属性。 在构造方法，访问方法和 readObject 方法中进行防御性拷贝。

# 多使用组合而不是继承

如果父类不是为了继承而专门设计，那么后面父类的修改可能会导致子类产生严重的bug。

明智的方法是多使用组合<font color=#f07c82>composition</font>。即在类内部使用其它类的引用。

一个<font color=#f07c82>新类</font>由一系列<font color=#f07c82>现有类</font>组成，**新类的方法通过调用现有类的方法获取结果**，成为<font color=#f07c82>转发 (farwarding)</font>。

# 继承需要深思熟虑还要详细的设计文档

测试为继承而设计的类的唯一方法是<font color=#f07c82>编写子类</font>。

**构造方法绝不能直接或间接调用可重写的方法。**

For example:
```java
public class Super {
    // Broken - constructor invokes an overridable method
    public Super() {
        overrideMe();
    }
    public void overrideMe() {
    }
}

public final class Sub extends Super {
    // Blank final, set by constructor
    private final Instant instant;

    Sub() {
        instant = Instant.now();
    }

    // Overriding method invoked by superclass constructor
    @Override 
    public void overrideMe() {
        System.out.println(instant);
    }

    public static void main(String[] args) {
        Sub sub = new Sub();
        sub.overrideMe();
    }
}
```
上述子类由于重写了方法，调用父类时 instant 成员没有指向任何一个对象，因此有问题。

专门为了继承而设计类是一件很辛苦的工作。你必须建立文档说明其所有的自用模式，并且一旦建立了文档，在这个类的整个生命周期中都必须遵守。如果没有做到，子类就会依赖父类的实现细节，如果父类的实现发生了变化，它就有可能遭到破坏。为了允许其他人能编写出高效的子类，还你必须导出一个或者多个受保护的方法。除非知道真正需要子类，否则最好通过将类声明为 <font color=#f07c82>final</font>，或者确保没有可访问的构造器来禁止类被继承。