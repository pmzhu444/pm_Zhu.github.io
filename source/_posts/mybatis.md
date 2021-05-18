---
title: mybatis
date: 2021-05-05 16:11:50
tags: [踩坑,笔记]
---

# Mybatis使用流程及遇到的问题记录

postgresql为例

详情：移动硬盘 华宇/mybatis 文件夹

<!--more-->

## 使用流程

1. 首先数据库要有张表。。
2. 修改mybatis-generator-core-1.3.7/lib/generatorConfig.xml
3. mybatis-generator-core-1.3.7/lib文件夹下有mybatis-generator-core-1.3.7-sources.jar、postgresql-42.2.5.jar、mybatis-3.2.7.jar、generatorConfig.xml四个文件、一个src文件夹（我有5个jar包，其余两个没有行不行还没试。。）
4. 进入cmd
5. 输入java -jar mybatis-generator-core-1.3.7.jar -configfile generatorConfig.xml -overwrite
6. 代码自动生成完成。。（为啥只有insert方法：见注8）
7. 将生成的代码copy到对应的文件夹下（文件夹名为对应路径）

## 踩过的坑

1. copy到项目中注意路径的问题（或者直接生成到项目中，总之都要注意路径）

2. controller、service需要自己写

3. 将DemoApplication.java修改一下，让其扫描dao层接口。  @MapperScan("com.example.mybatistest.mapper")

4. 新建项目时至少选web、jpa、postgresql（依要用的数据库定）、mybatis

5. mapper.xml要放在resources下面的xml下！！要不然找不到

6. properties配置文件：

   ```
   spring.profiles.active=dev
   mybatis.type-aliases-package=com.example.mybatistest.entity
   mybatis.mapper-locations=classpath:xml/*Mapper.xml
   
   spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
   spring.profiles.include=dev
   spring.datasource.url=jdbc:postgresql://192.168.74.128:6543/postgres
   spring.datasource.username=sa
   spring.datasource.password=tusc@6789#JKL
   spring.datasource.driverClassName=org.postgresql.Driver
   spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
   spring.jpa.properties.hibernate.hbm2ddl.auto=update
   spring.jpa.properties.hibernate.temp.use_jdbc_metadata_defaults = false
   ```

7. maven依赖：

   ```
   <dependencies>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-data-jpa</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.mybatis.spring.boot</groupId>
           <artifactId>mybatis-spring-boot-starter</artifactId>
           <version>2.0.1</version>
       </dependency>
       <dependency>
           <groupId>org.postgresql</groupId>
           <artifactId>postgresql</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid</artifactId>
           <version>1.1.0</version>
       </dependency>
   </dependencies>
   ```

8. 注意数据库表有没有主键

   ```
   <table tableName="client_operate_platform_demo_tb" domainObjectName="PlatformDemoOperateLogInfo" enableCountByExample="false" enableUpdateByExample="false"enableDeleteByExample="false" enableSelectByExample="false"enableSelectByPrimaryKey="true" enableUpdateByPrimaryKey="false"enableDeleteByPrimaryKey="false" ></table>
   ```

   