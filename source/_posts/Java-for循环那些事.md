---
title: Java-for循环那些事
toc: true
date: 2019-05-21 10:28:44
categories: Java
tags:
    - Tips
---

# Java **for**循环里面的 **i++** 与 **++i** 

**在for循环里两者的作用是一样的**

**i++**
```Java
for(int i=0; i<5; i++){
    System.out.print(i + ",");
}

>> 0, 1, 2, 3, 4
```

**++i**
```Java
for(int i=0; i<5; ++i){
    System.out.print(i + ",");
}

>> 0, 1, 2, 3, 4
```

**工作原理**

i++
```Java
{
    System.out.print(i + ",");
    i++;
}
```

++i
```Java
{
    System.out.print(i + ",");
    ++i;
}
```

**区别**
> 在Java里面，i++ 需要开辟新的存储空间用于存储结果，++i 直接在原存储空间中存储结果。**故 **++i** 在 for 循环里面执行效率要高。** 可以作为代码优化的一部分。

# foreach 与 for循环的效率

1. **首先测试数组**

    ```Java
    // 测试for循环
    int[] A = new int[100000000];
    long startTime = System.nanoTime();
    int len = A.length;
    int res = 0;
    for(int i=0; i<len; i++){
        res += A[i];
    }
    long endTime = System.nanoTime();
    System.out.println("for循环: "+(endTime- startTime));

    // 测试foreach
    int[] A = new int[100000000];
    long startTime = System.nanoTime();
    int len = A.length;
    int res = 0;
    for(Integer i: A){
        res += i;
    }
    long endTime = System.nanoTime();
    System.out.println("foreach循环: "+(endTime- startTime));
    ```

    **结果**

    ```Java
    for循环: 37143324
    forecah循环: 75450311
    ```

    可见对于大数组采用**for循环效率**更高

2. **再测试ArrayList**

    ```Java
    ArrayList<Integer> list = new ArrayList<>(30000000);
        for(int i=0; i<30000000; i++){
            list.add(i);
        }
    // 测试for循环
    long startTime = System.nanoTime();
    int size = list.size();
    int res = 0;
    for(int i=0; i<size; i++){
        res += list.get(i);
    }
    long endTime = System.nanoTime();
    System.out.println("for循环: "+(endTime- startTime));

    // 测试foreach
    long startTime = System.nanoTime();
    int res = 0;
    for(Integer i: list){
            res += i;
        }
    long endTime = System.nanoTime();
    System.out.println("foreach循环: "+(endTime- startTime));
    ```
    **结果**
    ```Java
    for循环: 39368248
    forecah循环: 42177137
    ```

    可见对于ArrayList仍然是采用**for循环效率**更高！

# for循环小技巧

## 关于数组

循环嵌套采用**小套大**。原理跟复制几个大文件跟复制一堆小文件耗时一样。

**测试**

```Java
long startTime = System.nanoTime();
int res = 0;
// 大套小
for(int i=0; i<10000000; i++){
    for(int j=0; j<100; j++){
        res += i;
    }
}
long endTime = System.nanoTime();
System.out.println("大套小: "+(endTime- startTime));
```

```Java
// 小套大
for(int i=0; i<100; i++){
    for(int j=0; j<10000000; j++){
        res += i;
    }
}
long endTime = System.nanoTime();
System.out.println("小套大: "+(endTime- startTime));
```

**结果**
```Java
大套小: 57934223
小套大: 4918044
```

## 关于ArrayList

在循环时，首**先把ArrayList长度size记录下来**。

**测试**

```Java
ArrayList<Integer> list = new ArrayList<>(30000000);
for(int i=0; i<30000000; i++){
    list.add(i);
}
long startTime = System.nanoTime();
//先把长度保存下来
int len = list.size();
int res = 0;
for(int i=0; i<len; i++){
    res += list.get(i);
}
long endTime = System.nanoTime();
System.out.println("保存size(): "+(endTime- startTime));

//没有保存size()
for(int i=0; i<list.size(); i++){
    res += list.get(i);
}
long endTime = System.nanoTime();
System.out.println("没有保存size(): "+(endTime- startTime));
```

**结果**
```Java
保存size(): 38973440
没有保存size(): 39486862
```





