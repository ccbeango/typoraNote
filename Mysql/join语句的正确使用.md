---

title: Join语句的正确使用

date: 2018-03-02 12:43:10

categories: Mysql

tag: Mysql 

---

# Join语句的正确使用

## 前言

最近对于Mysql使用少之又少，今天刚好看了一点关于Mysql的视频，记录下经常使用的Join语句，了解Join语句的优点~

正确使用SQL语言的重要性

* 增加数据库处理效率，较少应用响应时间。
* 减少数据库服务器负载，增加服务器稳定性。
* 减少服务器间通讯的网络流量。

先上下面例子中讲解要用到的数据表：

1. 西天取经四人组表 `user1`：

   | id   | user_name | title      |
   | ---- | --------- | ---------- |
   | 1    | 唐僧      | 旖檀功德佛 |
   | 2    | 猪八戒    | 净坛使者   |
   | 3    | 孙悟空    | 斗战胜佛   |
   | 4    | 沙僧      | 金身罗汉   |

2. 悟空的结拜兄弟表 `user2`：

   | id   | user_name | title  |
   | ---- | --------- | ------ |
   | 1    | 孙悟空    | 成佛   |
   | 2    | 牛魔王    | 被降服 |
   | 3    | 蛟魔王    | 被降服 |
   | 4    | 鹏魔王    | 被降服 |
   | 5    | 狮驼王    | 被降服 |

3. 按日期记录了`user1`四人组中每个人打怪的数量`user_kills`：

   | Id   | user_id | timestr    | kills |
   | ---- | ------- | ---------- | ----- |
   | 1    | 2       | 2018-06-05 | 10    |
   | 2    | 2       | 2018-06-06 | 2     |
   | 3    | 2       | 2018-07-05 | 12    |
   | 4    | 4       | 2018-02-05 | 3     |
   | 5    | 2       | 2018-07-15 | 6     |
   | 6    | 2       | 2018-06-13 | 2     |
   | 7    | 3       | 2018-04-28 | 12    |
   | 8    | 2       | 2018-09-04 | 10    |
   | 9    | 2       | 2018-03-05 | 18    |

之后通过这三张表，来帮助我们理解Join从句的使用方式。

## 如何正确的使用Join从句

Join从句的类型：

* 内连接（INNER）
* 全外连接（FULL OUTER）
* 左外连接（LEFT OUTER）
* 右外连接（RIGHT OUTER）
* 交叉连接（CROSS）

### INNER JOIN

​	内连接`INNER JOIN`基于连接谓词将两种表（如A和B）的列组合在一起，产生新的表结果。

![](https://raw.githubusercontent.com/ccbeango/blogImages/master/Mysql/join语句的正确使用01.png)

```mysql
SELECT <select_list> FROM TableA A INNER JOIN TableB B ON A.Key = B.KEY;
```

例子：

取出取经四人组`user1`和悟空朋友们`user2`这两张表中同时存在于两张表中的用户记录。

```mysql
SELECT a.user_name,a.title,b.title FROM user1 a INNER JOIN user2 b on a.user_name = b.user_name;
+-----------+--------------+--------+
| user_name | title        | title  |
+-----------+--------------+--------+
| 孙悟空     | 斗战胜佛      | 成佛    |
+-----------+--------------+--------+
1 row in set (0.00 sec)
```

### LEFT JOIN

以A表为基础，包含A表中所有的数据与B表中谓词连接词相等的数据。

![](https://raw.githubusercontent.com/ccbeango/blogImages/master/Mysql/join语句的正确使用02.png)

```mysql
SELECT <select_list> FROM TableA A LEFT JOIN TABLEB B ON A.Key = B.Key;
```

以A表为基础，查询只存在A表中的数据。

![](https://raw.githubusercontent.com/ccbeango/blogImages/master/Mysql/join语句的正确使用03.png)

```mysql
SELECT <select_list> FROM TableA A LEFT JOIN TableB B ON A.Key = B.Key WHERE B.Key IS NULL;
```

在使用`not in`时，不可以使用索引优化，使用`left join`这种方法可以用来优化`not in `查询。

例子：

查询取经四人组中哪些人不是悟空的结拜兄弟？

```mysql
SELECT a.user_name, a.title, b.title FROM user1 a LEFT JOIN user2 b ON a.user_name = b.user_name WHERE b.user_name is NULL;
+-----------+-----------------+-------+
| user_name | title           | title |
+-----------+-----------------+-------+
| 唐僧      | 旖檀功德佛        | NULL  |
| 猪八戒    | 净坛使者          | NULL  |
| 沙僧      | 金身罗汉          | NULL  |
+-----------+-----------------+-------+
3 rows in set (0.00 sec)
```

### RIGHT JOIN



![](https://raw.githubusercontent.com/ccbeango/blogImages/master/Mysql/join语句的正确使用04.png)

```mysql
SELECT <select_list> FROM TableA A RIGHT JOIN TableB B ON A.Key = B.key
```

![](https://raw.githubusercontent.com/ccbeango/blogImages/master/Mysql/join语句的正确使用05.png)

```mysql
SELECT <select_list> FROM TableA A RIGHT JOIN TableB B ON A.Key = B.Key WHERE A.Key IS NULL;
```

例子：

悟空的结拜兄弟中哪些人没有去取经？

```mysql
SELECT b.user_name,b.title,a.title FROM user1 A RIGHT JOIN user2 b ON a.user_name=b.user_name WHERE a.user_name is NULL;
+-----------+-----------+-------+
| user_name | title     | title |
+-----------+-----------+-------+
| 牛魔王     | 被降服     | NULL  |
| 蛟魔王     | 被降服     | NULL  |
| 鹏魔王     | 被降服     | NULL  |
| 狮驼王     | 被降服     | NULL  |
+-----------+-----------+-------+
4 rows in set (0.00 sec)
```

### FULL JOIN

查询A、B表所有的数据都显示出来，如果连接谓词中无法对应它们，则以`NULL`来填充。

![](https://raw.githubusercontent.com/ccbeango/blogImages/master/Mysql/join语句的正确使用06.png)

```mysql
SELECT <select_list> FROM TableA A FULL OUTER JOIN TableB B ON A.Key = B.Key;
```

查询只存在于A或B表中的数据，即过滤A、B表的公共部分。

![](https://raw.githubusercontent.com/ccbeango/blogImages/master/Mysql/join语句的正确使用07.png)

```mysql
SELECT <select_list> FROM TableA A FULL OUTER JOIN TableB B ON A.Key = B.Key WHERE A.Key IS NULL OR B.Key IS NULL;
```

**问题**：

在mysql中执行`FULL JOIN`会报错`1064`：

```mysql
SELECT a.user_name,a.title,b.title FROM user1 a FULL JOIN user2 b ON a.user_name=b.user_name;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'FULL JOIN user2 b ON a.user_name=b.user_name' at line 1
```

这是因为`MySQL`中不支持`FULL JOIN`的连接。如果想要在`MySQL`中使用`FULL JOIN`，可以通过使用 `UNION ALL`来解决。

```mysql
SELECT a.user_name,a.title,b.title FROM user1 a LEFT JOIN user2 b ON a.user_name=b.user_name UNION ALL SELECT b.user_name,b.title,a.title FROM user1 a RIGHT JOIN user2 b ON b.user_name=a.user_name;
+-----------+-----------------+--------------+
| user_name | title           | title        |
+-----------+-----------------+--------------+
| 孙悟空    | 斗战胜佛        | 成佛         |
| 唐僧      | 旖檀功德佛      | NULL         |
| 猪八戒    | 净坛使者        | NULL         |
| 沙僧      | 金身罗汉        | NULL         |
| 孙悟空    | 成佛            | 斗战胜佛     |
| 牛魔王    | 被降服          | NULL         |
| 蛟魔王    | 被降服          | NULL         |
| 鹏魔王    | 被降服          | NULL         |
| 狮驼王    | 被降服          | NULL         |
+-----------+-----------------+--------------+
9 rows in set (0.01 sec)
```

> UNION 操作符用于合并两个或多个 SELECT 语句的结果集。
>
> 请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。
>
> ### SQL UNION 语法
>
> ```mysql
> SELECT column_name(s) FROM table_name1
> UNION
> SELECT column_name(s) FROM table_name2
> ```
>
> 注释：默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。
>
> ### SQL UNION ALL 语法
>
> ```mysql
> SELECT column_name(s) FROM table_name1
> UNION ALL
> SELECT column_name(s) FROM table_name2
> ```
>
> 另外，UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。

### CROSS JOIN

交叉连接，又称笛卡尔连接（cartesian join）或叉乘（Product）,如果`A`和`B`是两个集合，它们的交叉连接就记为：`A×B`。

```mysql
SELECT a.user_name,a.title,a.user_name,b.title FROM user1 a CROSS JOIN user2 b;
+-----------+-----------------+-----------+-----------+
| user_name | title           | user_name | title     |
+-----------+-----------------+-----------+-----------+
| 唐僧      | 旖檀功德佛      | 唐僧      | 成佛      |
| 猪八戒    | 净坛使者        | 猪八戒    | 成佛      |
| 孙悟空    | 斗战胜佛        | 孙悟空    | 成佛      |
| 沙僧      | 金身罗汉        | 沙僧      | 成佛      |
| 唐僧      | 旖檀功德佛      | 唐僧      | 被降服    |
| 猪八戒    | 净坛使者        | 猪八戒    | 被降服    |
| 孙悟空    | 斗战胜佛        | 孙悟空    | 被降服    |
| 沙僧      | 金身罗汉        | 沙僧      | 被降服    |
| 唐僧      | 旖檀功德佛      | 唐僧      | 被降服    |
| 猪八戒    | 净坛使者        | 猪八戒    | 被降服    |
| 孙悟空    | 斗战胜佛        | 孙悟空    | 被降服    |
| 沙僧      | 金身罗汉        | 沙僧      | 被降服    |
| 唐僧      | 旖檀功德佛      | 唐僧      | 被降服    |
| 猪八戒    | 净坛使者        | 猪八戒    | 被降服    |
| 孙悟空    | 斗战胜佛        | 孙悟空    | 被降服    |
| 沙僧      | 金身罗汉        | 沙僧      | 被降服    |
| 唐僧      | 旖檀功德佛      | 唐僧      | 被降服    |
| 猪八戒    | 净坛使者        | 猪八戒    | 被降服    |
| 孙悟空    | 斗战胜佛        | 孙悟空    | 被降服    |
| 沙僧      | 金身罗汉        | 沙僧      | 被降服    |
+-----------+-----------------+-----------+-----------+
20 rows in set (0.00 sec)
```

## Join使用技巧

### 使用Join更新表

如何更新使用过滤条件中包含自身的表？

情景：

​	把同时存在于取经四人组和悟空兄弟表中的记录的人在取经四人组表中的`title`字段更新为“齐天大圣”。

最容易想到的是：

```mysql
UPDATE user1 SET title='齐天大圣' WHERE user1.user_name IN (SELECT b.user_name FROM user1 a INNER JOIN user2 b ON a.user_name=b.user_name);
ERROR 1093 (HY000): You can't specify target table 'user1' for update in FROM clause
```

会报错不能更新`FROM`从句中出现的表，`MySQL`不支持这种更新。如果放在`Oracle`或`SQLServer`中，是完全可以执行的。

**联合更新**，在`MySQL`中这种方式更新可以使用`JOIN`：

```mysql
UPDATE user1 a JOIN (SELECT b.user_name FROM user1 a INNER JOIN user2 b ON a.user_name=b.user_name) b ON a.user_name=b.user_name SET a.title='齐天大圣';
```

利用`join`进行更新，其实就是将`user1`与`user2`表中查询出的结果进行联合，然后进行更新。

在实际工作中如果遇到需要更新自身表的情况，即==更新出现在`FROM`从句中的表的话==(???)，就可使用此方法。

### 使用JOIN优化子查询

查询存在于**取经四人组**和**悟空的结拜兄弟**的数据：

1. 利用子查询：

```mysql
SELECT a.user_name,a.title,(SELECT b.title FROM user2 b WHERE a.user_name=b.user_name) AS title2 FROM user1 a;
+-----------+-----------------+--------+
| user_name | title           | title2 |
+-----------+-----------------+--------+
| 唐僧       | 旖檀功德佛       | NULL   |
| 猪八戒     | 净坛使者         | NULL   |
| 孙悟空     | 斗战胜佛         | 成佛    |
| 沙僧       | 金身罗汉         | NULL   |
+-----------+-----------------+--------+
4 rows in set (0.00 sec)
```

​		如果两张表中的数据非常多，那么`user1`表中的每一条数据都要进行一次子查询，那么这种查询方式是非常低效的。

2. 使用联合查询：

```mysql
SELECT a.user_name,a.title,b.title AS title2 FROM user1 a LEFT JOIN user2 b ON a.user_name=b.user_name;
+-----------+-----------------+--------+
| user_name | title           | title2 |
+-----------+-----------------+--------+
| 唐僧       | 旖檀功德佛       | NULL   |
| 猪八戒     | 净坛使者         | NULL   |
| 孙悟空     | 斗战胜佛         | 成佛    |
| 沙僧       | 金身罗汉         | NULL   |
+-----------+-----------------+--------+
4 rows in set (0.00 sec)
```

### 使用JOIN优化聚合子查询

如何查询出四人组中打怪最多的日期？

1. 聚合子查询

```mysql
SELECT a.user_name,b.timestr,b.kills FROM user1 a INNER JOIN user_kills b ON a.id=b.user_id WHERE b.kills=(SELECT MAX(kills) FROM user_kills c WHERE c.user_id=b.user_id); 
+-----------+------------+-------+
| user_name | timestr    | kills |
+-----------+------------+-------+
| 猪八戒     | 2018-03-05 |    18 |
| 孙悟空     | 2018-04-28 |    12 |
| 沙僧       | 2018-02-05 |     3 |
+-----------+------------+-------+
3 rows in set (0.01 sec)
```

2. JOIN优化查询

```mysql
SELECT a.user_name,b.timestr,b.kills FROM user1 a JOIN user_kills b ON a.id=b.user_id JOIN user_kills c ON c.user_id=b.user_id GROUP BY a.user_name,b.timestr,b.kills HAVING b.kills=MAX(c.kills);
+-----------+------------+-------+
| user_name | timestr    | kills |
+-----------+------------+-------+
| 孙悟空    | 2018-04-28 |    12 |
| 沙僧      | 2018-02-05 |     3 |
| 猪八戒    | 2018-03-05 |    18 |
+-----------+------------+-------+
3 rows in set (0.00 sec)
```

​	这个例子中`user_kills`这张表关联了两次，目的是第一次是为了查询出最终的结果集==时间==和==杀怪数量==，第二次是为了跟`HAVING`从句中使用来得出最大的杀怪数量是多少。

### 实现分组选择



