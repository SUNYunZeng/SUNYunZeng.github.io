---
title: Java创建对象设计模式
comments: true
toc: true
date: 2019-09-20 21:20:39
categories: Java
tags: Java设计模式
---

# 1. 使用静态工厂方法代替构造方法

静态工厂方法是一个静态方法，用来生成实例。例如：

```java
// 单例模式
public class Dog{
    private Dog(){

    }
    private static class Inner(){
        private static final Dog dog = new Dog();
    }
    public static getInstance(){
        return Inner.dog;
    }
}
```

因为构造方法每次调用都需要<font color=#f07c82>新建一个对象</font>，有些情况下不能满我们的要求。

而静态工厂方法生成对象有以下几个好处：

1. **名字更有意义。**

+ from —— 类型转换方法，它接受单个参数并返回此类型的相应实例，例如：Date d = Date.from(instant);
+ of —— 聚合方法，接受多个参数并返回该类型的实例，并把他们合并在一起，例如：Set faceCards = EnumSet.of(JACK, QUEEN, KING);
+ valueOf —— from 和 to 更为详细的替代 方式，例如：BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
+ instance 或 getinstance —— 返回一个由其参数 (如果有的话) 描述的实例，但不能说它具有相同的值，例如：StackWalker luke = StackWalker.getInstance(options);
+ create 或 newInstance —— 与 instance 或 getInstance 类似，除此之外该方法保证每次调用返回一个新的实例，例如：Object newArray = Array.newInstance(classObject, arrayLen);
+ getType —— 与 getInstance 类似，但是在工厂方法处于不同的类中的时候使用。getType 中的 Type 是工厂方法返回的对象类型，例如：FileStore fs = Files.getFileStore(path);
+ newType —— 与 newInstance 类似，但是在工厂方法处于不同的类中的时候使用。newType中的 Type 是工厂方法返回的对象类型，例如：BufferedReader br = Files.newBufferedReader(path);
+ type —— getType 和 newType 简洁的替代方式，例如：List litany = Collections.list(legacyLitany);

2. 可以实现**实例数量**的控制。

例如单例、不可实例化类的实现。

3. 返回的对象可以**根据参数不同而不同**。

4. **返回的实例可以没有对应的对象类型。**

例如服务者提供框架，就是用到静态工厂方法。

服务接口，表示实现；提供者注册API，选择实现；服务访问API，客户端调用。服务提供者接口，描述生成服务接口实例的工厂对象。

<font color=#f07c82>依赖注入框架</font>可以被看作强大的服务提供者。

# 2. 当构造函数多时使用**builder**模式

构造函数名字都与类名一样，区分不同构造函数依靠参数的顺序与数量。

当构造参数很多时，实例的生成非常麻烦，往往不知道调用哪个构造函数。

```java
public class NutritionFacts{
    private final int servingSize;
    private final int calories;
    private final int fat;
    public static class Builder{
        private final int servingSize;
        private  int calories;
        private  int fat;

        public Builder(int servingSize){
            this.servingSize = servingSize;
        }

        public Builder calories(int calories){
            calories = calories;
            return this;
        }

        public Builder fat(int fat){
            fat = fat;
            return this;
        }

        public NutritionFacts build(){
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder){
        this.servingSize = builder.servingSize;
        this.calories = builder.calories;
        this.fat = builder.fat;
    }
}

NutritionFacts coca = NutritionFacts.Builder(10).calories(40).fat(0).build();
```

**使用场景：**

当设计类的构造方法或静态工厂的参数超过几个时，Builder 模式是一个不错的选择，特别是如果许多参数是可选的或相同类型的。

3. 单例模式

```java
// 懒汉模式，线程不安全，不是严格意义上单例模式
public class Dog{
    private static final Dog dog;
    private Dog(){

    }
   
    public static getInstance(){
        if(dog==null)
            dog = new Dog();
        return dog;
    }
}

// 饿汉模式，线程安全，但易产生垃圾
public class Dog{
    private static final Dog dog = new Dog();
    private Dog(){

    }
    public static getInstance(){
        return dog;
    }
}

// 内部类方式，线程安全，但是序列化要保证单例，需要重写 readResolve()
public class Dog{
    private Dog(){

    }
    private static class Inner(){
        private static final Dog dog = new Dog();
    }
    public static getInstance(){
        return Inner.dog;
    }

    // 序列化也保证一个单类
    Object readResolve() throws ObjectStreamException{
        return dog;
    }
}

//枚举模式，最简单，但不常用。线程安全，且保证序列化唯一性
public enum Dog{
    INSTANCE;
    public static getInstance(){
        return Dog.INSTANCE;
    }
}
```


