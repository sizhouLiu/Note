# SpringBoot 光速入门

-----

## 前言 

最近实习需要用Java写一个web项目 为了实习混分我拉了一个springboot的项目为了答辩的时候能够讲清楚 我把这个东西速成 应付答辩

## Springboot是什么

1.SpringBoot是一个脚手架基于Spring的web

2.用于快速搭建一个应用，开箱即用!创建即可开发业务代码

3.其设计目的是用来简化 Spring 应用的初始搭建以及开发过程

## 优点

- 快速构建一个独立的 Spring 应用程序
- 嵌入的 Tomcat、Jetty 或者 Undertow，无须部署 WAR文件;
- 提供starter POMs来简化Maven配置和减少版本冲突所带来的问题
- 对Spring和第三方库提供默认配置(约定大于配置)可修改默认值，简化框架配置;
- -无需配置XML--JavaConfig，无代码生成，开箱即用



![image-20241210194116427](https://stupid-blog-img.oss-cn-beijing.aliyuncs.com/Blog/image-20241210194116427.png)

配置

```spring
spring.application.name=SpringBoot_Sttudy
spring.jpa.hibernate.ddl-auto=update

spring.datasource.url=jdbc:mysql://localhost:3306/springboot?serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=vs8824523
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#spring.jpa.show-sql= true
spring.jpa.properties.hibernateformat_sql=true
server.port=8088

```

Rest
而在HTTP协议中，有不同的发送请求的方式，分别是GET、POST、PUT、DELETE等，我们如果能让不同的请求方式表示不同的请求类型就可以简化我们的查询,改成名词:
查询用户: http://localhost:8080/user/1 GET --查询

查询多个用户: http://localhost:8080/users GET

新增用户: http://localhost:8080/user POST ---新增

修改用户: http://localhost:8080/user PUT --修改

删除用户:http://localhost:8080/user/1
