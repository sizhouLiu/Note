# MySQL常用命令

## **一、登录**

### 1、mysql服务的启动和停止

```
mysql> net stop mysql``mysql> net start mysql
```

### 2、登陆mysql

```
mysql> 语法如下： mysql ``-``u用户名 ``-``p用户密码
```

注意，如果是连接到另外的机器上，则需要加入一个参数-h机器IP

 

## **二、常用命令**

### 1.创建数据库：

```
1 mysql> create database test ;                         // 在mysql里面创建数据库，数据库的ID是test。                 
2 
3 [root@host] # mysqladmin -u root -p create test ;     // 在linux下创建数据库，数据库的ID是test。    
```

###  2.删除数据库：

```
mysql> drop  database test ;    // 在mysql里面删除数据库，数据库的ID是test。

[root@host] # mysqladmin -u root -p drop  test ;    //在linux下删除数据库，数据库的ID是test。
```

###  3.创建数据表：

```
root@host# mysql -u root -p               
Enter password:*******
mysql> use RUNOOB;                                 //选择数据库
Database changed
mysql> CREATE TABLE runoob_tbl(        //数据表的名字是runoob_tbl
   -> runoob_id INT NOT NULL AUTO_INCREMENT,
   -> runoob_title VARCHAR(100) NOT NULL,
   -> runoob_author VARCHAR(40) NOT NULL,
   -> submission_date DATE,
   -> PRIMARY KEY ( runoob_id )
   -> )ENGINE=InnoDB DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.16 sec)
```

###  4.删除数据表：

```
root@host# mysql -u root -p
Enter password:*******
mysql> use RUNOOB;
Database changed
mysql> DROP TABLE runoob_tbl                  //删除数据表 runoob_tbl
Query OK, 0 rows affected (0.8 sec)
```

### 5.select查询：

```
select * from runoob_tbl ;            //查询表 runoob_tbl

select ip,username from runoob_tbl ; //查询runoob_tbl表中ip和username字段

select ip from runoob_tbl  where username="woniu" ; //在runoob_tbl表中查询字段username=“woniu”的ip

select ip from runoob_tbl  where username="woniu" order by ip desc ; //在runoob_tbl表中查询字段username=“woniu”的ip，按照ip倒序
```

### 6.常用命令

```
show databases;  //显示所有的库

  use  test;             //选择数据库，库的ID是test

  show  tables；       //显示库中所有的表，要先use 选中数据库

  show create table  runoob_tbl；       //查看表结构

  show  index  from             //显示数据表的详细索引信息，包括PRIMARY KEY（主键）。

  quit/exit                     //退出mysql
```

### 7.模糊查询--like

```
mysql> select  * from runoob_tbl  where  username like '%wugui';               //在 runoob_tbl 表中获取 runoob_author 字段中以 wugui为结尾的的所有记录

'%a'     //以a结尾的数据
'a%'     //以a开头的数据
'%a%'    //含有a的数据

查询以 mysql字段开头的信息。

SELECT * FROM runoob_tbl  WHERE name LIKE 'mysql%';

查询包含 mysql字段的信息。

SELECT * FROM runoob_tbl  WHERE name LIKE '%mysql%';

查询以 mysql字段结尾的信息。

SELECT * FROM runoob_tbl  WHERE name LIKE '%mysql';
```

###  8.msyql插入百万数据

```
insert into warn_message(username,strength,ip,port,clienttype,logtime,country,warn_type) select username,strength,ip,port,clienttype,logtime,country,warn_type from warn_message;
```

###  9.修改数据表的默认编码格式

```
alter table 表名 convert to character set utf8(latin1);
```

 

###  10.清空表数据

```
truncate table 表名
```