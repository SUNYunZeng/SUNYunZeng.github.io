---
title: Java： i++ and ++i  in for-loop
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