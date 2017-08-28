---
layout: post
title: "常用SQL函数"
date: 2016-06-01 13:21:29 +0800
categories: notes
author: yangxiangming
tags: [sql]
---

`SQL`拥有很多常用的计数&计算的内置函数……
<!-- more -->
函数的语法

```bash
SELECT function(column) FROM `表`

#Sample
SELECT AVG(column) FROM `表` #返回column的平均值
SELECT COUNT(column) FROM `表` #返回column的行数(不包括 NULL 值)
SELECT COUNT(*) FROM `表` #返回被选行数(一般以WHERE为标准)
SELECT MAX(column) FROM `表` #返回column的最高值
SELECT MIN(column) FROM `表` #返回column的最低值
SELECT SUM(column) FROM `表` #返回column的总和
SELECT AVG(column) FROM `表` #返回column的平均值
SELECT ABS(column) FROM `表` #返回column的绝对值
...
```

当然除此之外还有很多可用函数，一切以实际需求所需的场景为准……

`SQL`根据不同参数查询对应相近的数据，这种场景需求很多，比如根据价格、统计、时间戳等字段为条件，查询出和条件相近的数据。

例如相近的数据查询，主要在相近二字，测试表数据如下：

```bash
mysql> select * from sample;
+----+-------------+-------+-------+------------+------------+
| id | title       | price | total | createTime | updateTime |
+----+-------------+-------+-------+------------+------------+
|  1 | sample.no.1 |    13 |    22 | 1396420646 | 1434506775 |
|  2 | sample.no.2 |    21 |    26 | 1396423023 | 1434206709 |
|  3 | sample.no.3 |    23 |    34 | 1396420457 | 1434526780 |
|  4 | sample.no.4 |    21 |    30 | 1396430609 | 1434509801 |
|  5 | sample.no.5 |    10 |    45 | 1396422347 | 1434507875 |
|  6 | sample.no.6 |    15 |    45 | 1396520234 | 1434606824 |
|  7 | sample.no.7 |    67 |    56 | 1396720623 | 1434906790 |
|  8 | sample.no.8 |    23 |    73 | 1397020678 | 1434706771 |
|  9 | sample.no.9 |    88 |    10 | 1397620609 | 1434636732 |
+----+-------------+-------+-------+------------+------------+
9 rows in set (0.00 sec)
```

通过`SQL`中的`ABS`绝对值函数来进行值的查询筛选，如下操作测试：
```bash
mysql> select id,title,createTime,abs(1396489808-createTime) as tmpTime from sample order by tmpTime asc limit 6;
+----+-------------+------------+---------+
| id | title       | createTime | tmpTime |
+----+-------------+------------+---------+
|  6 | sample.no.6 | 1396520234 |   30426 |
|  4 | sample.no.4 | 1396430609 |   59199 |
|  5 | sample.no.5 | 1396422347 |   67461 |
|  2 | sample.no.2 | 1396423023 |   68685 |
|  1 | sample.no.1 | 1396420646 |   69162 |
|  3 | sample.no.3 | 1396420457 |   69351 |
+----+-------------+------------+---------+
6 rows in set (0.00 sec)
```

如上测试可以看出通过`SQL`的`ABS`绝对值函数计算的值在进行`ORDER BY`排序，获取到了和`SQL`条件中的时间戳`1396489808`最相近的`6`条数据，当然除此之外还可以根据表中的`price`、`total`字段进行读取如下事例（这种方法仅适合数字类型数据读取）

```sql
SELECT `id`, `title`, `total`, ABS(50-`total`) AS `tmpTotal` FROM `sample` ORDER BY `tmpTotal` LIMIT 6
SELECT `id`, `title`, `price`, ABS(50-`price`) AS `tmpPrice` FROM `sample` ORDER BY `tmpPrice` LIMIT 6
```
