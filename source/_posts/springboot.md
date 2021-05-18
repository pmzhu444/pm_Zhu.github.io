---
title: SpringBoot
date: 2021-04-05 16:11:50
tags: [踩坑, 笔记]
---

# Springboot 使用过程中遇到的问题记录

<!--more-->

1. 不连数据库的话，pom里有数据库方面的依赖会报错

   - 解决方案：

     - 删除依赖    或者

     - properties里配置好数据库：      

       ```
       spring.datasource.url=jdbc:[postgresql://192.168.74.129:6543/postgres](postgresql://192.168.74.129:6543/postgres)      spring.datasource.username=sa      spring.datasource.password=tusc@6789#JKL      spring.datasource.driverClassName=org.postgresql.Driver      spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect      spring.jpa.properties.hibernate.hbm2ddl.auto=update      spring.jpa.properties.hibernate.temp.use_jdbc_metadata_defaults = false
       ```

       *（后面三行待看hibernate）*

2. 运行时空指针的问题注意：

   - @Autoware标签是否注入进去
   - Application里@SpringBootApplication标签后面是否加了不该加的东西

3. 注入时Could not autowire，但是不影响正常运行

   - 不解决也行。。。

   - 解决办法：

     - 降低Autowired检测的级别，将Severity的级别由之前的error改成warning或其它可以忽略的级别

       **File->Settings->Editor->Code Style->Inspections中Spring->Spring Core->Code->Autowiring for Bean Class  error改成warning **

     - 注意导入正确的包

       最容易导入的错误包：*importcom.alibaba.dubbo.config.annotation.Service;*      正确的包应该是*importorg.springframework.stereotype.Service;*

4. 报错：无法注入！
   - 启动类忘了扫描mapper吧

5. 数据库(postgresql)时间戳时间比较：（数据库中为时间戳timestamp类型）
   - 字符串转为date类型：to_date(#{ arg2 }, 'yyyy-mm-dd hh24:mi:ss')可比较>(&gt;)、<(&lt;)、>=(&gt;=)、<=(&lt;=) 不能比较相等 =  
   - 字符串转为timestamp类型可比较等于=：to_timestamp(#{ arg1 }, 'yyyy-mm-dd hh24:mi:ss')

6. mybatis的xml：  
   - resultmap：
     - column 数据库中取出来的列 property 实体类的属性
     - 如果实体类为pojo类的话 上面两个的名字可以不一样，普通实体类必须一样。**大概是因为pojo类有数据库的映射，实体类不一样的话找不到吧**        
     - sql传进一个参数的话 select标签加parameterType="java.lang.String" 类似 多个参数不加，取参时使用 #{ arg0 }···  
   - properties配置文件加：mybatis.mapper-locations=classpath:xml/*Mapper.xml

7. 传参

   - 网页间传参 ：@Param("cid") String cid     @RequestMapping("/demo1")  

   - ajax传参：@PathVariable String info       @RequestMapping("/chaxun/{info}")

8. maven打包遇到问题： jar中没有主清单属性 

   - pom中增加依赖：

   ```xml
   <plugins>
       <plugin>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-maven-plugin</artifactId>
           <executions>
               <execution>
                   <goals>
                       <goal>repackage</goal>
                   </goals>
               </execution>
           </executions>
       </plugin>
   </plugins>
   ```

9. 1099端口占用：

   -  artery版本过高，6.0.35后集成了tas，默认端口1099，需修改默认端口。(不重要)

   - maven排除依赖：

     - **maven是层级依赖的，当想要引用此依赖却不想用它引用的依赖时（当一个依赖引用的依赖出问题时），就排除出问题的依赖：**

     - 找到是哪个依赖引用了它，然后在它的dependency下加入

       ```
       <exclusions>
       	<exclusion>
       		<groupId></groupId>
       		<artifactId></artifactId>
       	</exclusion>
       </exclusions>
       ```

     - exclusions标签解释

       - 这个标签的作用是排除关联依赖的引入，因为maven的pom依赖其中有一点是将关联的依赖全都引入进来
       - 这个标签在这的作用就是 如果关联的依赖和引入的其他依赖可能存在冲突， 就必须将关联的依赖排除掉，所以就用这个标签。 
       - 另外这个标签附带s，大家应该也明白 ，就是可以包含多个！！！！

     -  例如：

       ```
       <dependency>
       	<groupId>com.thunisoft.artery</groupId>
       	<artifactId>artery-starter</artifactId>
       	<exclusions>
       		<exclusion>
       			<groupId>com.thunisoft.tas</groupId>
       			<artifactId>tas-starter</artifactId>
       		</exclusion>
       	</exclusions>
       </dependency>    
       ```

     - 注：这个排除tas不好用，因为artery6.0.35后干掉了tomcat，不引用tas还是会报错。(这个也不重要)

10. jar解压打包
    - 解压：jar -xvf *.jar
    - 压缩：jar -cvf \*.jar ./\*
    -  注：jdk版本必须与原来压缩时的版本一致

11. @PostConstruct 注解作用
    - 在Springboot项目启动的时候所执行的方法。在Bean加载完但用户线程进来之间执行的方法。