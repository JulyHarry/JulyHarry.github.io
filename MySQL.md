# MySQL

[toc]

# Database

| 关键字 | 功能         | 方法                                                         |
| ------ | ------------ | ------------------------------------------------------------ |
| CREATE | 建库         | **CREATE DATABASE** _dbname_;<br />**CREATE DATABASE IF NOT EXISTS** *dbname*; |
| DROP   | 删库         | **DROP DATABASE** *dbname*;<br />**DROP DATABASE IF EXISTS** *dbname*; |
| USE    | 切换到数据库 | USE _dbname_;                                                |

# Table

| 关键字           | 功能                           | 方法                                                         |
| ---------------- | ------------------------------ | ------------------------------------------------------------ |
| CREATE           | 建表                           | **CREATE TABLE** _tbname_ (<br />_columnA INT NOT NULL AUTO_INCREMENT PRIMARY KEY,_<br />_columnB VARCHAR(20) NULL DEFAULT 'something',_<br />_columnC DATE,_<br />); |
| INSERT           | 插入数据                       | **INSERT INTO** _tbname_ **(**_columnA, columnB, columnC_**)** <br />**VALUES**  **(**‘_valueA1_’**,**  ‘_valueB1_’**,** ‘_valueC1_’**)**,<br />&emsp;&emsp;&emsp;&emsp;&ensp;(‘_valueA2_’**,**  ‘_valueB2_’**,** ‘_valueC2_’),<br /> &emsp;&emsp;&emsp;&emsp;&ensp;(‘_valueA3_’**,**  ‘_valueB3_’**,** ‘_valueC3_’); |
| UPDATE           | 更新数据                       | **UPDATE** _tbname_ **SET** _columnA_='_valueA_', _columnB_='_valueB_' <br />_WHERE_ _conditon_; |
| DELETE           | 删除数据                       | --删除后插入数据从**断点后继续**<br />**DELETE FROM** *tbname WHERE condition*; |
| TRUNCATE         | 重建表<br />(**删除所有数据**) | --删除**效率高**<br />--删除后插入数据**从0继续**<br />**TRUNCATE TABLE** *tbname*; |
| DESCRIBE         | 查看表结构                     | **DESCRIBE** *tbname*;<br />**SHOW COLUMNS FROM** *tbname*;  |
| [ALTER](##Alter) | 改表                           | **ALTER TABLE** *tbname* **ADD COLUMN** *columnD char(10)*;  |
| LIKE             | 复制表                         | **CREATE TABLE** *newtable* **LIKE** *tbname*;                      --仅复制表结构<br />**CREATE TABLE** *newtable* **SELECT * FROM** *tbname*; --复制表结构和数据 |

## 约束条件

| 关键字      | 功能     | 用法                                                         |
| ----------- | -------- | ------------------------------------------------------------ |
| ~~CHECK~~   | ~~检查~~ | ~~--插入数据必须满足条件才能插入~~<br />age INT* **CHECK(** *age BETWEEN 0 AND 100* **)**~~ |
| DEFAULT     | 默认值   | *gender CHAR* **DEFAULT** *'M'*                              |
| FOREIGN KEY | 外键     | --限制两个表的关系，要求外键列的值一定来自于主表的关联列<br />--从表的外键列和主表的关联列类型必须一致<br />--主表的关联列~~一定~~(最好)是主键 (MySQL可以关联唯一键)<br />**CONSTRAINT *fkname* FOREIGN KEY (*colFK*) REFERENCES *tbname*(*colKey*);** |
| NOT NULL    | 非空     | *name VARCHAR(5)* **NOT NULL**                               |
| PRIMARY KEY | 主键     | *id INT* **PRIMARY KEY**<br />**PRIMARY KEY(** *id, name***)** |
| UNIQUE      | 唯一     | *phone CHAR(10)* **UNIQUE**<br />**UNIQUE(** *name, phone* **)** |

## Alter

| 关键字    | 功能         | 用法                                                         |
| --------- | ------------ | ------------------------------------------------------------ |
| ADD       | 添加字段     | **ALTER TABLE** *tbname* **ADD COLUMN** *columnD char(10)*;  |
| CHANGE    | 修改字段名   | **ALTER TABLE** *tbname* **CHANGE COLUMN** *columnA columnB*; |
| DROP      | 删除字段     | **ALTER TABLE** *tbname* **DROP COLUMN** *columnB*;          |
| MODIFY    | 修改字段类型 | **ALTER TABLE** *tbname* **MODIFY COLUMN** *columnC int*;    |
| RENAME TO | 修改表名     | **ALTER TABLE** *tbname* **RENAME TO** *newname*;            |
|           | 多字段增删改 | **ALTER TABLE** *tbname* *ADD COLUMN columnD char(10)***,** <br /> *MODIFY COLUMN columnC int***,**  *DROP COLUMN columnB*; |





# Select

## 语法

| 语法顺序                                                     | 执行顺序                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| SELECT<br />FROM<br />JOIN<br />ON<br />WHERE<br />GROUP BY<br />HAVING<br />ORDER BY<br />LIMIT | FROM<br />JOIN<br />ON<br />WHERE<br />GROUP BY<br />HAVING<br />SELECT<br />ORDER BY<br />LIMIT |

## 关键字

| 关键字            | 功能                             | 方法                                                         |
| ----------------- | -------------------------------- | ------------------------------------------------------------ |
| ALL               | 全部查询                         | *SELECT \* FROM tbname WHERE colA >* **ALL** *(10, 20, 30)*;<br />--相当于 <u>colA > max(10, 20, 30)</u><br />*SELECT \* FROM tbname WHERE colA >* **ALL** *(SELECT ...)*; |
| ANY / SOME        | 任意查询                         | *SELECT \* FROM tbname WHERE colA >* **ANY** *(10, 20, 30)*;<br />--相当于 <u>colA > min(10, 20, 30)</u><br />*SELECT \* FROM tbname WHERE colA >* **ANY** *(SELECT ...)*;<br />*SELECT \* FROM tbname WHERE colA =* **ANY** *(10, 20, 30)*;<br />--相当于 <u>colA IN (10, 20, 30)</u>; |
| AS                | 别名                             | *SELECT colA* **AS** *a FROM tbname*;<br />*SELECT colA a FROM tbname*;<br />--MySQL中`AS`可以省略 |
| BETWEEN AND       | 从..到...的值                    | *SELECT \* FROM tbname WHERE colA* **BETWEEN** *10* **AND** *100*;<br />--数值有小到大，**不可写反** |
| DISTINCT          | 不同的值                         | *SELECT* **DISTINCT** *colA FROM tbname*;                    |
| ESCAPE            | 声明转义字符                     | *SELECT \* FROM tbname WHERE colA='a''* **ESCAPE** *'a'*;<br />--查询colA的等于`'`的，`a`作为转义符，相当于`\` |
| EXISTS            | 存在                             | *SELECT* **EXISTS** *(SELECT ...)*; --0: 不存在 1:存在       |
| GROUP BY          | 分组                             |                                                              |
| HAVING            | 分组的筛选                       |                                                              |
| IN                | 包含                             | *SELECT \* FROM tbname WHERE colA* **IN** *('valueA', 'valueB', 'valueC')*;<br />*SELECT \* FROM tbname WHERE colA* **IN** *(SELECT ...)*;<br />*SELECT \* FROM tbname WHERE* ==*(colA, colB)* **IN**<br /> *(SELECT a, b FROM tbname1)*== |
| [JOIN ON](##JOIN) | 关联查询                         | *SELECT \* FROM a* **JOIN** *b* **ON** *a.key = b.key*; <br />*SELECT \* FROM a* **LEFT JOIN** *b* **ON** *a.key = b.key*;<br />*SELECT \* FROM a*  **JOIN** *b* **ON** *a.key = b.key* **JOIN** *c ON b.key = c.key*; |
| LIKE              | 通配表示                         | *SELECT \* FROM tbname WHERE colA* **LIKE** *'a_'*;<br />`-`匹配**一个**字符<br />*SELECT \* FROM tbname WHERE colA* **LIKE** *'a%'*;<br />`%`匹配**任意**字符(**可为0**) |
| LIMIT             | 限制行数                         | *SELECT \* FROM tbname ORDER BY colA* **LIMIT m**;<br />--前m行<br />*SELECT \* FROM tbname ORDER BY colA* **LIMIT m, n**;<br />--第==**m+1**==行到第**m+n**行<br />*SELECT \* FROM tbname ORDER BY colA* **LIMIT n OFFSET m**;<br />--第==**m+1**==行到第**m+n**行<br />*SELECT \* FROM tbname ORDER BY colA* **LIMIT (page-1)*size, size**; |
| NOT               | 不在                             | *SELECT \* FROM tbname WHERE colA* **NOT** *IN (1, 2, 3, 4)*;<br />*SELECT \* FROM tbname WHERE colA* **NOT** *BETWEEN 1 AND 4*;<br />*SELECT \* FROM tbname WHERE* **NOT** *(colA = 1 OR colA=2)*; |
| ORDER BY          | 排序                             | *SELECT \* FROM tbname* **ORDER BY** *colA* ;             --升序<br />*SELECT \* FROM tbname* **ORDER BY** *colA* **DESC**;   --降序<br />*SELECT \* FROM tbname* **ORDER BY** *colA* **DESC**, *colB, colC* **DESC**;<br />*SELECT \* FROM tbname* **ORDER BY** *2*;                   --按第`2`列升序 |
| SELECT            | 查询                             | **SELECT** *\* FROM tbname*;                                 |
| UNION             | 联合查询                         | --*SELECT ...* 的**查询列数要保持一致**，字段类型、字段意义最好一致<br />*SELECT ...* **UNION** *SELECT...* **UNION** *SELECT...*<br />--==**自动去重**<br />==*SELECT ...* **UNION ALL** *SELECT...* **UNION ALL** *SELECT...*<br />--**不去重** |
| <=>               | 安全等于<br />(**可以判断NULL**) |                                                              |
| <>                | 不等于                           |                                                              |

## JOIN

| 查询关系                                                     | 语法                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![截屏2020-11-23 上午11.32.16](/Users/hang/Library/Mobile Documents/com~apple~CloudDocs/Documents/Notes/Pics/截屏2020-11-23 上午11.32.16.png) | SELECT * FROM a **JOIN** b **ON** a.key = b.key;             |
| ![截屏2020-11-23 上午11.32.30](/Users/hang/Library/Mobile Documents/com~apple~CloudDocs/Documents/Notes/Pics/截屏2020-11-23 上午11.32.30.png) | SELECT * FROM a **LEFT JOIN** b **ON** a.key = b.key;        |
| ![截屏2020-11-23 上午11.32.37](/Users/hang/Library/Mobile Documents/com~apple~CloudDocs/Documents/Notes/Pics/截屏2020-11-23 上午11.32.37.png) | SELECT * FROM a **RIGHT JOIN** b **ON** a.key = b.key;       |
| ![截屏2020-11-23 上午11.32.44](/Users/hang/Library/Mobile Documents/com~apple~CloudDocs/Documents/Notes/Pics/截屏2020-11-23 上午11.32.44.png) | SELECT * FROM a **LEFT JOIN** b **ON** a.key = b.key <br />**WHERE b.key IS NULL**; |
| ![截屏2020-11-23 上午11.32.50](/Users/hang/Library/Mobile Documents/com~apple~CloudDocs/Documents/Notes/Pics/截屏2020-11-23 上午11.32.50.png) | SELECT * FROM a **RIGHT JOIN** b ON a.key = b.key <br />**WHERE a.key IS NULL**; |
| ![截屏2020-11-23 上午11.32.56](/Users/hang/Library/Mobile Documents/com~apple~CloudDocs/Documents/Notes/Pics/截屏2020-11-23 上午11.32.56.png) | ~~SELECT * FROM a **FULL JOIN** b **ON** a.key = b.key;~~    |
| ![截屏2020-11-23 上午11.33.11](/Users/hang/Library/Mobile Documents/com~apple~CloudDocs/Documents/Notes/Pics/截屏2020-11-23 上午11.33.11.png) | ~~SELECT * FROM a **FULL JOIN** b ON a.key = b.key <br />**WHERE a.key IS NULL OR b.key IS NULL**;~~ |





# View

| 关键字 | 功能 | 方法 |
| ------ | ---- | ---- |
|        |      |      |

# Function

| 函数名   | 功能 | 方法                |
| -------- | ---- | ------------------- |
| IFNULL() |      | IFNULL(*colA*, *0*) |

# 注释

1. 单行注释

   * --
   * #

2. 多行注释

   * /* 

     */

3. 版本控制注释

```mysql
/*!40014 SET @OLD_UNIQUE_CHECKS = @@UNIQUE_CHECKS, UNIQUE_CHECKS = 0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS = @@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS = 0 */;
/*!40101 SET @OLD_SQL_MODE = @@SQL_MODE, SQL_MODE = 'NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES = @@SQL_NOTES, SQL_NOTES = 0 */;

/*!50000 USE myemployee */;
```

 Those are code-containing comments, such that the code will be executed
 only if the server is as recent as the version number at the beginning
 of the comment.

http://dev.mysql.com/doc/refman/5.0/en/comments.html

Basically, it's version-specific code for features that are unavailable
 in older servers.

如`/*!50000 USE myemployee */;` MySQL5.0之后的版本才能运行`USE myemployee` 

# `` & ' '

`` `是 MySQL的**转义符**，避免和 mysql 的本身的关键字冲突，只要不在列名、表名中使用 mysql 的保留字或中文，就不需要转义。

所有的数据库都有类似的设置，不过mysql用的是`而已。**通常用来说明其中的内容是数据库名、表名、字段名，不是关键字**。例如：

```mysql
select from from table; #from是一个字段名，table是一个表名
```

第一个from是字段名，最后的table表名，但是<u>同时也是mysql关键字</u>，这样执行会报错，所以应使用

```mysql
select `from` from `table`;
```

当然，为了便于阅读，~~不建议使用关键字作为字段名、表名~~，同时，应对数据库名、表名、字段名用一对儿反引号包含。

~~两者在linux下和windows下不同，linux下不区分，windows下区分。~~

# Delete

## DELETE `p1`

在DELETE官方文档中，给出了这一用法，比如下面这个DELETE语句

```mysql
DELETE t1 FROM t1 LEFT JOIN t2 ON t1.id=t2.id WHERE t2.id IS NULL;
```

这种DELETE方式很陌生，竟然和SELETE的写法类似。它涉及到t1和t2两张表，DELETE t1表示要删除t1的一些记录，具体删哪些，就看WHERE条件，满足就删；

这里删的是t1表中，跟t2匹配不上的那些记录。

所以，官方sql中，DELETE p1就表示从p1表中删除满足WHERE条件的记录。



