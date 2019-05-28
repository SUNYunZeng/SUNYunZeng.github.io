---
title: Java--Map API
toc: true
date: 2019-05-19 21:42:31
categories: Java
tags: 
    - Map
---

# **Java Map**

## **常用类型**


类型 |特征 
:-: | :-: 
**HashMap** | 根据 HashCode 存储数据，访问速度快。至多允许一条记录键为 null;允许多条记录的值为 null；线程非同步  
**TreeMap** | 保存的记录按照键(key)排序，也可自定义排序规则。用生成的 Iterator 遍历 TreeMap 得到记录是排序后的。不允许记录的键为 null；线程非同步 
**Hashtable** | 用 HashMap 类似,不同的是键值都不允许为 null。 支持线程同步，但写入较慢 
**LinkedHashMap** | 保留记录的插入顺序，生成Iterator遍历顺序与插入顺序一致。遍历比HashMap慢，键值都允许为 null；线程非同步 

> --> [**HashMap剖析**](https://zhuanlan.zhihu.com/p/21673805)


## **常用 API**

方法 | 描述 
:-: | :-: 
**Object put(Object k, Object v)** | 存入键值对 
**Object get(Object k)** | 返回键所映射的值；如果不存在该映射则返回 null 
**boolean containsKey(Object k)** | 是否包含键 k 
**boolean containsKey(Object v)** | 是否包含值 v 
**boolean isEmpty()** | Map 是否为空 
**int size()** | 返回 Map 的键值对数 
**boolean remove(Object k)** | 如果存在一个键映射关系，则删除此关系(映射关系不存在不会报错) 
**void clear()** | 移除Map中所有映射关系 
**boolean equals(Object obj)** | 比较指定对象于此 Map 是否相等 
**void putAll(Map m)** | 将指定 Map 的映射关系复制到此 Map 中 
**Collection values()** | 以 Collection 形式返回 Map 包含的值 
**Set keySet()** | 以 Set 形式返回 Map 包含的键 
**Set entrySet()** | 以 Set 形式返回 Map 的映射关系

## **List 应用示例**

```Java
Map<Integer, Character> map = new HashMap<>();
map.put(1, 'c');
map.get(1);
for(int i: map.keySet()){
        System.out.println(map.get(i));
    }
if(map.containsKey(1)){
    map.remove(1);
}
```