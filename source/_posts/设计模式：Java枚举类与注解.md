---
title: 设计模式：Java枚举类与注解
comments: true
toc: true
date: 2019-09-30 22:45:23
categories: Java
tags: 设计模式
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

**嵌套枚举**

```java
// The strategy enum pattern
enum PayrollDay {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY,
    SATURDAY(PayType.WEEKEND), SUNDAY(PayType.WEEKEND);

    private final PayType payType;

    PayrollDay(PayType payType) { this.payType = payType; }
    PayrollDay() { this(PayType.WEEKDAY); }  // Default

    int pay(int minutesWorked, int payRate) {
        return payType.pay(minutesWorked, payRate);
    }

    // The strategy enum type
    private enum PayType {
        WEEKDAY {
            int overtimePay(int minsWorked, int payRate) {
                return minsWorked <= MINS_PER_SHIFT ? 0 :
                  (minsWorked - MINS_PER_SHIFT) * payRate / 2;
            }
        },
        WEEKEND {
            int overtimePay(int minsWorked, int payRate) {
                return minsWorked * payRate / 2;
            }
        };

        abstract int overtimePay(int mins, int payRate);
        private static final int MINS_PER_SHIFT = 8 * 60;

        int pay(int minsWorked, int payRate) {
            int basePay = minsWorked * payRate;
            return basePay + overtimePay(minsWorked, payRate);
        }
    }
}
```

# 实例属性代替内置序数

内置序数会随枚举内部类位置而改变，通过在内部类的实例属性实现标记与类的一一对应。

```java
public enum Ensemble {
    SOLO(1), DUET(2), TRIO(3), QUARTET(4), QUINTET(5),
    SEXTET(6), SEPTET(7), OCTET(8), DOUBLE_QUARTET(8),
    NONET(9), DECTET(10), TRIPLE_QUARTET(12);

    private final int numberOfMusicians;

    Ensemble(int size) { this.numberOfMusicians = size; }
    public int numberOfMusicians() { return numberOfMusicians; }
}
```

# 按位属性替换为EnumSet

**替代前:**

```java
class Test{
    public static final int STYLE_BOLD          = 1 << 0;  // 1
    public static final int STYLE_ITALIC        = 1 << 1;  // 2
    public static final int STYLE_UNDERLINE     = 1 << 2;  // 4
    public static final int STYLE_STRIKETHROUGH = 1 << 3;  // 8

    // Parameter is bitwise OR of zero or more STYLE_ constants
    public void applyStyles(int styles) { ... }
}
```

**修改后：**

```java
// EnumSet - a modern replacement for bit fields
public class Text {
    public enum Style { BOLD, ITALIC, UNDERLINE, STRIKETHROUGH }

    // Any Set could be passed in, but EnumSet is clearly best
    public void applyStyles(Set<Style> styles) { ... }
}

// ----------------------------------------------------------------
text.applyStyles(EnumSet.of(Style.BOLD, Style.ITALIC));

```

# 使用EnumMap

```java
class Plant {
    enum LifeCycle { ANNUAL, PERENNIAL, BIENNIAL }
    final String name;
    final LifeCycle lifeCycle;

    Plant(String name, LifeCycle lifeCycle) {
        this.name = name;
        this.lifeCycle = lifeCycle;
    }

    @Override public String toString() {
        return name;
    }
}

// Using an EnumMap to associate data with an enum
Map<Plant.LifeCycle, Set<Plant>>  plantsByLifeCycle =
    new EnumMap<>(Plant.LifeCycle.class);

for (Plant.LifeCycle lc : Plant.LifeCycle.values())
    plantsByLifeCycle.put(lc, new HashSet<>());

for (Plant p : garden)
    plantsByLifeCycle.get(p.lifeCycle).add(p);

System.out.println(plantsByLifeCycle);

```