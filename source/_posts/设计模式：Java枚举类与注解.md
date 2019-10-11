---
title: 设计模式：Java枚举类与注解
comments: true
toc: true
date: 2019-09-30 22:45:23
categories: Java
tags: Java设计模式
---

# 枚举类替代整形常量

枚举类的基本形式如下：
```java
public enum Apple  { FUJI, PIPPIN, GRANNY_SMITH }
public enum Orange { NAVEL, TEMPLE, BLOOD }
```

<font color=#f07c82>enum</font>关键字指明一个类继承 <font color=#f07c82>abstract</font>Enum。

该类有两个重要的方法：

1. <font color=#f07c82>ordinal()</font>. 返回整形序列值（从0开始）
```java
public enum Season{
    SPRING, SUMMER, FALL, WINTER
}

System.out.println(Season.SPRING.ordinal())

// 打印 0
```

2. <font color=#f07c82>Enum()</font>. 只能被继承Enum的类调用的构造方法，枚举类中每个实例调用该方法实现赋值，每个实例的构造方法都是私有的。
```java
protected Enum(String name, int ordinal) {
        this.name = name;
        this.ordinal = ordinal;
    }
```

**枚举类的特征**
**1. 不能被继承其它类也不能被其他类继承，但可以实现接口，对枚举类内部实例进行组织。**
```java
interface Info{
    public String getName();
}

public enum Season implements Info{
    SPRING("春天"), SUMMER("夏天"), FALL("秋天"), WINTER("冬天");
    // 必须为private的构造器，因为枚举类本身是final的，其内部实例也是final，且在枚举类生成同时调用私有的构造器实现值的赋予
    private String name;
    private Season(){}
    private Season(String name){
        this name = name;
    }

    // 实现接口，每个实例都有该接口，可以返回对应的中文名称
    public String getName(
        return name;
    );

    @test
    System.out.println(Season.SPRING.getName());
    // 打印 春天
}
```

**2. 类中每个实例可以重写其中定义的抽象方法**

```java
public enum Season implements{
    SPRING(public String getName(return "春天"), 
    SUMMER(public String getName(return "夏天")), 
    FALL(public String getName(return "秋天")), 
    WINTER(public String getName(return "冬天"));
    // 必须为每个实例都重写抽象方法，否则报错
    private String name;
    private Season(){}
    private Season(String name){
        this name = name;
    }

    // 实现接口，每个实例都有该接口，可以返回对应的中文名称
    public abstract String getName();

    @test
    System.out.println(Season.SPRING.getName());
    // 打印 春天
}
```

**3. 可利用枚举实现单例**
```java
public enmu Sigleton{
    SINGLETON;
    public String method(){
        return "method";
    }
}

@test
System.out.println(Singleton.SINGLETON.method())
// 打印 method
```

**4. 调用values()方法遍历枚举类型中的每个实例**
```java
for(Season season: Season.values){
    System.out.println(season.getName());
}
```