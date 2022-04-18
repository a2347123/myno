# 一、MyBatis-Plus简介

## 1、简介

**MyBatis-Plus**(简称 MP)是一个 **MyBatis的增强工具**，在 MyBatis 的基础上**只做增强不做改变**，为 **简化开发、提高效率而生。**

>愿景
>
>我们的愿景是成为 MyBatis 最好的搭档，就像魂斗罗中的 1P、2P，基友搭配，效率翻倍。

![image](MyBatisPlus(SpringBoot版)-尚硅谷.assets/image-16502578565771.png)

## 2、特性

- **无侵入**:只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑 
- **损耗小**:启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**:内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
- **支持 Lambda 形式调用**:通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错 支持主键自动生成:支持多达 4 种主键策略(内含分布式唯一 ID 生成器 - Sequence)，可自由 配置，完美解决主键问题
- **支持 ActiveRecord 模式:支持 ActiveRecord 形式调用**，实体类只需继承 Model 类即可进行强 大的 CRUD 操作
- **支持自定义全局通用操作**:支持全局通用方法注入( Write once, use anywhere ) 内置代码生成器:采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**:基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等 同于普通 List 查询
- **分页插件支持多种数据库**:支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、 Postgre、SQLServer 等多种数据库
- **内置性能分析插件**:可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出 慢查询
- **内置全局拦截插件**:提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防 误操作

## 3、支持数据库

> 任何能使用MyBatis进行 CRUD, 并且支持标准 SQL 的数据库，具体支持情况如下

- MySQL，Oracle，DB2，H2，HSQL，SQLite，PostgreSQL，SQLServer，Phoenix，Gauss ， ClickHouse，Sybase，OceanBase，Firebird，Cubrid，Goldilocks，csiidb 
- 达梦数据库，虚谷数据库，人大金仓数据库，南大通用(华库)数据库，南大通用数据库，神通数据 库，瀚高数据库

## 4、 框架结构

![2](MyBatisPlus(SpringBoot版)-尚硅谷.assets/2.png)

## 5、代码文档及文档地址

官方地址: http://mp.baomidou.com

代码发布地址:

Github: https://github.com/baomidou/mybatis-plus 

Gitee: https://gitee.com/baomidou/mybatis-plus 

文档发布地址: https://baomidou.com/pages/24112f

# 二、入门案例

## 1、开发环境

IDE:idea：2021.3

JDK:JDK8+

构建工具:maven 3.8.4 

MySQL版本:MySQL 8.0.27

Spring Boot:2.6.4 

MyBatis-Plus:3.5.1

系统：mac m1

## 2、创建数据库及表

### a>创建表

```sql
CREATE DATABASE `mybatis_plus` /*!40100 DEFAULT CHARACTER SET utf8mb4 */;
use `mybatis_plus`;
CREATE TABLE `user` (
`id` bigint(20) NOT NULL COMMENT '主键ID', `name` varchar(30) DEFAULT NULL COMMENT '姓名', `age` int(11) DEFAULT NULL COMMENT '年龄', `email` varchar(50) DEFAULT NULL COMMENT '邮箱', PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### b>添加数据

```sql
INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

## 3、创建SpringBoot工程

### a>初始化工程

![3](MyBatisPlus(SpringBoot版)-尚硅谷.assets/3.png)



![4](MyBatisPlus(SpringBoot版)-尚硅谷.assets/4-16502583751101.png)



### b>引入依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <!--MyBatis-plus启动器-->
    <dependency>
        <groupId>com.baomidou</groupId>
        <artifactId>mybatis-plus-boot-starter</artifactId>
        <version>3.5.1</version>
    </dependency>

    <!--lombok用于简化实体类开发-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>

    <!--mysql驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
</dependencies>
```

### c>idea中安装lombok插件

![5](MyBatisPlus(SpringBoot版)-尚硅谷.assets/5.png)

**一般现在IDEA都内置有这个插件**