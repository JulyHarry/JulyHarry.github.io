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
| DELETE           | 删除数据                       | --删除后插入数据从**断点后继续**<br />--返回删除行数<br />-**支持事务回滚**<br />**DELETE FROM** *tbname WHERE condition*; |
| TRUNCATE         | 重建表<br />(**删除所有数据**) | --删除后插入数据**从0继续**<br />--不返回删除行数<br />--**不支持事务回滚**<br />--删除**效率高**<br />**TRUNCATE TABLE** *tbname*; |
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
| LIMIT             | 限制行数                         | *SELECT \* FROM tbname ORDER BY colA* **LIMIT m**;<br />--前m行<br />*SELECT \* FROM tbname ORDER BY colA* **LIMIT m, n**;<br />--第==**m+1**==行到第**m+n+1**行<br />*SELECT \* FROM tbname ORDER BY colA* **LIMIT n OFFSET m**;<br />--第==**m+1**==行到第**m+n+1**行<br />*SELECT \* FROM tbname ORDER BY colA* **LIMIT (page-1)*size, size**; |
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

# 字符集(需补充)





# 权限管理(需补充角色)

| 功能       | 用法                                                         |
| ---------- | ------------------------------------------------------------ |
| 查看用户   | **SELECT user FROM mysql.user;<br />SELECT * FROM mysql.user;** |
| 创建用户   | **CREATE USER** *myuser* **IDENTIFIED BY** *'mypassword'*;<br />**CREATE USER** 'myuser'@'localhost' **IDENTIFIED BY** *'mypassword'*;<br />--if [ Your password does not satisfy the current policy requirements ]<br />-- SHOW GLOBAL variables LIKE 'validate_password%';<br />-- SET GLOBAL variables validate_password**.**policy = 0;  #MySQL8.0<br />-- SET GLOBAL variables validate_password**_**policy = 0;  #MySQL5.0 |
| 修改用户名 | **RENAME USER** *myuser* **TO** newuser;                     |
| 删除用户   | **DROP USER** *myuser*;<br />**DROP USER** *'myuser'@'localhost'*; |
| 查看权限   | **SHOW GRANTS FOR** *myuser*;<br />**SHOW GRANTS FOR** *'myuser'@'localhost'*; |
| 授予权限   | **GRANT** *ALL PRIVILEGES* **ON** \*.\* **FROM** *'myuser'@'localhost'*;<br />-- \*.\* 第一个*指数据库，第二个*指数据表，可以换位指定的数据库和数据表(e.g. 'dbname'.'tbname' )；<br />-- all privileges 可换成select, update, insert, delete, drop, create等 |
| 收回权限   | **REVOKE** *ALL PRIVILEGES* **ON** \*.\* **FROM** *'myuser'@'localhost'*; |
| 修改密码   | **ALTER USER** *'myuser'@'localhost'* **IDENTIFIED BY** *'newpassword'*;  #MySQL8.0<br /> **SET PASSWORD FOR** *myuser* **=** **Password**('*newpassword*');            #MySQL5.0 |
| 刷新权限   | **FLUSH PRIVILEGES**;                                        |



# 事务

**一个事务是由一条或者多条sql语句构成，这一条或者多条sql语句要么全部执行成功，要么全部执行失败！**

## 四大特性（ACID）

- 原子性（Atomicity）：事务中所有操作是不可再分割的原子单位。事务中所有操作要么全部执行成功，要么全部执行失败。
- 一致性（Consistency）：事务执行后，数据库状态与其它业务规则保持一致。如转账业务，无论事务执行成功与否，参与转账的两个账号余额之和应该是不变的。
- 隔离性（Isolation）：隔离性是指在并发操作中，不同事务之间应该隔离开来，使每个并发中的事务不会相互干扰。
- 持久性（Durability）：一旦事务提交成功，事务中所有的数据操作都必须被持久化到数据库中，即使提交事务后，数据库马上崩溃，在数据库重启时，也必须能保证通过某种机制恢复数据。

## 事务步骤

1. 取消自动提交

   * SHOW variables LIKE 'auto%';

     * | Variable\_name             | Value |
       | :------------------------- | :---- |
       | auto\_generate\_certs      | ON    |
       | auto\_increment\_increment | 1     |
       | auto\_increment\_offset    | 1     |
       | autocommit                 | ON    |
       | automatic\_sp\_privileges  | ON    |

   * SET autocommit = 0;

   autocommit针对每一个连接，不是针对服务器

2. 开启事务

   * START TRANSACTION;

   当出现`START TRANSACTION;`时，会自动关闭隐式提交。

3. SQL实现语句

4. (设置保留点 SAVEPOINT)

5. 关闭事务

   * COMMIT;      --提交
   * ROLLBACK; --回滚

```mysql
START TRANSACTION;
// ...
SAVEPOINT delete1;
// ...
ROLLBACK TO delete1;
// ...
COMMIT;
```

不能回退select语句（无意义），**不能回退`CREATE`, `DROP`语句**。

# JDBC

JDBC ( **J**ava **D**ata**b**ase **C**onnectivity )是一个独立于特定数据库管理系统（DBMS）、通用的SQL数据库存取和操作的公共接口（一组API），定义了用来访问数据库的标准Java类库，使用这个类库可以以一种标准的方法、方便地访问数据库资源。

JDBC为访问不同的数据库提供了一种统一的途径，目标是使程序员使用JDBC可以连接任何提供了JDBC驱动程序的数据库系统。

## 步骤

总体实例：

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.sql.*;
import java.util.Properties;

public class Mysql8 {
    public static void main(String[] args) throws IOException, ClassNotFoundException, SQLException {
        Connection connection = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        String sql = "";

        Properties info = new Properties();
        info.load(new FileInputStream("Mysql/src/jdbc.properties"));  //ModulesName/src/file
        String user = info.getProperty("user");
        String password = info.getProperty("password");
        String url = info.getProperty("url");
        String driver = info.getProperty("driver");

        //1. 加载驱动
        Class.forName(driver);

        //2. 连接数据库
        connection = DriverManager.getConnection(url, user, password);

        //3 编写SQL
        sql = "select id, name, gender from employee where age = ? and location = ?";

        //4. 获取执行SQL语句命令对象
        ps = connection.prepareStatement(sql);
        ps.setInt(1, 18);
        ps.setString(2, "Beijing");

        //5. 使用命令对象执行sql语句
        rs = ps.executeQuery();
        while (rs.next()) {
            int id = rs.getInt(1);
            String name = rs.getString("name");
            Object gender = rs.getObject(3);
            System.out.println(id + name + gender);
        }

        //6.关闭连接 (后用先关)
        rs.close();
        ps.close();
        connection.close();
    }
}
```

### 1 加载驱动

```java
//方式1：反射
Class.forName("com.mysql.cj.jdbc.Driver"); //MySQL6.0之后使用 cj
Class.forName("com.mysql.jdbc.Driver");    //MySQL5.0使用

//方式2：new 对象 （不推荐）
import com.sql.DriverManager;
import com.mysql.cj.jdbc.Driver;           //MySQL6.0之后使用 cj
import com.mysql.jdbc.Driver;              //MySQL5.0使用
DriverManager.registerDriver(new Driver());
```

### 2 连接数据库

```java
//方式1：url格式为 "jdbc:mysql://localhost:3306/databaseName"
//端口查询：mysql 中 show variables like 'port';
Connection connection = DriverManager.getConnection("url", "user", "password");

//方式2：url格式为 "jdbc:mysql://localhost:3306/databaseName?user=root&password=root"
Connection connection = DriverManager.getConnection("url")
```

### 3 编写SQL

```java
//方式1：带参数的SQL语句，使用PreparedStatement执行
String sql = "select * from employee where id = ? and name = ?";

//方式2：完整的拼接好的SQL语句，使用Statement执行
int id = 20;
String name = "tom";
String sql = "select * from employee where id = " + id + " and name = " + name;
```

### 4 获取执行SQL的编译对象

```java
//方式1：PreparedStatement
PreparedStatement ps = connection.prepareStatement(sql); //预编译
ps.setString(2, "tom"); //序号表示第几个参数
ps.setInt(1, 20);
ps.setObject(3, Object);  //不知道类型时使用

//方式2：Statement
Statement statement = connection.createStatement();
```

注意：

* **Prepared**Statement ps = connection.**prepare**Statement(sql); 两个**拼写不一样**

* PreparedStatement ps = connection.prepareStatement(**sql**);   **必须有参数**

  Statement statement = connection.createStatement();              没有参数

### 5 执行SQL

```java
//增删改
ps.executeUpdate()；
statement.executeUpdate(sql);

//查
ResultSet rs = ps.executeQuery();
ResultSet rs = statement.executeQuery(sql);
//返回单一行
if (rs.next()) {                  //next会不断指向下一行; previous会指向上一行(几乎不用)
  String name = rs.getString(1);  //序号数是指select语句中对应的列值，不是数据表中对应的列值
  int id = rs.getInt("id");       //也可以直接使用列名直接查询
  Object ob = rs.getObject(3);    //不清楚类型时可以使用Object替代
}
//返回多行
while (rs.next()) {
  String name = rs.getString(1);
  int id = rs.getInt("id");
  Object ob = rs.getObject(3);
}

//都可以执行,但是返回BOOLEAN,不推荐
ps.execute();
statement.execute(sql);
```

### 6 关闭连接

```java
//本着Stack(后用先关)的原则
rs.close();
statement.close();
ps.close();
connection.close();
```

## PreparedStatement VS Statement

PreparedStatement

* **不拼接SQL语句**，减少语法错误，语义性强
* SQL的String部分与**参数部分分离**，提高维护性
* 有效**解决SQL注入**问题
* **效率高**，大大减少编译次数（运行次数不变）



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



