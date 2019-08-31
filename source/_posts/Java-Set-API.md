---
title: 'Java: Set API'
comments: true
toc: true
date: 2019-08-31 09:12:40
categories: java
tags: Tips
---

# **Java Set**

## **概述**

**Set 继承自集合（Collection），该集合不能包含相同的元素。**

**Set** 里面进行元素是否相同的判定是通过 **Object** 类自带的**equals()**实现。

**Set** 最多可存储一个 **null** 元素。

**Set** 只是一个抽象的**接口**，具体的使用还要用具体的实现，如**HashSet**、**TreeSet**等。

## **常用方法**

因为 Set 继承自集合 Collection，所以具有集合的方法。

方法 | 描述
:-: | :-:
**int size()** | 返回Set里面存储的元素个数。
**boolean isEmpty()** | 如果没有元素，返回true。
**boolean add(E e)** | 如果Set里面没有包含元素e，就将其加入。
**boolean addAll(Collection<? extends E>c)** | 如果指定集合中的元素不存在Set中，就将其加入Set。如果该Collection也是一个Set，相当于对这两个Set取并集。
**boolean contains(Object o)** | 是否包含特定的元素 o。即对Set里面的任意元素e执行判定(o==null?e==null:o.equals(e))。
**boolean containsAll(Collecton< ? >c)** | 该Set是否包含指定Collection的所有元素。
**void clear()** | 清除所有元素。
**boolean remove(Object o)** | 删除指定元素。
**boolean removeAll(Collection< ? >c)** | 删除Set中存在于该Collection里的元素。
**Object[ ] toArray()** | 将Set转化为数组。
**< T > T[ ] toArray(T[ ] a)** | 返回所有的Set元素并存储在Array中。如果a的长度大于Set长度，则多余空间以null补全。
**Iterator< E > iterator()** | 返回一个该Set的迭代器
**default Spliterator< E > spliterator()** | 在该集合中创建拆分器。

## **常用Set实现**

### **HashSet**

HashSet的方法基本与Set一致，只不过多了一个**Object clone()**方法（浅复制，只复制地址）。

主要方法包括：
+ add()
+ clear()
+ clone()
+ contains()
+ isEmpty()
+ iterator()
+ remove()
+ size()
+ spliterator()

**HashSet**底层是基于**HashMap**实现的。即通过HashMap的键唯一性实现。

**构造方法示例**
```java
public HashSet() {
        map = new HashMap<>();
    }
public HashSet(Collection<? extends E> c) {
        map = new HashMap<>(Math.max((int) (c.size()/.75f) + 1, 16));
        addAll(c);
    }
public HashSet(int initialCapacity, float loadFactor) {
        map = new HashMap<>(initialCapacity, loadFactor);
    }
public HashSet(int initialCapacity) {
        map = new HashMap<>(initialCapacity);
    }
// 该构造方法为LinkedHashSet实现准备
HashSet(int initialCapacity, float loadFactor, boolean dummy) {
        map = new LinkedHashMap<>(initialCapacity, loadFactor);
    }

```

### **LinkedHashSet**

**LinkedHashSet**保存了元素的顺序，即插入时的顺序，再使用iterator遍历时会按顺序遍历。

**LinkedHashSet**底层也是根据**LinkHashMap**实现的。通过父类**HashSet**的构造方法，调用**LinkHashMap**。
```java
HashSet(int initialCapacity, float loadFactor, boolean dummy) {
        map = new LinkedHashMap<>(initialCapacity, loadFactor);
    }
```

继承自 **HashSet**，实现了接口 **Serializable** 及 **Cloneable**。

**Serializable**是一个空接口，是一个序列化的标记，用来告诉JVM该类可以序列化。

<u>**序列化就是把对象的状态转化为可存储和传输的格式（如二进制流）；反序列化就是序列化的逆过程，根据序列化后的数据重新恢复对象及其状态。**</u>

### **TreeSet**

可实现排序的Set，排序规则可以是自带的或者通过Comparator实现。

基础操作（add,contains and remove）的耗时是log(n)。

实现了**NavigableSet**接口，该接口是基于**TreeMap**的。

**TreeSet**底层是基于TreeMap实现，所以对于系统内部类例如Integer、String等，由于实现了Comparable接口，可直接进行存储；对于自定义的类，必须实现Comparable接口并重写comparaTo（）方法，这样TreeSet才能根据排序规则进行排序。

**TreeSet**不能有重复元素。

**TreeSet**的存取不如**HashSet**快。

**comparaTo()**方法在被调用过程中，如果返回真值（或大于零的数），就认为新插入元素大于根元素，存入右节点，此时为顺序排列；反之存入左节点，为逆序排列。

**构造函数**：

序号 | 构造函数的说明
:-:|:-:
1 | TreeSet()，此构造函数构造空树集，将在根据其元素的自然顺序按升序排序。
2 | TreeSet (集合 c)，此构造函数生成树的集合，它包含的元素的集合 c。
3 | TreeSet (比较器 comp)，此构造函数构造一个空树集，将根据给定的比较器进行排序。
4 | TreeSet (SortedSet ss)，此构造函数生成包含给定 SortedSet 的元素 TreeSet

 **常用方法**：

修饰符和类型|方法和描述
:-:|:-:
boolean|add(E e)，将指定的元素添加到这套，如果它已不存在。
boolean|addAll(Collection<? extends E> c)，在加入这一组指定的集合中添加的所有元素。
E|ceiling(E e)，返回最小的元素在这一组大于或等于给定的元素，则null如果没有这样的元素。
void |clear()，从这一组中移除所有元素。
Object|clone()，返回此TreeSet实例浅表副本。
Comparator<? super E>|comparator()，返回用于排序在这集，或空元素，如果这套使用自然排序其元素的比较。
boolean|contains(Object o)，如果此集合包含指定的元素，则返回true 。
Iterator<E>|descendingIterator()，返回迭代器中这套降序排序的元素。
NavigableSet<E>	|descendingSet()，返回逆序视图中包含的元素这一套。
E|first()，返回第一个 （最低） 元素当前在这一套。
E|floor(E e)，返回的最大元素在这一组小于或等于null如果没有这样的元素。
SortedSet<E>|headSet(E toElement)，返回其元素是严格小于toElement这套的部分视图.
NavigableSet<E>|headSet(E toElement, boolean inclusive)，返回一个视图的这部分设置的元素都小于 （或等于，如果inclusive是真的） toElement.
E| higher(E e)，返回最小的元素在这套严格大于给定的元素，则null如果没有这样的元素。
boolean|isEmpty()，如果此集不包含任何元素，则返回true 。
Iterator<E>|iterator()，返回迭代器中这套以升序排序的元素。
E|last()，在这套目前返回的最后一个 （最高） 的元素。
E|lower(E e)，在这一套严格的小于给定的元素，则null返回的最大元素，如果没有这样的元素。
E|pollFirst()，检索和删除第一个 （最低） 元素，或如果此集合为空，则返回null 。
E|pollLast()，检索和删除的最后一个 （最高） 的元素，或如果此集合为空，则返回null 。
boolean	|remove(Object o)，从这一组中移除指定的元素，如果它存在。
int|size()，在这套 （其基数） 中返回的元素的数目。
NavigableSet<E>|subSet(E fromElement, boolean fromInclusive, E toElement, boolean toInclusive)，返回此集的部分视图的元素范围从fromElement到toElement.
SortedSet<E>|subSet(E fromElement, E toElement)，返回视图的部分的这一套的元素范围从fromElement，具有包容性，到toElement，独家。
SortedSet<E>|tailSet(E fromElement)，返回其元素是大于或等于fromElement这套的部分视图.
NavigableSet<E>|tailSet(E fromElement, boolean inclusive)，返回其元素是大于 （或等于，如果inclusive是真的） 这套的部分视图fromElement.

**实现原理**

**TreeSet**底层依赖于TreeMap，通过TreeMap来作为存储TreeSet的容易，键值保证了元素的唯一性。

采用“**红黑树**”的排序二叉树保存Map中的每个Entry，每个Entry被当做“红黑树”的一个节点。

“**红黑树**”是一种平衡二叉查找树，树中节点都大于等于左子树所有节点，且小于等于右子树左右节点。

**TreeSet部分源码**
```java
package java.util;

public class TreeSet<E> extends AbstractSet<E>
    implements NavigableSet<E>, Cloneable, java.io.Serializable
{
    // 使用NavigableMap对象的key来保存Set集合的元素
    private transient NavigableMap<E,Object> m;

    //使用PRESENT作为Map集合中的value
    private static final Object PRESENT = new Object();

    // 不带参数的构造函数。创建一个空的TreeMap
    //以自然排序方法创建一个新的TreeMap，再根据该TreeMap创建一个TreeSet
    //使用该TreeMap的key来保存Set集合的元素
    public TreeSet() {
        this(new TreeMap<E,Object>());
    }

    // 将TreeMap赋值给 "NavigableMap对象m"
    TreeSet(NavigableMap<E,Object> m) {
        this.m = m;
    }

    //以定制排序的方式创建一个新的TreeMap。根据该TreeMap创建一个TreeSet
    //使用该TreeMap的key来保存set集合的元素
    public TreeSet(Comparator<? super E> comparator) {
        this(new TreeMap<E,Object>(comparator));
    }

    // 创建TreeSet，并将集合c中的全部元素都添加到TreeSet中
    public TreeSet(Collection<? extends E> c) {
        this();
        // 将集合c中的元素全部添加到TreeSet中
        addAll(c);
    }

    // 创建TreeSet，并将s中的全部元素都添加到TreeSet中
    public TreeSet(SortedSet<E> s) {
        this(s.comparator());
        addAll(s);
    }

    // 返回TreeSet的顺序排列的迭代器。
    // 因为TreeSet时TreeMap实现的，所以这里实际上时返回TreeMap的“键集”对应的迭代器
    public Iterator<E> iterator() {
        return m.navigableKeySet().iterator();
    }

    // 返回TreeSet的逆序排列的迭代器。
    // 因为TreeSet时TreeMap实现的，所以这里实际上时返回TreeMap的“键集”对应的迭代器
    public Iterator<E> descendingIterator() {
        return m.descendingKeySet().iterator();
    }

    // 返回TreeSet的大小
    public int size() {
        return m.size();
    }

    // 返回TreeSet是否为空
    public boolean isEmpty() {
        return m.isEmpty();
    }

    // 返回TreeSet是否包含对象(o)
    public boolean contains(Object o) {
        return m.containsKey(o);
    }

    // 添加e到TreeSet中
    public boolean add(E e) {
        return m.put(e, PRESENT)==null;
    }

    // 删除TreeSet中的对象o
    public boolean remove(Object o) {
        return m.remove(o)==PRESENT;
    }

    // 清空TreeSet
    public void clear() {
        m.clear();
    }

    // 将集合c中的全部元素添加到TreeSet中
    public  boolean addAll(Collection<? extends E> c) {
        // Use linear-time version if applicable
        if (m.size()==0 && c.size() > 0 &&
            c instanceof SortedSet &&
            m instanceof TreeMap) {
            //把C集合强制转换为SortedSet集合
            SortedSet<? extends E> set = (SortedSet<? extends E>) c; 
             //把m集合强制转换为TreeMap集合
            TreeMap<E,Object> map = (TreeMap<E, Object>) m;
            Comparator<? super E> cc = (Comparator<? super E>) set.comparator();
            Comparator<? super E> mc = map.comparator();
            //如果cc和mc两个Comparator相等
            if (cc==mc || (cc != null && cc.equals(mc))) {
            //把Collection中所有元素添加成TreeMap集合的key
                map.addAllForTreeSet(set, PRESENT);
                return true;
            }
        }
        return super.addAll(c);
    }

    // 返回子Set，实际上是通过TreeMap的subMap()实现的。
    public NavigableSet<E> subSet(E fromElement, boolean fromInclusive,
                                  E toElement,   boolean toInclusive) {
        return new TreeSet<E>(m.subMap(fromElement, fromInclusive,
                                       toElement,   toInclusive));
    }

    // 返回Set的头部，范围是：从头部到toElement。
    // inclusive是是否包含toElement的标志
    public NavigableSet<E> headSet(E toElement, boolean inclusive) {
        return new TreeSet<E>(m.headMap(toElement, inclusive));
    }

    // 返回Set的尾部，范围是：从fromElement到结尾。
    // inclusive是是否包含fromElement的标志
    public NavigableSet<E> tailSet(E fromElement, boolean inclusive) {
        return new TreeSet<E>(m.tailMap(fromElement, inclusive));
    }

    // 返回子Set。范围是：从fromElement(包括)到toElement(不包括)。
    public SortedSet<E> subSet(E fromElement, E toElement) {
        return subSet(fromElement, true, toElement, false);
    }

    // 返回Set的头部，范围是：从头部到toElement(不包括)。
    public SortedSet<E> headSet(E toElement) {
        return headSet(toElement, false);
    }

    // 返回Set的尾部，范围是：从fromElement到结尾(不包括)。
    public SortedSet<E> tailSet(E fromElement) {
        return tailSet(fromElement, true);
    }

    // 返回Set的比较器
    public Comparator<? super E> comparator() {
        return m.comparator();
    }

    // 返回Set的第一个元素
    public E first() {
        return m.firstKey();
    }

    // 返回Set的最后一个元素
    public E first() {
    public E last() {
        return m.lastKey();
    }

    // 返回Set中小于e的最大元素
    public E lower(E e) {
        return m.lowerKey(e);
    }

    // 返回Set中小于/等于e的最大元素
    public E floor(E e) {
        return m.floorKey(e);
    }

    // 返回Set中大于/等于e的最小元素
    public E ceiling(E e) {
        return m.ceilingKey(e);
    }

    // 返回Set中大于e的最小元素
    public E higher(E e) {
        return m.higherKey(e);
    }

    // 获取第一个元素，并将该元素从TreeMap中删除。
    public E pollFirst() {
        Map.Entry<E,?> e = m.pollFirstEntry();
        return (e == null)? null : e.getKey();
    }

    // 获取最后一个元素，并将该元素从TreeMap中删除。
    public E pollLast() {
        Map.Entry<E,?> e = m.pollLastEntry();
        return (e == null)? null : e.getKey();
    }

    // 克隆一个TreeSet，并返回Object对象
    public Object clone() {
        TreeSet<E> clone = null;
        try {
            clone = (TreeSet<E>) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new InternalError();
        }

        clone.m = new TreeMap<E,Object>(m);
        return clone;
    }

    // java.io.Serializable的写入函数
    // 将TreeSet的“比较器、容量，所有的元素值”都写入到输出流中
    private void writeObject(java.io.ObjectOutputStream s)
        throws java.io.IOException {
        s.defaultWriteObject();

        // 写入比较器
        s.writeObject(m.comparator());

        // 写入容量
        s.writeInt(m.size());

        // 写入“TreeSet中的每一个元素”
        for (Iterator i=m.keySet().iterator(); i.hasNext(); )
            s.writeObject(i.next());
    }

    // java.io.Serializable的读取函数：根据写入方式读出
    // 先将TreeSet的“比较器、容量、所有的元素值”依次读出
    private void readObject(java.io.ObjectInputStream s)
        throws java.io.IOException, ClassNotFoundException {
        // Read in any hidden stuff
        s.defaultReadObject();

        // 从输入流中读取TreeSet的“比较器”
        Comparator<? super E> c = (Comparator<? super E>) s.readObject();

        TreeMap<E,Object> tm;
        if (c==null)
            tm = new TreeMap<E,Object>();
        else
            tm = new TreeMap<E,Object>(c);
        m = tm;

        // 从输入流中读取TreeSet的“容量”
        int size = s.readInt();

        // 从输入流中读取TreeSet的“全部元素”
        tm.readTreeSet(size, s, PRESENT);
    }

    // TreeSet的序列版本号
    private static final long serialVersionUID = -2479143000061671589L;
}
```

我们发现，**TreeSet**底层是依靠**TreeMap**对**key**进行存储排序实现的，现在看一下**TreeMap**的部分源码。

**TreeMap的put()方法**
```java
public V put(K key, V value) {
      //定义一个t来保存根元素
        Entry<K,V> t = root;
        //如果t==null，表明是一个空链表
        if (t == null) {
        //如果根节点为null，将传入的键值对构造成根节点（根节点没有父节点，所以传入的父节点为null）
            root = new Entry<K,V>(key, value, null);
            //设置该集合的size为1
            size = 1;
            //修改此时+1
            modCount++;
            return null;
        }
        // 记录比较结果
        int cmp;
        Entry<K,V> parent;
        // 分割比较器和可比较接口的处理
        Comparator<? super K> cpr = comparator;
        // 有比较器的处理，即采用定制排序
        if (cpr != null) {
            // do while实现在root为根节点移动寻找传入键值对需要插入的位置
            do {
                //使用parent上次循环后的t所引用的Entry
                // 记录将要被掺入新的键值对将要节点(即新节点的父节点)
                parent = t;
                // 使用比较器比较父节点和插入键值对的key值的大小
                cmp = cpr.compare(key, t.key);
                // 插入的key较大
                if (cmp < 0)
                    t = t.left;
                // 插入的key较小
                else if (cmp > 0)
                    t = t.right;
                // key值相等，替换并返回t节点的value(put方法结束)
                else
                    return t.setValue(value);
            } while (t != null);
        }
        // 没有比较器的处理
        else {
            // key为null抛出NullPointerException异常
            if (key == null)
                throw new NullPointerException();
            Comparable<? super K> k = (Comparable<? super K>) key;
            // 与if中的do while类似，只是比较的方式不同
            do {
                parent = t;
                cmp = k.compareTo(t.key);
                if (cmp < 0)
                    t = t.left;
                else if (cmp > 0)
                    t = t.right;
                else
                    return t.setValue(value);
            } while (t != null);
        }
        // 没有找到key相同的节点才会有下面的操作
        // 根据传入的键值对和找到的“父节点”创建新节点
        Entry<K,V> e = new Entry<K,V>(key, value, parent);
        // 根据最后一次的判断结果确认新节点是“父节点”的左孩子还是又孩子
        if (cmp < 0)
            parent.left = e;
        else
            parent.right = e;
        // 对加入新节点的树进行调整
        fixAfterInsertion(e);
        // 记录size和modCount
        size++;
        modCount++;
        // 因为是插入新节点，所以返回的是null
        return null;
    }
```

发现在插入过程中，会进行二叉树排序的判定：

1. 如果新增节点大于当前节点且当前节点的右子节点存在，则以右子节点作为当前节点。并继续循环
2. 如果新增节点小于当前节点且当前节点的左子节点存在，则以左子节点作为当前节点。并继续循环
3. 如果新增节点等于当前节点，则新增节点覆盖当前节点，并结束循环。

**TreeMap的get()方法**
```java
public V get(Object key) {
     //根据key取出Entry
     Entry<K,V> p = getEntry(key);
     //取出Entry所包含的value
     return (p==null ? null : p.value);
 }
```
关键在于 **getEntry()**是怎么根据Comparable取出对应Entry的。
```java
final Entry<K,V> getEntry(Object key) {
    // 如果有比较器，返回getEntryUsingComparator(Object key)的结果
    if (comparator != null)
        return getEntryUsingComparator(key);
    // 查找的key为null，抛出NullPointerException
    if (key == null)
        throw new NullPointerException();
    // 如果没有比较器，而是实现了可比较接口
    //将key强制转换为Comparable接口
    Comparable<? super K> k = (Comparable<? super K>) key;
    // 获取根节点
    Entry<K,V> p = root;
    // 从根节点开始对树进行遍历查找节点
    while (p != null) {
        // 把key和当前节点的key进行比较
        int cmp = k.compareTo(p.key);
        // key小于当前节点的key
        if (cmp < 0)
            // p “移动”到左节点上
            p = p.left;
        // key大于当前节点的key
        else if (cmp > 0)
        // p “移动”到右节点上
　　　　p = p.right;
        // key值相等则当前节点就是要找的节点
        else
            // 返回找到的节点
            return p;
        }
    // 没找到则返回null
    return null;
}
```

其实就是一个二叉查找，根据Comparable进行key大小的判断。如果采用定制比较器，则采用**getEntryUsingComparator()**方法。

```java
final Entry<K,V> getEntryUsingComparator(Object key) {
    K k = (K) key;
    // 获取比较器
    Comparator<? super K> cpr = comparator;
    // 其实在调用此方法的get(Object key)中已经对比较器为null的情况进行判断，这里是防御性的判断
    if (cpr != null) {
        // 获取根节点
            Entry<K,V> p = root;
            // 遍历树
            while (p != null) {
                // 获取key和当前节点的key的比较结果
                int cmp = cpr.compare(k, p.key);
                // 查找的key值较小
                if (cmp < 0)
                    // p“移动”到左孩子
                    p = p.left;
                // 查找的key值较大
                else if (cmp > 0)
                    // p“移动”到右节点
                    p = p.right;
                // key值相等
                else
                    // 返回找到的节点
                    return p;
            }
    }
    // 没找到key值对应的节点，返回null
    return null;
}
```

<u>[参考资料1](https://blog.csdn.net/qq_36057761/article/details/80923053)</u>      <u>[参考资料2](https://www.jianshu.com/p/12f4dbdbc652)</u>