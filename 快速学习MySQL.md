## MySQL简介
MySQL 是一个开源的关系型数据库管理系统（RDBMS），现在属于 Oracle 公司管理。MySQL 是最流行的关系型数据库之一，被广泛应用于各种网站和应用程序中。

以下是 MySQL 的一些主要特点和优势：

1.  **开源性**：MySQL 是开源的，可以免费使用，同时也有企业版提供商业支持。 
2.  **跨平台性**：MySQL 可以在多种操作系统上运行，包括 Windows、Linux、Mac 等。 
3.  **性能优化**：MySQL 以其高性能而闻名，通过优化的存储引擎（如 InnoDB、MyISAM 等）和查询优化器，可以处理大规模数据并提供高效的查询处理。 
4.  **支持标准 SQL**：MySQL 遵循 SQL 标准，支持大多数 SQL 语法和功能，使得开发人员可以轻松地迁移和管理数据。 
5.  **扩展性**：MySQL 支持主从复制、分区表、分布式数据库等高级特性，以满足不同规模和需求的应用。 
6.  **社区支持**：由于 MySQL 是开源的，拥有庞大的社区支持，用户可以从社区中获取技术支持、文档和资源。 
7.  **安全性**：MySQL 提供了各种安全功能，如用户身份验证、数据加密、权限控制等，保护数据免受未经授权的访问和攻击。 
8.  **灵活性**：MySQL 支持多种编程语言和开发框架，如 PHP、Python、Java 等，使得开发人员可以灵活选择适合自己项目的开发环境。 

总的来说，MySQL 是一个功能强大、性能优越、易于使用和管理的关系型数据库管理系统，适用于各种规模和类型的应用程序开发。

## 安装mysql 服务端，客户端
### macos
+ homebrew安装

```java
brew install mysql
```

homebrew可切换至国内镜像源

+ 直接从官网下载安装包
+ 使用docker安装

```java
docker pull mysql
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<your_password> -d mysql

```



#### 启动mysql
mysql.server start

### windows
+ 从官网下载安装包
+ 使用docker安装



### mysql客户端
+ mysql workbench
+ navicat (收费)
+ dbeaver（有免费及收费版）



## mysql数据类型
### 数字类型
| <font style="color:rgb(255, 255, 255);">型</font> | <font style="color:rgb(255, 255, 255);">大小</font> |
| --- | --- |
| <font style="color:rgb(51, 51, 51);">TINYINT</font> | <font style="color:rgb(51, 51, 51);">1 Bytes</font> |
| <font style="color:rgb(51, 51, 51);">SMALLINT</font> | <font style="color:rgb(51, 51, 51);">2 Bytes</font> |
| <font style="color:rgb(51, 51, 51);">MEDIUMINT</font> | <font style="color:rgb(51, 51, 51);">3 Bytes</font> |
| <font style="color:rgb(51, 51, 51);">INT或INTEGER</font> | <font style="color:rgb(51, 51, 51);">4 Bytes</font> |
| <font style="color:rgb(51, 51, 51);">BIGINT</font> | <font style="color:rgb(51, 51, 51);">8 Bytes</font> |
| <font style="color:rgb(51, 51, 51);">FLOAT</font> | <font style="color:rgb(51, 51, 51);">4 Bytes</font> |
| <font style="color:rgb(51, 51, 51);">DOUBLE</font> | <font style="color:rgb(51, 51, 51);">8 Bytes</font> |
| <font style="color:rgb(51, 51, 51);">DECIMAL</font> | <font style="color:rgb(51, 51, 51);">对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2</font> |




### 日期和时间类型
| <font style="color:rgb(51, 51, 51);">DATE</font> | <font style="color:rgb(51, 51, 51);">3</font> | <font style="color:rgb(51, 51, 51);">1000-01-01/9999-12-31</font> | <font style="color:rgb(51, 51, 51);">YYYY-MM-DD</font> | <font style="color:rgb(51, 51, 51);">日期值</font> |
| --- | --- | --- | --- | --- |
| <font style="color:rgb(51, 51, 51);">TIME</font> | <font style="color:rgb(51, 51, 51);">3</font> | <font style="color:rgb(51, 51, 51);">'-838:59:59'/'838:59:59'</font> | <font style="color:rgb(51, 51, 51);">HH:MM:SS</font> | <font style="color:rgb(51, 51, 51);">时间值或持续时间</font> |
| <font style="color:rgb(51, 51, 51);">YEAR</font> | <font style="color:rgb(51, 51, 51);">1</font> | <font style="color:rgb(51, 51, 51);">1901/2155</font> | <font style="color:rgb(51, 51, 51);">YYYY</font> | <font style="color:rgb(51, 51, 51);">年份值</font> |
| <font style="color:rgb(51, 51, 51);">DATETIME</font> | <font style="color:rgb(51, 51, 51);">8</font> | <font style="color:rgb(51, 51, 51);">'1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'</font> | <font style="color:rgb(51, 51, 51);">YYYY-MM-DD hh:mm:ss</font> | <font style="color:rgb(51, 51, 51);">混合日期和时间值</font> |
| <font style="color:rgb(51, 51, 51);">TIMESTAMP</font> | <font style="color:rgb(51, 51, 51);">4</font> | <font style="color:rgb(51, 51, 51);">'1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC</font><br/><font style="color:rgb(51, 51, 51);">结束时间是第</font><font style="color:rgb(51, 51, 51);"> </font>**<font style="color:rgb(51, 51, 51);">2147483647</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">秒，北京时间</font><font style="color:rgb(51, 51, 51);"> </font>**<font style="color:rgb(51, 51, 51);">2038-1-19 11:14:07</font>**<font style="color:rgb(51, 51, 51);">，格林尼治时间 2038年1月19日 凌晨 03:14:07</font> | <font style="color:rgb(51, 51, 51);">YYYY-MM-DD hh:mm:ss</font> | <font style="color:rgb(51, 51, 51);">混合日期和时间值，时间戳</font> |


timestamp<font style="color:rgb(33, 37, 41);"> </font><font style="color:rgb(33, 37, 41);">只占 4 个字节，而且是以</font>utc<font style="color:rgb(33, 37, 41);">的格式储存， 它会自动检索当前时区并进行转换。</font>

datetime<font style="color:rgb(33, 37, 41);">以 8 个字节储存，不会进行时区的检索.</font>

<font style="color:rgb(33, 37, 41);">还有一个区别就是如果存进去的是</font>NULL<font style="color:rgb(33, 37, 41);">，</font>timestamp<font style="color:rgb(33, 37, 41);">会自动储存当前时间，而 </font>datetime<font style="color:rgb(33, 37, 41);">会储存 </font>NULL<font style="color:rgb(33, 37, 41);">。</font>

<font style="color:rgb(33, 37, 41);"></font>

### 字符串类型
| <font style="color:rgb(51, 51, 51);">CHAR</font> | <font style="color:rgb(51, 51, 51);">0-255 bytes</font> | <font style="color:rgb(51, 51, 51);">定长字符串</font> |
| --- | --- | --- |
| <font style="color:rgb(51, 51, 51);">VARCHAR</font> | <font style="color:rgb(51, 51, 51);">0-65535 bytes</font> | <font style="color:rgb(51, 51, 51);">变长字符串</font> |
| <font style="color:rgb(51, 51, 51);">TINYBLOB</font> | <font style="color:rgb(51, 51, 51);">0-255 bytes</font> | <font style="color:rgb(51, 51, 51);">不超过 255 个字符的二进制字符串</font> |
| <font style="color:rgb(51, 51, 51);">TINYTEXT</font> | <font style="color:rgb(51, 51, 51);">0-255 bytes</font> | <font style="color:rgb(51, 51, 51);">短文本字符串</font> |
| <font style="color:rgb(51, 51, 51);">BLOB</font> | <font style="color:rgb(51, 51, 51);">0-65 535 bytes</font> | <font style="color:rgb(51, 51, 51);">二进制形式的长文本数据</font> |
| <font style="color:rgb(51, 51, 51);">TEXT</font> | <font style="color:rgb(51, 51, 51);">0-65 535 bytes</font> | <font style="color:rgb(51, 51, 51);">长文本数据</font> |
| <font style="color:rgb(51, 51, 51);">MEDIUMBLOB</font> | <font style="color:rgb(51, 51, 51);">0-16 777 215 bytes</font> | <font style="color:rgb(51, 51, 51);">二进制形式的中等长度文本数据</font> |
| <font style="color:rgb(51, 51, 51);">MEDIUMTEXT</font> | <font style="color:rgb(51, 51, 51);">0-16 777 215 bytes</font> | <font style="color:rgb(51, 51, 51);">中等长度文本数据</font> |
| <font style="color:rgb(51, 51, 51);">LONGBLOB</font> | <font style="color:rgb(51, 51, 51);">0-4 294 967 295 bytes</font> | <font style="color:rgb(51, 51, 51);">二进制形式的极大文本数据</font> |
| <font style="color:rgb(51, 51, 51);">LONGTEXT</font> | <font style="color:rgb(51, 51, 51);">0-4 294 967 295 bytes</font> | <font style="color:rgb(51, 51, 51);">极大文本数据</font> |


## <font style="color:rgb(64, 64, 64);">连接数据库</font>
```plain
>mysql -u用户名 -p密码
```

```plain
>mysql -u用户名 -p
Enter password: ******
```



## 数据库管理
+ <font style="color:rgb(64, 64, 64);">查看所有数据库</font>

```dart
mysql> show databases;
```

+ <font style="color:rgb(64, 64, 64);">创建数据库</font>

```plain
mysql> create database 数据库名称;
```

+ <font style="color:rgb(64, 64, 64);">创建指定字符集数据库</font>

```cpp
mysql> create database 数据库名称 character set utf8;
```

+ <font style="color:rgb(64, 64, 64);">查看数据库的字符集</font>

```dart
mysql> show create database 数据库名称;
```

+ <font style="color:rgb(64, 64, 64);">删除数据库</font>

```rust
mysql> drop database 数据库名称;
```

  


## <font style="color:rgb(64, 64, 64);">表管理</font>
+ <font style="color:rgb(64, 64, 64);">切换使用的数据库</font>

```php
mysql> use 数据库名称;
```

+ <font style="color:rgb(64, 64, 64);">查看所有表</font>

```dart
mysql> show tables;
```

+ <font style="color:rgb(64, 64, 64);">创建表</font>

```plain
mysql> create table 表名(`字段名` 列类型 [属性] [索引] [注释]);

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    birthdate DATE,
    is_active BOOLEAN DEFAULT TRUE
);
```

+ <font style="color:rgb(64, 64, 64);">查看表结构</font>

```plain
mysql> desc 表名;
```

+ <font style="color:rgb(64, 64, 64);">删除表</font>

```rust
mysql> drop table 表名;
```

+ 修改表的列

```csharp
mysql> alter table 表名 add column 列 列的类型;
mysql> alter table 表名 drop column 列名;
mysql> alter table 表名 modify column 列名 新的类型;
mysql> alter table 表名 change column 列名 新的列名 新的类型;
```

  


## <font style="color:rgb(64, 64, 64);">增删改语句</font>
+ <font style="color:rgb(64, 64, 64);">插入数据</font>

```csharp
mysql> insert into 表名(列名,列名) values(值,值);
```

+ <font style="color:rgb(64, 64, 64);">删除表中全部数据</font>

```jsx
mysql> delete from 表名;
```

+ <font style="color:rgb(64, 64, 64);">删除所有满足条件的数据</font>

```csharp
mysql> delete from 表名 where 列名=值 and|or 列名=值;
```

+ <font style="color:rgb(64, 64, 64);">修改表中数据</font>

```bash
mysql> update 表名 set 列=值 where 列=值;
```

## <font style="color:rgb(64, 64, 64);">查询数据</font>
+ <font style="color:rgb(64, 64, 64);">查询指定列数据</font>

```csharp
mysql> select 列名 from 表名;
```

+ <font style="color:rgb(64, 64, 64);">指定常量列（别名）</font>

```csharp
mysql> select 列名 as 别名 from 表名;
```

+ <font style="color:rgb(64, 64, 64);">去重查询</font>

```csharp
mysql> select distinct 列名 from 表名;
```

+ <font style="color:rgb(64, 64, 64);">按条件查询</font>

```csharp
mysql> select * from 表名 where 列名=值;
```

```csharp
mysql> select * from 表名 where 列名=值 and 列名=值;
```

```csharp
mysql> select * from 表名 where 列名 is null;
mysql> select * from 表名 where 列名 is not null;
```

```plain
>、<、>=、<=、=、<>、between and
```



+ <font style="color:rgb(64, 64, 64);">查询时合并</font>

```csharp
mysql> select 列名+列名 from 表名;
mysql> select 列名+列名 as 别名 from 表名;
```

  


+ <font style="color:rgb(64, 64, 64);">模糊查询</font>

```csharp
mysql> select * from 表名 where 列名 like '%模糊字段%';
mysql> select * from 表名 where 列名 like '模糊字段_';
```

<font style="color:rgb(64, 64, 64);"></font>

### <font style="color:rgb(64, 64, 64);">通过聚合函数查询</font>
+ <font style="color:rgb(64, 64, 64);">查询表中数据条数</font>

```csharp
mysql> select count(*) from 表名;
```

+ <font style="color:rgb(64, 64, 64);">如果列没有数据不计算</font>

```csharp
mysql> select count(列名) from 表名;
```

+ <font style="color:rgb(64, 64, 64);">求平均值</font>

```csharp
mysql> select avg(列名) from 表名;
```

+ <font style="color:rgb(64, 64, 64);">求最极值</font>

```csharp
mysql> select max(列名) from 表名;
mysql> select min(列名) from 表名;
```

+ <font style="color:rgb(64, 64, 64);">求和</font>

```csharp
mysql> select sum(列名) from 表名;
```

+ <font style="color:rgb(64, 64, 64);">排序</font>

```csharp
mysql> select * from 表名 order by 列名;
mysql> select * from 表名 where 列名=值 order by 列名;
```

+ <font style="color:rgb(64, 64, 64);">指定排序格式</font>

```csharp
mysql> select * from 表名 order by 列名 asc;
mysql> select * from 表名 order by 列名 desc;
```

+ <font style="color:rgb(64, 64, 64);">指定多个排序标准</font>

```csharp
mysql> select * from 表名 order by 列名 asc,列名 desc;
```

+ <font style="color:rgb(64, 64, 64);">分组查询</font>

```csharp
mysql> select 列名,count(*) from 表名 group by 列名;
```

+ <font style="color:rgb(64, 64, 64);">分页查询</font>

```csharp
mysql> select * from 表名 limit 开始位置,数据条数;
```



### 数据库表关联查询
+ inner join
+ left join
+ right join

join.. on或者where来实现



### 子查询
使用一个查询的结果作为另一个查询的条件

子查询语句可用于select或者where子句，也可用于数据或者新建表



## 数据库常用内置函数
### 日期函数
<font style="color:rgb(51, 51, 51);">CURDATE(),CURRENT_DATE()</font>

### <font style="color:rgb(51, 51, 51);">字符串函数</font>
| <font style="color:rgb(51, 51, 51);">CHAR_LENGTH(s)</font> | <font style="color:rgb(51, 51, 51);">返回字符串s的字符数</font> |
| :--- | :--- |
| <font style="color:rgb(51, 51, 51);">LENGTH(s)</font> | <font style="color:rgb(51, 51, 51);">返回字符串s的长度</font> |
| <font style="color:rgb(51, 51, 51);">CONCAT(s1,s2,...)</font> | <font style="color:rgb(51, 51, 51);">将字符串s1,s2等多个字符串合并为一个字符串</font> |


### **<font style="color:rgb(0, 0, 0);">数学计算相关函数</font>**
| <font style="color:rgb(51, 51, 51);">ABS(x)</font> | <font style="color:rgb(51, 51, 51);">返回x的绝对值</font> |
| :--- | :--- |
| <font style="color:rgb(51, 51, 51);">CEIL(x),CEILING(x)</font> | <font style="color:rgb(51, 51, 51);">向上取整</font> |
| <font style="color:rgb(51, 51, 51);">FLOOR(x)</font> | <font style="color:rgb(51, 51, 51);">向下取整</font> |


### **<font style="color:rgb(0, 0, 0);">聚合函数</font>**
| <font style="color:rgb(51, 51, 51);">AVG([DISTINCT] expr)</font> | <font style="color:rgb(51, 51, 51);">返回expr的平均值，distinct选项用于忽略重复值</font> |
| :--- | :--- |
| <font style="color:rgb(51, 51, 51);">COUNT([DISTINCT] expr)</font> | <font style="color:rgb(51, 51, 51);">返回select中expr的非0值个数，返回值为bigint类型</font> |
| <font style="color:rgb(51, 51, 51);">GROUP_CONCAT</font> | <font style="color:rgb(51, 51, 51);">连接组内的非空值，若无非空值，则返回NULL</font> |




## MySQL事务
<font style="color:rgb(51, 51, 51);">在 MySQL 中，事务是一组SQL语句的执行，它们被视为一个单独的工作单元。</font>

<font style="color:rgb(51, 51, 51);">一般来说，事务是必须满足4个条件（ACID）：：原子性（</font>**<font style="color:rgb(51, 51, 51);">A</font>**<font style="color:rgb(51, 51, 51);">tomicity，或称不可分割性）、一致性（</font>**<font style="color:rgb(51, 51, 51);">C</font>**<font style="color:rgb(51, 51, 51);">onsistency）、隔离性（</font>**<font style="color:rgb(51, 51, 51);">I</font>**<font style="color:rgb(51, 51, 51);">solation，又称独立性）、持久性（</font>**<font style="color:rgb(51, 51, 51);">D</font>**<font style="color:rgb(51, 51, 51);">urability）。</font>

### <font style="color:rgb(51, 51, 51);">开启事务</font>
1. <font style="color:rgb(51, 51, 51);">用 BEGIN, ROLLBACK, COMMIT 来实现</font>
+ **<font style="color:rgb(51, 51, 51);">BEGIN 或 START TRANSACTION</font>**<font style="color:rgb(51, 51, 51);">：开用于开始一个事务。</font>
+ **<font style="color:rgb(51, 51, 51);">ROLLBACK</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">事务回滚，取消之前的更改。</font>
+ **<font style="color:rgb(51, 51, 51);">COMMIT</font>**<font style="color:rgb(51, 51, 51);">：事务确认，提交事务，使更改永久生效。</font>

<font style="color:rgb(51, 51, 51);"></font>

2. <font style="color:rgb(51, 51, 51);">直接用 SET 来改变 MySQL 的自动提交模式:</font>
+ **<font style="color:rgb(51, 51, 51);">SET AUTOCOMMIT=0</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">禁止自动提交</font>
+ **<font style="color:rgb(51, 51, 51);">SET AUTOCOMMIT=1</font>**<font style="color:rgb(51, 51, 51);"> 开启自动提交</font>

## MySQL索引
<font style="color:rgb(51, 51, 51);">MySQL 索引是一种数据结构，用于加快数据库查询的速度和性能。</font>

<font style="color:rgb(51, 51, 51);">MySQL 索引的建立对于 MySQL 的高效运行是很重要的，索引可以大大提高 MySQL 的检索速度。</font>

### 创建索引
```java
CREATE INDEX index_name
ON table_name (column1, column2, ...);

ALTER TABLE table_name
ADD INDEX index_name (column1 , column2 ...);
```



### 删除索引
```java
DROP INDEX index_name ON table_name;

ALTER TABLE table_name
DROP INDEX index_name;
```

<font style="color:rgb(51, 51, 51);"></font>



## JDBC操作数据库
Java Database Connectivity（JDBC）是一种用于在Java程序和数据库之间进行连接的API。通过JDBC，Java应用程序可以执行SQL查询、更新和处理数据库。

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DatabaseConnection {
    private static final String URL = "jdbc:your_database_url";
    private static final String USER = "your_username";
    private static final String PASSWORD = "your_password";

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASSWORD);
    }
}
```



---



<font style="color:rgb(24, 25, 28);">大家学习Java碰到问题欢迎加入知识星球「风筑的编程圈子」，答疑所有碰到的编程问题，面试问题以及职业发展问题，欢迎加入</font>

<font style="color:rgb(24, 25, 28);"></font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/26411187/1719459784632-b665952c-7260-492c-af91-faa772aa4c88.jpeg)

<font style="color:rgb(24, 25, 28);"></font>

