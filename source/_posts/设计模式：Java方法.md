---
title: 设计模式：Java方法
comments: true
toc: true
date: 2019-09-22 21:14:07
categories: Java
tags: Java设计模式
---

# 使用 <font color=#f07c82>try-with-resources</font>调用用完需关闭的方法。

实现了 <font color=#f07c82>AutoCloseable</font>接口（由一个返回为 <font color=#f07c82>void</font>的<font color=#f07c82>close</font>组成）的资源可以使用<font color=#f07c82>try-with-resources</font>方法。

<font color=#f07c82>AutoCloseable</font>接口在Java的类库和第三方类库中许多类和接口都有实现或继承，例如 BufferedReader、InputStream、OutputStream 等。

```java
// example
static void copy(String src, String dst) throws IOException {
    try(InputStream in = new FileInputStream(src);
        OutputStream out = new FileOutputStream(dst)){
            byte[] buf = new byte[BUFFER_SIZE];
            int n;
            while((n = in.read(buf)) >= 0){
                out.write(buf, 0, n);
            }
        }
    OutputStream 
}
```

# 重写<font color=#f07c82>equals</font>方法规则

1. 使用 **==** 运算符检查参数是否为该对象的引用。如果是，返回 **true**。这只是一种性能优化，但是如果这种比较可能很昂贵的话，那就值得去做。

2. 使用 **instanceof** 运算符来检查参数是否具有正确的类型。 如果不是，则返回 false。 通常，正确的类型是 equals 方法所在的那个类。 有时候，改类实现了一些接口。 如果类实现了一个接口，该接口可以改进 equals 约定以允许实现接口的类进行比较，那么使用接口。 集合接口（如 Set，List，Map 和 Map.Entry）具有此特性。

3. 参数转换为正确的类型。因为转换操作在 instanceof 中已经处理过，所以它肯定会成功。

4. 对于类中的每个「重要」的属性，请检查该参数属性是否与该对象对应的属性相匹配。如果所有这些测试成功，返回 true，否则返回 false。如果步骤 2 中的类型是一个接口，那么必须通过接口方法访问参数的属性;如果类型是类，则可以直接访问属性，这取决于属性的访问权限。


+ 对于类型为**非 float** 或 **double** 的基本类型，使用 == 运算符进行比较；对于对象引用属性，递归地调用 equals 方法；对于 float 基本类型的属性，使用静态 **Float.compare(float, float)** 方法；对于 double 基本类型的属性，使用 **Double.compare(double, double)** 方法。由于存在 **Float.NaN，-0.0f 和类似的 double 类型**的值，所以需要对 float 和 double 属性进行特殊的处理；虽然你可以使用静态方法 Float.equals 和 Double.equals 方法对 float 和 double 基本类型的属性进行比较，这会导致每次比较时发生自动装箱，引发非常差的性能。 对于数组属性，将这些准则应用于每个元素。 如果数组属性中的每个元素都很重要，请使用其中一个重载的 Arrays.equals 方法。

+ 某些对象引用的属性可能合法地包含 **null**。 为避免出现 NullPointerException 异常，请使用静态方法 **Objects.equals(Object, Object)** 检查这些属性是否相等。

# 重写<font color=#f07c82>equals</font>方法时一定重写<font color=#f07c82>hashCode</font>方法。

基于Hash值的散列数据结构，例如hashMap、HashSet等，需要基于Hash值判断两个是否相等，其实是根据<font color=#f07c82>equals</font>方法来判断。

<font color=#f07c82>equals</font>方法改变，要想实例也能跟被基于hash的散列结构识别，需要将<font color=#f07c82>hashCode</font>方法重写。

# 时刻重写<font color=#f07c82>toString</font>方法。

# 使用clone方法对于<font color=#f07c82>可变对象</font>的克隆需要谨慎。

克隆出来的对象的类型与原始对象的一致，并且<font color=#f07c82>x.clone != x</font>返回<font color=#f07c82>true</font>。

```java
// HashTable的克隆方法，需要深度克隆，保证克隆出来的对象是原先对象的深层拷贝，桶或链都拷贝一份
// Recursive clone method for class with complex mutable state
public class HashTable implements Cloneable {
    private Entry[] buckets = ...;

    private static class Entry {
        final Object key;
        Object value;
        Entry  next;

        Entry(Object key, Object value, Entry next) {
            this.key   = key;
            this.value = value;
            this.next  = next;  
        }

        // Recursively copy the linked list headed by this Entry
        //Entry deepCopy() {
        //    return new Entry(key, value,
        //        next == null ? null : next.deepCopy());
        //}
        // 避免链表过长导致栈溢出错误
        Entry deepCopy(){
            Entry result = new Entry(key, value, next);
            for(Entry p = result; p!=null; p=p.next){
                 p.next = new Entry(p.next.key, p.next.value, p.next.next);
            }
            return result;
        }
    }

    @Override public HashTable clone() {
        try {
            HashTable result = (HashTable) super.clone();
            result.buckets = new Entry[buckets.length];
            for (int i = 0; i < buckets.length; i++)
                if (buckets[i] != null)
                    result.buckets[i] = buckets[i].deepCopy();
            return result;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
    ... // Remainder omitted
}
```

# 在可比较大小的类中考虑实验<font color=#f07c82>Comparable</font>接口。

```java
public interface Comparable<T>{
    int compareTo(T t);
}
```

在<font color=#f07c82>conpareTo()</font>中比较大小时，避免使用<font color=#f07c82> < </font>或者<font color=#f07c82> > </font>,应该使用包装类中的<font color=#f07c82>compare()</font>方法或者自己实现<font color=#f07c82>Comparator</font>接口中的<font color=#f07c82>compare()</font>方法

```java
// Comparator based on static compare method
static Comparator<Object> hashCodeOrder = new Comparator<>() {
    public int compare(Object o1, Object o2) {
        return Integer.compare(o1.hashCode(), o2.hashCode());
    }
};

// Comparator based on Comparator construction method
static Comparator<Object> hashCodeOrder =
        Comparator.comparingInt(o -> o.hashCode());

```

**参考：** <u>https://sjsdfg.github.io/effective-java-3rd-chinese/#/</u>