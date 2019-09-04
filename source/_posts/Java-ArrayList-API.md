---
title: Java ArrayList Handbook
toc: true
date: 2019-05-21 21:54:33
categories: Java
tags: 
    - Tips
---

# **Java List**

## **常用类型**

类型 |特征 
:-: | :-: 
**ArrayList** | 随机访问元素快；中间插入与删除元素较慢；操作不是线程安全的 
**LinkedList** | 中间插入与删除操作代价较低，提供优化的顺序访问；随机访问元素慢 

## **ArrayList**

**ArrayList的UML类图如下所示：**

![avatar](http://www.plantuml.com/plantuml/png/TKynQiGm4Epr2k7UVa4AcrI1L4a-i9Ot85XB8AsBXFmahqWTo6_2VUDiOT_D2QuHgvcTsPa-AoLTPZ5lNeLjCwH47yy-w-ddz__xhp4-AATtScoVre56OO0UemWxzJ40kAaDZLaJERLEA6Sxx5yGtDdOr63sGc6Ay-0Svuo79GQkmhnoyYJ-iy2xwT76c2EzVVXgMmn-7LUgwwgezKfH5qVSG_AcoFQWxGvZgq93qakrca27SWeux8uwp81Ops5QPZa3)

ArrayList 继承了<font color=#f07c82> AbstractList</font>, 直接实现了<font color=#f07c82> Cloneable, Serializable，RandomAccess </font>类型标志接口。

+ AbstractList 作为列表的抽象实现，将元素的增删改查都交给了具体的子类去实现，在元素的迭代遍历的操作上提供了默认实现。

+ RandomAccess 接口实现，表示 ArrayList 里的元素可以被高效效率的随机访问，以下标数字的方式获取元素。实现 RandomAccess 接口的列表上在遍历时可直接使用普通的<font color=#f07c82> for </font>循环方式，并且执行效率上给迭代器方式更高。

+ Cloneable 接口的实现，表示了 ArrayList 支持调用 Object 的 <font color=#f07c82> clone </font>方法，实现 ArrayList 的拷贝。

+ Serializable 接口实现，说明了 ArrayList 还支持序列化和反序列操作，具有固定的 <font color=#f07c82> serialVersionUID </font> 属性值。

### **ArrayList 常用 API**

方法 | 描述 
:-: | :-: 
**boolean add(E object)** | 再 ArrayList 尾存入对象 
**void add(int location, E object)** | 在 location 添加对象 
**boolean addAll(Collection<? extends E> collection)** | 将一个 Collection 对象中存储全部的对象复制并存入 
**boolean addAll(int location, Collection<? extends E> collection)** | 将一个 Collection 对象中存储全部的对象复制并存入location 
**boolean contains(Object object)** | 是否包对象 object 
**boolean containsAll(Collection<?> collection)** | 是否包含一个 Collection 对象 
**boolean isEmpty()** | ArrayList 是否为空 
**E get(int location)** | 获取 location 的对象 
**E set(int location, E object)** | 将某个位置的元素替换成 object
**int indexOf(Object object)** | 获取某个对象的位置（顺序遍历第一个） 
**int lastIndexOf(Object object)** | 获取某个对象的位置（逆序遍历第一个） 
**int size()** | 返回 ArrayList 存储的对象数量 
**boolean remove(Object object)** | 删除存储的某个对象 
**E remove(int lcoation)** | 删除位于 location 的对象 
**boolean removeAll(Collection<?> collection)** | 删除存储的某个 Collection 对象 
**Object [] toArray()** | ArrayList 转数组 
**List<'E'> subList(int start, int end)** | 获取位于 start 与 end 之间的 List 
**void clone()** | 克隆ArrayList 
**void clear()** | 移除ArrayList中所有对象 
**Iterator<'E'> iterator()** | 获取该 ArrayList 对应的迭代器对象 

### ArrayList 应用示例

1. ArrayList与数组之间的转换

    ```Java
        // 转数组

        List<String> list = new ArrayList<>();
        String[] strings = new String[list.size()];
        list.toArray(strings);

        // 或者
        String[] strings = (String[])list.toArray(new String[list.size()]);

        // 数组转List
        // 法1 快速，但是以视图形式返回，无法对数据进行删除及添加操作；
        // 可用 set() 方法修改元素，但是原始List数据会随之改变

        String[] s = {"abc", "def", "ghi"};
        List<String> list = java.util.Arrays.asList(s);

        //法 2 慢，但是新生成一个ArrayList，可对List进行操作不会对原对象产生影响

        List<String> assertList = new ArrayList();
        Collections.addAll(assertList, strings);
    ```
2. ArrayList遍历方式

    ```Java
    // 第一种，通过迭代器遍历。即通过Iterator去遍历。
    Integer value = null;
    Iterator iter = list.iterator();
    while (iter.hasNext()) {
        value = (Integer)iter.next();
    }

    // 第二种，随机访问，通过索引值去遍历。
    Integer value = null;
    int size = list.size();
    for (int i=0; i < size; i++) {
        value = (Integer)list.get(i);        
    }

    // 第三种，for循环遍历。
    Integer value = null;
    for (Integer integer: list) {
        value = integer;
    }
    ```

3. 初始化

    ```Java
    // 第一种方式 Arrays.asList()方法
    ArrayList<Integer> list = new Arraylist<>(Arrays.asList(1, 2, 3));

    // 第二种方法 常规方式
    ArrayList<Integer> list = new ArrayList<>();
    list.add(1);
    list.add(2);
    list.add(3);

    //或者
    List innerList = Arrays.asList(1, 2, 3);
    list.addAll(innerList);

    // 第三种方法 使用生成匿名内部类
    ArrayList<Integer> list = new ArrayList<>(){
        {
            add(1);
            add(2);
            add(3);
        }
    }；

    // 第四种方式 使用Collection.nCopies
    int element = 1;
    ArrayList<Integer> list = new ArrayList<>(Collections.nCopies(2, element)); //复制伍分到list中。
    ```
4. 打印信息

    由于ArrayList内部实现了toString()方法，所以可以直接打印

    ```Java
        ArrayList<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 3));
        System.out.println(list);
    ```

    对于数组信息的打印

    ```Java
        int[] A = {1, 2, 3};

        // 方法一
        for(Integer i: A){
            System.out.println(i);
        }

        //方法二
        System.out.println(Arrays.toString(A));
    ```

# 参考资料

>1. <font color=#2b1216> ArrayList源码剖析:</font> <u>http://www.spring4all.com/article/16262</u>