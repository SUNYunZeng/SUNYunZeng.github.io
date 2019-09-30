---
title: '设计模式: Java泛型'
comments: true
toc: true
date: 2019-09-28 22:17:08
categories: Java
tags: Java设计模式
---

# 不要使用原始类型

**原始类型**是Java(5之前)的历史遗留问题，**之前集合对于类型在编译期间不进行检查，而在运行期进行类型检查。**

这导致问题的定位困难，降低效率。

此后采用**泛型**解决这个问题，保证在编译期间进行检查。

例如<font color=#f07c82>List</font> 类型是原始类型，而<font color=#f07c82>List< E ></font>是泛型类型，指定了参数化类型为 <font color=#f07c82>E</font> 类型。

**List**表明可以存储任何类型的对象，<font color=#f07c82>List< E > </font>是它的子类型，可以转化为**List**类型，但是丧失了安全性检查；而<font color=#f07c82>List< E > </font> 却不是<font color=#f07c82>List< Object > </font>的子类型。<font color=#f07c82>List< Object > </font>只能是<font color=#f07c82>List< Object > </font>。

如果想实现类似于**List等存储任何类型的对象**， 可以利用**无限制通配符类型（unbounded wildcard types）**表示泛型类型，即<font color=#f07c82>List< ? > </font>。

**泛型的几个注意点：**

**1. 类字面常量不允许使用泛型**

数组<font color=#f07c82>String[].class</font>、基本类型<font color=#f07c82>int.class</font>、不带参数化类型的类<font color=#f07c82>List.class</font>可以使用。

**2. instanceof 只能对无限制通配符类型的参数化类型的类或接口使用。**

```java
if(o instanceof Set){
    Set<?> s = (Set<?>) o;
}
```

术语 | 中文含义 | 举例	
:-: | :-: | :-:
Parameterized type | 参数化类型 | List< String >
Actual type parameter | 实际类型参数 | String	
Generic type | 泛型类型 | List< E >	
Formal type parameter | 形式类型参数 | E	
Unbounded wildcard type | 无限制通配符类型 | List<?>	
Raw type | 原始类型 | List	
Bounded type parameter | 限制类型参数 | < E extends Number>	
Recursive type bound | 递归类型限制 | < T extends Comparable< T >>	
Bounded wildcard type | 限制通配符类型 | List<? extends Number>	
Generic method | 泛型方法 | static < E > List< E > asList(E[] a)	
Type token | 类型令牌 | String.class	

# 列表优先于数组

即<font color=#f07c82>List</font>优先于数组。

因为数组是**协变类型**的，所以下面代码编译期间是合法的, 但是运行期间是违法的。

```java
Object[] o = new String[1];
o[1] = 1;
```

尽量使用**列表**来在编译过程中就确保安全性，虽然会损失掉一定的性能与简洁性。

下面代码运行有警告，因为java程序中的泛型信息在编译后会进行**擦除**（这意味着它们只在编译时执行类型约束，并在运行时丢弃它们的元素类型信息），为了与Java5之前的代码共存。

因此实际在运行中虚拟机并不知道返回数组具体类型，返回的类型是顶层类 Object, 在强制转换为**T[]**时会有问题。

```java
public Chooser(Collection<T> choices) {
    choiceArray = (T[]) choices.toArray();
}
```

因此最好采用List替代。

```java
public Chooser(Collection<T> choices) {
    choiceList = new ArrayList<>(choices);
}
```

# 优先考虑泛型

一个简单实现的栈，原始版本 vs 添加泛型版本。

**原始版本**
```java
class Stack{
    // 存储数量
    private int size = 0;
    // 默认长度
    private static final int DEFAULT_INITIAL_CAPACITY;
    // 元素桶
    private Object[] elements;

    public Stack{
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e){
        ensureCapacity();
        elements[size++] = e;
    }

    public Object pop(){
        if(size==0)
            throw new EmptyStackException();
        Object res = elements[--size];
        // 清除无用的引用
        elements[size] = null;
        return res;
    }

    private ensureCapacity(){
        if(elements.length==size)
            elements = Arrays.copyOf(elements, size*2+1);
    }
}
```
这样的版本Stack每次取出元素都要进行强制类型转换。

**泛型版本**

```java
class Stack<E>{
    // 存储数量
    private int size = 0;
    // 默认长度
    private static final int DEFAULT_INITIAL_CAPACITY;
    // 元素桶
    private E[] elements;

    // 确保强制转换是安全的。有问题的数组（元素）保存在一个私有属性中，永远不会返回给客户端或传递给任何其他方法。 保存在数组中的唯一元素是那些传递给 push 方法的元素，它们是 E 类型的，所以未经检查的强制转换不会造成任何伤害
    @SuppressWarnings("unchecked")
    public Stack(){
        // 强制类型转换，因为无法创建泛型数组 new E[DEFAULT_INITIAL_CAPACITY];
        elements = (E)new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(E e){
        ensureCapacity();
        elements[size++] = e;
    }

    public E pop(){
        if(size==0)
            throw new EmptyStackException();
        E res = elements[--size];
        // 清除无用的引用
        elements[size] = null;
        return res;
    }

    public isEmpty(){
        return size==0;
    }

    private ensureCapacity(){
        if(elements.length==size)
            elements = Arrays.copyOf(elements, size*2+1);
    }
}
```

# 考虑泛型方法

声明类型参数的**类型参数列表**位于方法的**修饰符**和**返回类型**之间。

```java
// Generic method
public static <E> Set<E> union(Set<E> s1, Set<E> s2) {
    Set<E> result = new HashSet<>(s1);
    result.addAll(s2);
    return result;
}
```

# 限定通配符增加API灵活性

**PECS代表： producer-extends，consumer-super。**

如果一个参数化类型代表一个 <font color=#f07c82>T</font> 生产者，使用 <font color=#f07c82><? extends T></font>；如果它代表 <font color=#f07c82>T</font> 消费者，则使用 <font color=#f07c82><? super T></font>。 在我们的 <font color=#f07c82>Stack</font> 示例中，<font color=#f07c82>pushAll</font> 方法的 <font color=#f07c82>src</font> 参数生成栈使用的 <font color=#f07c82>E</font> 实例，因此 <font color=#f07c82>src</font> 的合适类型为 <font color=#f07c82>Iterable<? extends E></font>；<font color=#f07c82>popAll</font> 方法的 <font color=#f07c82>dst</font> 参数消费 <font color=#f07c82>Stack</font> 中的 <font color=#f07c82>E</font> 实例，因此 <font color=#f07c82>dst</font> 的合适类型是 <font color=#f07c82>Collection <? super E></font>。

**所有 <font color=#f07c82>Comparable</font> 和 <font color=#f07c82>Comparator</font> 都是消费者。**

# 可变参数与泛型

可变参数: 类型 <font color=#f07c82>T</font> 加<font color=#f07c82>...</font>构成，例如：

```java
public void test(String ... strs){
    for(String s: strs){
        System.out.println(s);
    }
}
```
Java用一个数组来保存变长的参数，但是**可变参数的类型与泛型要注意混淆**。

考虑一个**例子：**

```java
public <T> T[] toArray(T ... t){
    return t;
}

// test
String[] strs = toArray("abc", "def");
```

上面的会报错，因为泛型传递的是非具体类型，也就是编译时的类型信息要多与运行时信息（由于擦除），因此可变参数的数组用 <font color=#f07c82>Object</font>类型存储，当<font color=#f07c82>Object</font>转为<font color=#f07c82>String</font>时，会报错<font color=#f07c82>ClassCastException</font>。

**另一个例子：**

```java
public void test(List<String>... stringList){
    Object[] obj = stringList;
    List<Integer> intList = List.of(15); // List.of生成一个不可变列表（不能用set设置数值）
    obj[0] = intList; // 发成堆污染
    String s = obj[0].get(0); // 出现警告，编译没错，但是运行报错 ClassCastException。
}
```

可见，可变参数中混淆了泛型，还是很容易产生类型转换的不安全性，但是Java并没有因此抛弃，而在<font color=#f07c82>Arrays.asList(T... a)，Collections.addAll(Collection<? super T> c, T... elements)，EnumSet.of(E first, E... rest)</font>中大量使用，说明**只要保证可变参数中的泛型是安全的（以下三点），就可：**

**1. 可变参数数组不会存储跟修改。**
**2. 可变参数数组的引用不会转义。**
**3. 可变参数数组仅用来为方法传递可变参数。**

当然，可变参数数组可以变为列表，但同时降低了一定性能与可读性。

```java
public <T> List<T> toArray(List<T> t){
    return t;
}

// test
List<String> strs = toArray("abc", "def");
```

**可变参数的另一个用法**

因为可变参数数组的产生都会产生性能损耗，因此当 95％ 的调用是三个或更少的参数的方法，那么声明该方法的五个重载。

```java
public void foo() { }

public void foo(int a1) { }

public void foo(int a1, int a2) { }

public void foo(int a1, int a2, int a3) { }

public void foo(int a1, int a2, int a3, int... rest) { }
```

# 异构容器

泛型中规定的可变类型数量总是有限的，例如单个类型约束<font color=#f07c82>< T ></font>以及Map的<font color=#f07c82>< K , V></font>。

异构容器可以通过**参数化键**的方式，实现泛型设定数量的灵活性。例如在数据库查询中任意列值。

下面是一个异构容器的示例：

```java
public class Favorites{
    // 用Class<T>存放未知的键
    Map<Class<?>, T> favorites = new HashMap<>(); 
    public <T> void putFavorite(Class<T> type, T instance){
        favorites.put(Object.requireNonNull(type), type.cast(instance));
    }
    public <T> T get(Class<T> type){
        return type.cast(favorites.get(type));
    }
}

public static void main(String[] args) {
    Favorites f = new Favorites();
    f.put(String.class, "syz");
    f.put(Integer.class, 12);
    f,get(String.class);
}
```

可以使用 <font color=#f07c82>Column</font>Class</font> 对象作为此类型安全异构容器的键。 以这种方式使用的 Class 对象称为类型令牌。 也可以使用自定义键类型。 例如，可以有一个表示数据库行（容器）的 <font color=#f07c82>DatabaseRow</font> 类型和一个泛型类型 <font color=#f07c82>Column< T ></font> 作为其键。