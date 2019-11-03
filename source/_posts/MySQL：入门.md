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