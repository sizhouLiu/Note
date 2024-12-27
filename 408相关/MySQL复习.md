# MySQL复习

关系型数据库：

建立在关系模型基础上 由多张相互连接的表组成的数据库

### SOL通用语法

1. SOL语句可以单行或多行书写，以分号结尾，
2. SOL语句可以使用空格/缩进来增强语句的可读性。
3. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写

注释:
单行注释:--注释内容 或 #注释内容(MySOL特有)
多行注释:/*注释内容 */

| 分类 | 全称                       | 说明                                                 |
| ---- | -------------------------- | ---------------------------------------------------- |
| DDL  | Data Definition Language   | 数据库定义语言 用来定于数据库对象（数据库 表 字段）  |
| DML  | Data Manipulation Language | 数据操作语言，用来对数据表的数据进行增删改           |
| DQL  | Data Query Language        | 数据库查询语言 用来查询数据库中表的记录              |
| DCL  | Data Control Language      | 数据控制语言 用来创建数据库用户 控制数据库的访问权限 |

------

## DDL：



查询所有数据库

```sql
SHOW DATABASES ;
```

查询当前数据库

```sql
SELECT DATABASE();
```

创建

```mysql
CREATE DATABASE [IF NOT EXISTS]数据库名[DEFAULT CHARSET字符集][COLLATE 排序规则];
```

删除

```Mysql
DROP DATABASE[IF EXISTS]数据库名,
```

使用

```mysql
USE 数据库名
```

### DDL-表操作-查询

查询当前数据库所有表

```mysql
SHOW TABLES;
```

查询表结构

```mysql
DESC 表名;
```

查询指定表的建表语句

```mysql
SHOW CREATE TABLE 表名)
```

```mysql
CREATE TABLE 表名(
字段1字段1类型[COMMENT 字段1注释],
字段2 字段2类型[COMMENT 字段2注释]
字段3 字段3类型[COMMENT 字段3注释]
字段n 字段n类型 [COMMENT 字段n注释])[COMMENT 表注释];
```

### 数据类型

MySQL中的数据类型有很多，主要分为三类:数值类型、字符串类型、日期时间类型。

#### 数值类型

| 分类 | 类型         | 大小    | 有符号(SIGNED)范围                                 | 无符号                                                | 描述               |
| ---- | ------------ | ------- | -------------------------------------------------- | ----------------------------------------------------- | ------------------ |
|      | TINYINT      | 1 byte  | (-128，127)                                        | (0，255)                                              | 小整数值           |
|      | SMALLINT     | 2 bytes | (-32768，32767)                                    | (0，65535)                                            | 大整数值           |
|      | MEDIUMINT    | 3 bytes | (-8388608，8388607)                                | (0,16777215)                                          | 大整数值           |
|      | INT或INTEGER | 4 bytes | (-2147483648,2147483647)                           | (0，4294967295)                                       | 大整数值<          |
|      | BIGINT       | 8 bytes | (-2^63，2^63-1)                                    | (0，2~64-1)                                           | 极大整数值         |
|      | FLOAT        | 4 bytes | (-3.402823466 +38,3.402823466351 E+38)             | 0和(1.175494351 E-38，3.402823466 +38)                | 单精度浮点数值     |
|      | DOUBLE       | 8 bytes | (-1.7976931348623157 +308,1.7976931348623157 +308) | 0和(2.2250738585072014 -308，1.7976931348623157 +308) | 双精度浮点数值     |
|      | ECIMAL       |         | 依赖于M(精度)和D(标度)的值                         | 依赖于M(精度)和D(标度)的值                            | 小数值(精确定点数) |

#### 字符串类型

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

#### 日期和时间类型

| 类型      | 大小 ( bytes) | 范围                                                         | 格式                | 用途                     |
| :-------- | :------------ | :----------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8             | '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'               | YYYY-MM-DD hh:mm:ss | 混合日期和时间值         |
| TIMESTAMP | 4             | '1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYY-MM-DD hh:mm:ss | 混合日期和时间值，时间戳 |

### 修改

添加字段

```mysql
ALTER TABLE 表名 ADD 字段名 类型 (长度)[COMMENT 注释][约束]
```

修改数据类型

```mysql
ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);
```

修改字段名和字段类型

```mysql
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度)[COMMENT 注释][约束];
```

删除字段

```mysql
ALTER TABLE 表名 DROP 字段名
```

修改表名

```mysql
ALTER TABLE 表名 RENAME TO 新表名
```

删除表

```mysql
DROP TABLE[IF EXISTS]表名,
```

## DML

DML英文全称是Data Manipulation Language(数据操作语言)，用来对数据库中表的数据记录进行增删改操作。

- 添加数据(INSERT)
- 修改数据(UPDATE
- 删除数据(DELETE)

### 添加数据INSERT 



1.给指定字段添加数据

```mysql
INSERT INTO 表名 (字段名1,字段名2,..) VALUES (值1,值2, ..);
```

2.给全部字段添加数据

```mysql
INSERT INTO 表名 VALUES (值1,值2,..);
```

3.批量添加数据

```mysql
INSERT INTO 表名 (字段名1,字段名2,..) VALUES (值1,值2,..),(值1,值2,.),(值1,值2,.);INSERT INTO 表名 VALUES (值1,值2,….),(值1,值2,…),(值1,值2,….);
```

### 修改数据(UPDATE）

```mysql
UPDATE 表名 SET 字段名1=值1,字段名2=值2,…[WHERE 条件];
```

### DML-删除数据

```MySQL
DELETE FROM 表名[WHERE 条件]
```

## DQL

DQL-语法

```
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

- 基本查询
- 条件查询(WHERE)
- 聚合函数(count、max、min、avg、sum)
- 分组查询(GROUP BY)
- 排序查询(ORDER BY)
- 分页查询(LIMIT)

DOL-基本查询
查询多个字段1.

```mysql
SELECT 字段1,字段2,字段3.. FROM 表名;
SELECT*FROM 表名
```

设置别名

```mysql
SELECT 字段1[AS 别名1],字段2[AS 别名2]…. FROM 表名;
```

去除重复记录

```mysql
SELECT DISTINCT 字段列表 FROM 表名:
```

### DOL-条件查询

1. 语法

   ```mysql
   SELECT 字段列表 FROM 表名WHERE条件列表
   ```

   - 查询语句中你可以使用一个或者多个表，表之间使用逗号**,** 分割，并使用WHERE语句来设定查询条件。
   - 你可以在 WHERE 子句中指定任何条件。
   - 你可以使用 AND 或者 OR 指定一个或多个条件。
   - WHERE 子句也可以运用于 SQL 的 DELETE 或者 UPDATE 命令。
   - WHERE 子句类似于程序语言中的 if 条件，根据 MySQL 表中的字段值来读取指定的数据。

| =      | 等号，检测两个值是否相等，如果相等返回true                   | (A = B) 返回false。  |
| ------ | ------------------------------------------------------------ | -------------------- |
| <>, != | 不等于，检测两个值是否相等，如果不相等返回true               | (A != B) 返回 true。 |
| >      | 大于号，检测左边的值是否大于右边的值, 如果左边的值大于右边的值返回true | (A > B) 返回false。  |
| <      | 小于号，检测左边的值是否小于右边的值, 如果左边的值小于右边的值返回true | (A < B) 返回 true。  |
| >=     | 大于等于号，检测左边的值是否大于或等于右边的值, 如果左边的值大于或等于右边的值返回true | (A >= B) 返回false。 |
| <=     | 小于等于号，检测左边的值是否小于或等于右边的值, 如果左边的值小于或等于右边的值返回true | (A <= B) 返回 true。 |

![image-20240321215034352](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20240321215034352.png)

![image-20240321215041059](C:\Users\sz.L\AppData\Roaming\Typora\typora-user-images\image-20240321215041059.png)

### DOL-聚合函数

1. 介绍
   将一列数据作为一个整体，进行纵向计算。
   常见聚合函数

   | 函数  | 功能     |
   | ----- | -------- |
   | count | 统计数量 |
   | max   | 最大值   |
   | min   | 最小值   |
   | avg   | 平均值   |
   | sum   | 求和<    |

分组查询

### DOL-分组查询

```mysql
SELECT 字段列表 FROM 表名[WHERE 条件]GROUP BY 分组字段名[HAVING 分组后过滤条件];
```

where与having区别

- 执行时机不同:where是分组之前进行过滤，不满足where条件，不参与分组;而having是分组之后对结果进行过滤。
- 判断条件不同:where不能对聚合函数进行判断，而having可以。

### DQL-排序查询

语法

```mysql
SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1,字段2 排序方式2;
```

排序方式

- ASC:升序(默认值)
- DESC:降序

### DQL-分页查询

```mysql
SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询记录数
```

## DCL

### DCL-管理用户

1.查询用户

```mysql
USE mysql;
SELECT * FROM user;
```

2.创建用户

```mysql
CREATE USER '用户名'@'主机名’IDENTIFIED BY'密码'
```

3.修改用户密码

```mysql
ALTER USER'用户名'@'主机名’IDENTIFIED WITH mysql native password BY '新密码';
```

4.删除用户

```mysql
DROP USER'用户名'@'主机名’；
```

### DCL-权限控制

MySQL中定义了很多种权限，但是常用的就以下几种:

| 权限               | 说明               |
| ------------------ | ------------------ |
| ALL,ALL PRIVILEGES | 所有权限           |
| SELECT             | 查询数据           |
| INSERT             | 插入数据           |
| UPDATE             | 修改数据           |
| DELETE             | 删除数据           |
| ALTER              | 修改表             |
| DROP               | 删除数据库/表/视图 |
| CREATE             | 创建数据库/表      |

注意：

- 多个权限之间，使用逗号分隔；
- 授权时，数据库名和表名可以使用*进行通配，代表所有。

### **查询权限**

查询权限语法格式如下：

```mysql
SHOW GRANTS FOR '用户名'@'主机名';
```

示例代码如下：

授予权限语法格式如下：

```mysql
GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
```

### **撤销权限**

撤销权限语法格式如下：

```mysql
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```



## 函数：PASS





-----

## 约束（やくそく？）

1. 概念:约束是作用于表中字段上的规则，用于限制存储在表中的数据，
2. 目的:保证数据库中数据的正确、有效性和完整性。

| 约束NOT NULL             | 描述                                                     | 关键字      |
| ------------------------ | -------------------------------------------------------- | ----------- |
| 非空约束                 | 限制该字段的数据不能为null                               | NOT NULL    |
| 唯一约束                 | 保证该字段的所有数据都是唯一、不重复的                   | UNIQUE      |
| 主键约束                 | 主键是一行数据的唯一标识，要求非空且唯一                 | PRIMARY KEY |
| 默认约束                 | 保存数据时，如果未指定该字段的值，则采用默认值           | DEFAULT     |
| 检查约束(8.0.16版本之后) | 保证字段值满足某一个条件                                 | CHECK<      |
| 外键约束                 | 用来让两张表的数据之间建立连接，保证数据的一致性和完整性 | FOREIGN KEY |

### 外键约束：

- 外键用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性
- 具有外键的表称之为子
- 表外键所关联的这张表 称之为 父表

添加外键

```mysql
CREATE TABLE 表名(
字段名 数据类型
[CONSTRAINT[外键名称]FOREIGN KEY(外键字段名)REFRENCES主表(主表列名)
};
```

```mysql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY外键字段名)REFERENCES 主表(主表列名);
```

删除外键

```mysql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```

