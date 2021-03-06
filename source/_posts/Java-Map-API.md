---
title: Java：Map API
toc: true
date: 2019-05-19 21:42:31
categories: Java
tags: 
    - 语法
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
**boolean containsValue(Object v)** | 是否包含值 v 
**boolean isEmpty()** | Map 是否为空 
**int size()** | 返回 Map 的键值对数 
**boolean remove(Object k)** | 如果存在一个键映射关系，则删除此关系(映射关系不存在不会报错) 
**void clear()** | 移除Map中所有映射关系 
**boolean equals(Object obj)** | 比较指定对象于此 Map 是否相等 
**void putAll(Map m)** | 将指定 Map 的映射关系复制到此 Map 中 
**Collection values()** | 以 Collection 形式返回 Map 包含的值 
**Set keySet()** | 以 Set 形式返回 Map 包含的键 
**Set entrySet()** | 以 Set 形式返回 Map 的映射关系

**Map 应用示例**

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
## **常用遍历方法**

**1. 只获取键或值**

```java
// 获取键
for(Integer key: map.keyset()){
    System.out.println(key);
}

// 获取值
for(Integer value: map.value()){
    System.out.println(value);
}

```

**2. 同时获取键和值**

```java
// 2.1 先取key再取value。不推荐

for(Integer key:map.keySet()){
    System.out.println(map.get(key));
}

// 2.2 通过map entrySet遍历。性能优于上一种。

for(Map.Entry<Integer, Integer> entry: map.entrySet()){
    System.out.println(entry.getKey() + ":" + entry.getValue());
}
```
**3. Iterator**

上面的foreach都可以用Interator代替。

foreach是对Set遍历,大小不能改变。如果改变map的大小，会报错。如果想要删除元素，还是要用Interator的方式删除。

```java
Interator<Map.Entry<Integer, Integer>> it = map.enteySet().iterator();
while(it.hasNext()){
    Map.Entry<Integer, Integer> entry = it.next();
    System.out.println(entry.getKey() + ":" + entry.getValue());
    // it.remove()  删除元素
}
```

**4. Lambda**

代码简洁，但是性能低于entrySet。

```java
map.forEach((key, value)->{
    System.out.println(key + ":" + value);
})
```

**5. 性能测试**

用10万条数据，做了一个简单性能测试，数据类型为Integer，map实现选取HashMap
```java
static{
     for (int i = 0; i < 100000; i++) {
      map.put(i, 1);
    }
}
```

> **测试结果如下：**
    KeySet：           392
    Values：           320
    keySet get(key)：  552
    entrySet：         465
    entrySet Iterator：508
    Lambda：           536

[Map的遍历方法参考](https://www.cnblogs.com/zhaoguhong/p/7074597.html?utm_source=itdadao&utm_medium=referral)(侵删)