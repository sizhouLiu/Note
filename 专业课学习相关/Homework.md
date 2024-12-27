# Homework

```
             CREATE TABLE Employee
                     ( 
                     编号 CHAR(8) PRIMARY KEY,
                     姓名 VARCHAR(8) not null, # 少了逗号
                    部门 char(40), # chr为char 中文括号
                    工资 numeric(8,2),
                    生日 datetime,
                    职称 char(20)
                    )
```



```
INSERT INTO Student(Sno,Sname,Ssex,Sage,Sdept,sclass) VALUES('20100010',赵斌,'男','19','IS','1005')   赵斌没加引号
INSERT INTO Student VALUES('20100022','张明明',19,'男','CS','1002')    性别和年龄位置反了
```

```mysql
 UPDATE sc SET Grade=80
	 WHERE Sno="20100002"

```

```
UPDATE Student SET Sno="20100025"
    ->  WHERE Sno="20100021";
```

```
 UPDATE sc SET Grade=80
    ->  WHERE Sno="20100002";
```

