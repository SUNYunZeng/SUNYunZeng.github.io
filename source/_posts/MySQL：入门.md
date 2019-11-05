---
title: MySQL：入门
comments: true
toc: true
date: 2019-10-31 21:52:12
categories: 数据库管理系统
tags: MySQL
---

# MySQL 简介

MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle 旗下产品。MySQL 是最流行的关系型数据库管理系统之一，在 WEB 应用方面，MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件之一。

<center> {% asset_img  my-SQL.png %} </center>

**MySQL具有以下优点：**

 成本——MySQL是开放源代码的，一般可以免费使用（甚至可以 免费修改）。

 性能——MySQL执行很快（非常快）。  可信赖——某些非常重要和声望很高的公司、站点使用MySQL， 这些公司和站点都用MySQL来处理自己的重要数据。

 简单——MySQL很容易安装和使用。

# 基础入门

1. 启动服务

Windows：

```SQL
net start mysql
```

ubuntu:

```SQL
sudo service mysql start     
```

2. 登录

```SQL
mysql -u root -p
*****
```

3. 查看已有数据库

```SQL
SHOW DATABASES;
```

4. 创建数据库

```SQL
CREATE DATABASE;
```

5. 使用数据库

```SQL
USE DATABASE;
```

6. 显示所有表

```SQL
SHOW TABLES;
```

7. 显示已有表的所有列

```SQL
SHOW COLUMNS FROM YOURDATABASE;
```

8. 创建表

```SQL
CREATE TABLE book(name char(20), author char(20));
```

9. 在表中插入数据

```SQL
INSERT INTO book VALUES('C++ Primer', 'Stanley B. Lippman');
INSERT INTO book VALUES('Thinking in JAVA', 'Bruce Eckel');
INSERT INTO book VALUES('Easy Python', 'Kukeel Borant');
```

10. 查看表中数据

```SQL
SELECT * FROM book;
```

# 选择数据 **SELECT**

1. 选择单列

```SQL
SELECT name FROM book;
```

2. 选择多列

```SQL
SELECT name, author FROM book;
```

3. 选择所有列

```SQL
SELECT * FROM book;
```

4. 检索不同行

```SQL
SELECT DISTINCT author FROM book;
```

5. 限制检索的行数

从第一行开始，返回结果不超过3行

```SQL
SELECT author 
FROM book
LIMIT 3
```

从第3行（**第一行为0**）开始，只检索结果不超过3行
```SQL
SELECT author 
FROM book
LIMIT 2，3
```

6. 完全限定的表名

指定从表 book 中选择 author 列
从数据库 library 中选择 book 表

```SQL
SELECT book.author
FROM library.book;
```

# 排序检索数据

关系数据库设计理论认为，如果不明确规定排序顺序，则不应假设检索出得数据的顺序有意义。

SQL语句由字句(clause)构成，有的字句是必需的，而有的是可选的。

1. **OREDR BY** 关键字句排序

默认升序排序

```SQL
SELECT author
FROM book
OREDER BY name;
```

还可以显示多列，添加多列约束，约束规则是在**优先级高的数据有重复时，再按低一级的约束列排序**。

```SQL
SELECT author, name
FROM book
ORDER BY price, name;
```

2. 指定排序方向

**DESC**逆序排列。

只对价格逆序排列

```SQL
SELECT author, name
FROM book
ORDER BY price DESC, name;
```

3. 集合 LIMIT 与 ORDER 挑选最贵书籍

```SQL
SELECT name
FROM book
ORDER BY price DESC
LIMIT 1;
```

# 过滤数据

通过关键字 WHERE 进行数据过滤

操作符 | 说明
:-: | :-:
= | 等于
<> | 不等于
!= | 不等于
< | 小于
> | 大于
<= | 小于等于
>= | 大于等于
BETWEEN | 在指定两个值之间

1. 过滤行

```SQL
SELECT name
FROM book
WHERE name = 'Machine Learning';
```

2. 阈值过滤

```SQL
SELECT name
FROM book
WHERE price BETWEEN 5 AND 10;
```

3. 空值检查

```SQL
SELECT id
FROM book
WHERE price is NULL;
```

## LIKE 通配符搜索

通配符是 WHERE 字句中具有特殊含义的字符

1. % 通配符

% 通配符表示匹配任意字符出现任意次

匹配 Mach 开头的书名

```SQL
SELECT name
FROM book
WHERE name LIKE 'Mach%';
```

2. _ 通配符

_ 通配符表示任意字符出现一次

```SQL
SELECT name
FROM book
WHERE name LIKE '_ach%';
```

# 插入数据

1. 在首列插入新字段

```SQL
ALTER TABLE books
ADD COLUMN price float(5,2) --总共5位，小数2位
NOT NULL FRIST;
```

2. 在某列后插入新字段

```SQL
ALTER TABLE books
ADD COLUMN price float(5,2) --总共5位，小数2位
NOT NULL AFTER author;
```

3. 默认插入最后一列

```SQL
ALTER TABLE books
ADD COLUMN price float(5,2) --总共5位，小数2位
NOT NULL;
```


# 更新数据

一定记得加 **WHERE** 字句限定

1. 更新指定单行与单列 

```SQL
UPDATE book
SET price = 10.23
WHERE id = 0;
```

2. 更新指定多列与单行

```SQL
UPDATE book
SET price = 8.88, --多行之间逗号隔开
    name = 'Machine Learning'
WHERE id = 0;
```

3. 通过设置为NULL 删除某列的值

```SQL
UPDATE book
SET price = NULL
WHERE id = 0;
```

# 删除数据

一定记得加 **WHERE** 字句限定

1. 删除某一行

```SQL
DELECT FROM book
WHERE id = 0;
```

2. 删除某一列

属于重构表结构，谨慎选择

```SQL
ALTER TABLE book
DROP COLUMN price;
```
