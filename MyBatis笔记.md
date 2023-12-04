## 一、MyBtis 概述

### 1.1 框架

​	●在⽂献中看到的framework被翻译为框架

​	● Java常⽤框架：

> ○ SSM三⼤框架：Spring + SpringMVC + MyBatis
>
> ○ SpringBoot
>
> ○ SpringCloud
>
> ○ 等。。

​	● 框架其实就是对通⽤代码的封装，提前写好了⼀堆接⼝和类，我们可以在做项⽬的时候直接引⼊这些接⼝和类（引⼊框架），基于这些现有的接⼝和类进⾏开发，可以⼤⼤提⾼开发效率。

​	● 框架⼀般都以jar包的形式存在。(jar包中有class⽂件以及各种配置⽂件等。)

​	● SSM三⼤框架的学习顺序：

> ○ ⽅式⼀：MyBatis、Spring、SpringMVC（建议）
>
> ○ ⽅式⼆：Spring、MyBatis、SpringMVC

### 1.2 三层架构

![image-20230816091602933](图片\image-20230816091602933.png)

● 表现层（UI）：直接跟前端打交互（⼀是接收前端ajax请求，⼆是返回json数据给前端）

● 业务逻辑层（BLL）：⼀是处理表现层转发过来的前端请求（也就是具体业务），⼆是将从持久层获取的数据返回到表现层。

● 数据访问层（DAL）：直接操作数据库完成CRUD，并将获得的数据返回到上⼀层（也就是业务逻辑层）。

● Java持久层框架：

> ○ MyBatis
>
> ○ Hibernate（实现了JPA规范）
>
> ○ jOOQ
>
> ○ Guzz
>
> ○ Spring Data（实现了JPA规范）
>
> ○ ActiveJDBC
>
> ○ ......

### 1.3 JDBC不足

● 示例代码1：

![image-20230816092631504](.\图片\image-20230816092631504.png)



● JDBC不⾜：

> ○ SQL语句写死在Java程序中，不灵活。改SQL的话就要改Java代码。违背开闭原则OCP
>
> ○ 给?传值是繁琐的。能不能⾃动化？？？
>
> ○ 将结果集封装成Java对象是繁琐的。能不能⾃动化？？？

### 1.4 了解MyBatis

● MyBatis本质上就是对JDBC的封装，通过MyBatis完成CRUD。

● MyBatis在三层架构中负责持久层的，属于持久层框架。

● MyBatis的发展历程：【引⽤百度百科】

> ○ MyBatis本是apache的⼀个开源项⽬iBatis，2010年这个项⽬由apache software foundation迁移到了google code，并且改名为	MyBatis。2013年11⽉迁移到Github。
>
> ○ iBATIS⼀词来源于“internet”和“abatis”的组合，是⼀个基于Java的持久层框架。iBATIS提供的持久层框架包括SQL Maps和Data 	Access Objects（DAOs）。

● 打开mybatis代码可以看到它的包结构中包含：ibatis

![image-20230816093324604](.\图片\image-20230816093324604.png)

● ORM：对象关系映射

> ○ O（Object）：Java虚拟机中的Java对象
>
> ○ R（Relational）：关系型数据库
>
> ○ M（Mapping）：将Java虚拟机中的Java对象映射到数据库表中⼀⾏记录，或是将数据库表中
>
> ⼀⾏记录映射成Java虚拟机中的⼀个Java对象。
>
> ○ ORM图示
>
> ![001-ORM思想-对象关系映射](.\图片\001-ORM思想-对象关系映射.png)
>
> ![image-20230816093401876](.\图片\image-20230816093401876.png)
>
> ![image-20230816093416916](.\图片\image-20230816093416916.png)
>
> ○ MyBatis属于半⾃动化ORM框架。
>
> ○ Hibernate属于全⾃动化的ORM框架。

● MyBatis框架特点：

> ○ ⽀持定制化 SQL、存储过程、基本映射以及⾼级映射
>
> ○ 避免了⼏乎所有的 JDBC 代码中⼿动设置参数以及获取结果集
>
> ○ ⽀持XML开发，也⽀持注解式开发。【为了保证sql语句的灵活，所以mybatis⼤部分是采⽤
>
> XML⽅式开发。】
>
> ○ 将接⼝和 Java 的 POJOs(Plain Ordinary Java Object，简单普通的Java对象)映射成数据库中的
>
> 记录
>
> ○ 体积⼩好学：两个jar包，两个XML配置⽂件。
>
> ○ 完全做到sql解耦合。
>
> ○ 提供了基本映射标签。
>
> ○ 提供了⾼级映射标签。
>
> ○ 提供了XML标签，⽀持动态SQL的编写。
>
> ○ ......

## 二、MyBatis 入门程序

### 2.1 MyBatis入门程序开发步骤

MyBatis中有两个主要的配置文件：

> 第一个：mybatis-config.xml，这是核心配置文件，主要配置连接数据库的信息等。（通常只有一个文件）
>
> 第二个：XxxMapper.xml，这个文件是专门用来编写SQL语句的配置文件。（通常一个表对应一个文件）

1.在maven项目中的 resources 文件夹下创建一个 mybatis-config.xml 的文件

> （1）这个文件名不是必须叫做 mybatis-config.xml ，可以用其他的名字。
>
> （2）这个文件存放的位置不是固定的，可以随机，但一般情况下，会被放在类的根路径下
>
> （3）放在 resources 文件夹下，等同于放到了类的根路径下。

2.加入依赖：

> mysql依赖
>
> MyBatis依赖

3.编写MyBatis核心配置文件：mybatis-config.xml 

> ~~~xml
> <?xml version="1.0" encoding="UTF-8" ?>
> <!DOCTYPE configuration
>         PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
>         "https://mybatis.org/dtd/mybatis-3-config.dtd">
> <configuration>
>     <environments default="development">
>         <environment id="development">
>             <transactionManager type="JDBC"/>
>             <dataSource type="POOLED">
>                 <property name="driver" value="com.mysql.cj.jdbc.Driver"/>   <!--mysql驱动-->
>                 <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/><!--数据库连接-->
>                 <property name="username" value="root"/>			<!--用户名-->
>                 <property name="password" value="051727"/>			<!--密码-->
>             </dataSource>
>         </environment>
>     </environments>
>     <mappers>
>         <!--指定Xxxmapper.xml文件路径-->
>         <!--resource 属性自动会从类的根路径下开始查找文件-->
>         <mapper resource="org/mybatis/example/BlogMapper.xml"/>
>     </mappers>
> </configuration>
> ~~~

4.在 resources 目录下新建 XxxMapper.xml ，并编写

> ~~~xml
> <?xml version="1.0" encoding="UTF-8" ?>
> <!DOCTYPE mapper
>         PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
>         "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
> <mapper namespace="car"> <!--namespace 翻译为：命名空间，可以随意写-->
> <!--id是这条sql语句的唯一标识，这个id就代表了这个sql语句-->
> <!--select 语句-->
>     <select id="">
>         select * from t_car where id>1
>     </select>
>     <insert id=""></insert>
>     <update id=""></update>
>     <delete id=""></delete>
> </mapper>
> ~~~
>
> 注意：
>
> 1.sql语句最后结尾可以不写 " ; "
>
> 2.这个文件的文件名可以随意。文件位置也可以随意

5.将CarMapper.xml⽂件路径配置到mybatis-config.xml

> ~~~xml
>     <mappers>
>         <!--指定Xxxmapper.xml文件路径-->
>         <mapper resource="CarMapper.xml"/>
>     </mappers>
> ~~~

6. 编写 MyBatis 代码

​		使用MyBatis的类库，连接数据库，做增删改查

​	1.在MyBatis中负责执行sql语句的对象：SqlSession，SqlSession是专门用来执行sql语句的，是一个java虚拟机和数据库之间的会话。

​	2.想要获取SqlSession对象，需要先获取SqlSessionFactory 对象，通过SqlSessionFactory工厂来生产SqlSession对象。

​	3.获取 SqlSessionFactory 对象，需要 SqlSessionFactoryBuilder 对象，通过 SqlSessionFactoryBuilder 对象中的build方法，来获取			SqlSessionFactory 对象。build方法需要传入一个 inputstream 参数，这个 inputStream 就是对应的 mybatis-config.xml 文件

​	4.MyBatis的核心对象：

> ​	（1）SqlSession
>
> > ​	1.SqlSession对象中有insert、select、update、delete等方法，它默认不会自动提交事物，需要我们手动提交。
> >
> > ​	2.insert方法返回一个数字，这个数字就是影响数据库当中的记录条数。插入x条数据则返回x条数据。
>
> ​	（2）SqlSessionFactory
>
> > ​	SqlSessionFactory 对象中有个 openSession 方法用来获取 SqlSession 对象，该方法不需要传递参数。
>
> ​	（3）SqlSessionFactoryBuilder
>
> > ​	1.SqlSessionFactoryBuilder 对象有一个 build 方法，是用来获取 SqlSessionFactory 对象的，这个方法需要传递一          		个inputStream参数，input Stream来源于 mybatis-config.xml 文件，可以使用 FileInputStream 来获取。
> >
> > ​	2.MyBatis 内置的一个类Resources，该类有个静态 getResourceAsStream 方法来获取inputStream对象。这个方法		需要传递文件的位置，也可以直接写文件名，它从类的根目录下查找这个文件，并返回inputStream对象
>
> SqlSessionFactoryBuild ==> SqlSessionFactory ==> SqlSession

~~~java
package com.fengshun.mybatis.test;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.*;

public class MyBatisIntroductionTest {
    public static void main(String[] args) throws IOException {
        // 获取SQLSessionFactoryBuild对象
        SqlSessionFactoryBuilder sqlSessionFactoryBuild = new SqlSessionFactoryBuilder();
        //获取inputStream对象
        InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");
        // 获取SQLSessionFactory对象
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuild.build(inputStream);
        //  获取SQLSession
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //执行insert语句
        int num = sqlSession.insert("insertCar");//返回值是影响数据库表当中的记录条数
        //提交事务
        sqlSession.commit();
    }
}

~~~

注意：

> sqlSessionFactory 对象一般只有一个，可以使用sqlSessionFactory.openSession() 创建多个sqlSession对象.

-------

### 2.2 关于Mybatis 核心配置文件的名字和路径详情

细节：

> 1. mybatis中的sql语句的结尾可以不加“；”
>
> 2. 以后凡是遇到 Resources ，大部分情况下，这种加载资源的方式就是从类的根路径下开始加载（开始查找）
>
> 3. 使用 new FileInputStream（） 虽然也可以获取流，但是如果移动项目，那么还需要更改文件路径。
>
> 4. mybatis 的核心配置文件可以不放在类的根路径下，但是不建议这样做
>
> 5. 除了使用mybatis自带的 Resources 类加载资源以外，还可以使用java.lang包中的一个类：
>
>    ~~~java
>    InputStream is  = ClassLoader.getSystemClassLoader().getResourceAsStream("mybatis-config.xml");
>    ~~~
>
>    它也可以从类的根路径上查找对应的资源，并返回io流。
>
>    ClassLoader.getSystemClassLoader() ：获取系统类加载器。
>
>    mybatis中的 Resources.getResourceAsStream(）底层的源代码其实就是以上代码。
>
> 6. <mapper resource="CarMapper.xml"/>还有一个属性 url ，它是用来存放文件的绝对路径的。resource 也是从类的根目录中寻找文件

关于mybatis的事务管理机制

> 1. 在mybatis-config.xml文件中，可以通过一下的配置进行mybatis的事物管理
>
>    1. ~~~xml
>       <transactionManager type="JDBC"/>
>       ~~~
>
> 2. type属性的值包括两个：
>
>    1. JDBC
>    2. MANAGED
>
> 3. 在mybatis中提供了两种事务管理机制：
>
>    1. 第一种：JDBC事务管理器
>
>       > （1）mybatis框架自己管理事务，自己采用原生的JDBC代码去管理事务：
>       >
>       > ​		conn.setAutoCommit(false);  开启事务
>       >
>       > ​		conn.commit();  手动提交
>       >
>       > （2）使用JDBC事物管理器的话，底层会创建事物管理对象：JdbcTransaction对象
>       >
>       > （3）如果想要自动提交事务，可以：SqlSession sqlSession = sqlSessionFactory.openSession(false);
>
>    2. 第二种：MANAGED事务管理器
>
>       > mybatis不再负责事物的管理了。事物管理交给其他容器来负责。例如spring。

### 2.3 Mybatis 第一个比较完整的代码写法

~~~java
package com.fengshun.mybatis.test;
// 采用正确的方式，写一个完整版的mybatis程序

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MyBatisCompleteTest {
    public static void main(String[] args) {
        InputStream inputStream = null;
        SqlSession sqlSession=null;
        try {
            //获取SqlSessionFactoryBuilder对象
            SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
            //获取文件的输入流
            inputStream = Resources.getResourceAsStream("mybatis-config.xml");
            //获取SqlSessionFactory对象
            SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);
            //获取SqlSession对象
            sqlSession= sqlSessionFactory.openSession();
            //执行sql语句
            int num = sqlSession.insert("insertCar");
            //提交事物
            sqlSession.commit();
        } catch (IOException e) {
            //如果以上代码出错，则回滚事物
            //判断SqlSession是否为空，不为空则回滚事物，为空则
            if (sqlSession!=null) {
                sqlSession.rollback();
            }
            throw new RuntimeException(e);
        }finally{
            //最后关闭SqlSession
            try {
                sqlSession.close();
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        }

    }
}

~~~

### 2.4 引入Junit

引入junit

~~~xml
		<dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
~~~

在单元测试中可以使用 Assert 类的静态方法，对比期望值与实际值，如果相同则不会出错。

~~~java
    @Test
    public void test(){
        //实际值
        int actual = MyBatisComplete.insert();
        //期望值
        int expected =1;
        //对比
        Assert.assertEquals(expected,actual);
    }
~~~

### 2.5 引入日志框架 logback

常见的日志组件：

> (1)SLF4J
>
> (2)LOG4J
>
> (3)LOG4J2
>
> (4)STDOUT_LOGGING
>
> ​		可以看到连接对象什么时候创建，什么时候关闭，执行的sql语句是什么等。内容比较简单，想要获取详细的日志信息，可以使用第三方log组件
>
> (5)....
>
> 其中想使用除了 使用除了 STDOUT_LOGGING 以外的日志组件，需要使用其他的jar包，STDOUT_LOGGING 是mybatis自带的，  
>
> 其中 STDOUT_LOGGING 是标准日志，mybatis已经实现了这种标准日志，mybatis框架本身已经实现了这种标准。只要开启即可。

开启日志框架：

~~~xml
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
~~~

集成logback日志框架

> logback日志框架实现了slf4j标准。
>
> 第一步L引入logback的依赖：
>
> ~~~xml
> <dependency>
>             <groupId>ch.qos.logback</groupId>
>             <artifactId>logback-classic</artifactId>
>             <version>1.2.11</version>
> </dependency>
> ~~~
>
> 第二步：引入logback所必须的xml配置文件
>
> ​	注意：
>
> > 1.这个配置文件的名字必须是：logback.xml 或者 logback-test.xml ，不能是其他的名字。
> >
> > 2.这个配置文件必须放到类的根路径下，不能是其他位置
>
> ![image-20230818102645902](C:\Users\fengshun\AppData\Roaming\Typora\typora-user-images\image-20230818102645902.png)



### 2.6 MyBatis 工具类 sqlSessionUtil 的封装

~~~java
package com.fengshun.mybatis.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;

public class sqlSessionUtil {
    private static SqlSessionFactory sqlSessionFactory;

    // 工具类的构造方法一般都是私有化的
    // 工具类中的方法都是静态的，直接采用类名即可调用，不需要new
    // 为了防止new对象，构造方法私有化
    private sqlSessionUtil() {
    }

    // 类加载时执行，SqlSessionUtil工具类在进行第一次加载时，解析mybatis-config.xml文件，创建SqlSessionFactory对象
    static {
        // SqlSessionFactory 对象：一个SqlSessionFactory对象对应一个environment，一个environment通常是一个数据库。
        try {
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
    //定义方法，返回SqlSession对象
    public static SqlSession openSession() {
        return sqlSessionFactory.openSession();
    }
}

~~~

## 三、使用MyBatis完成CRUD

### 3.1 insert（create）

前言：

> 在第二章中，我们使用插入语句时，在CarMapper.xml文件中的sql语句并不是动态的。
>
> 在JDBC中，实现动态的sql语句是通过问号实现的：String sql ="select * from t_car where id=?"
>
> 在mybatis中，实现动态的sql语句是通过“ #{} ”实现的，“ #{} ” 和JDBC中的“ ？ ” 是等效的。

编写sql语句

> ~~~xml
> <insert id="insertCar">
>             insert into t_car(car_num,brand,guide_price,produce_time,car_type) value(#{k1},#{k2},#{k3},#{k4},#{k4})
>     </insert>
> ~~~

使用insert语句传递动态参数：

> ​	与JDBC传递参数不同，mybatis是通过对象传递参数的，这个对象可以是map映射
>
> 1.使用map传递参数：
>
> > ~~~java
> > Map<String,Object> map = new HashMap<>();
> >         map.put("k1","11111");
> >         map.put("k2","比亚迪");
> >         map.put("k3",10.0);
> >         map.put("k4","2020-12-12");
> >         map.put("k5","电车");
> >         SqlSession sqlSession = sqlSessionUtil.openSession();
> > //通过map给sql传递参数
> >         int num = sqlSession.insert("insertCar",map);
> >         sqlSession.commit();
> >         sqlSession.close();
> > ~~~
> >
> > 注意：
> >
> > 1.使用map集合给sql语句传递动态参数之后，需要在sql语句中的" #{} "中编写map的key,程序会自动调用传递过来的 对象.key
> >
> > 2.如果在#${}" 中编写的不是map的key，那么将不能通过map.get(key) 获取数据
>
> 2.使用对象传递参数(pojo)
>
> >  定义对象car
> >
> > ~~~java
> > package com.fengshun.mybatis.pojo;
> > 
> > public class Car {
> >     private Long id;
> >     private String car_num;
> >     private String brand;
> >     private Double guide_price;
> >     private String produce_time;
> >     private String car_type;
> > 
> >     public Car(Long id, String car_num, String brand, Double guide_price, String produce_time, String car_type) {
> >         this.id = id;
> >         this.car_num = car_num;
> >         this.brand = brand;
> >         this.guide_price = guide_price;
> >         this.produce_time = produce_time;
> >         this.car_type = car_type;
> >     }
> > 
> >     public Car() {
> >     }
> > 
> >     public Long getId() {
> >         return id;
> >     }
> > 
> >     public void setId(Long id) {
> >         this.id = id;
> >     }
> > 
> >     public String getCar_num() {
> >         return car_num;
> >     }
> > 
> >     public void setCar_num(String car_num) {
> >         this.car_num = car_num;
> >     }
> > 
> >     public String getBrand() {
> >         return brand;
> >     }
> > 
> >     public void setBrand(String brand) {
> >         this.brand = brand;
> >     }
> > 
> >     public Double getGuide_price() {
> >         return guide_price;
> >     }
> > 
> >     public void setGuide_price(Double guide_price) {
> >         this.guide_price = guide_price;
> >     }
> > 
> >     public String getProduce_time() {
> >         return produce_time;
> >     }
> > 
> >     public void setProduce_time(String produce_time) {
> >         this.produce_time = produce_time;
> >     }
> > 
> >     public String getCar_type() {
> >         return car_type;
> >     }
> > 
> >     public void setCar_type(String car_type) {
> >         this.car_type = car_type;
> >     }
> > 
> >     @Override
> >     public String toString() {
> >         return "Car{" +
> >                 "id=" + id +
> >                 ", car_num='" + car_num + '\'' +
> >                 ", brand='" + brand + '\'' +
> >                 ", guide_price=" + guide_price +
> >                 ", produce_time='" + produce_time + '\'' +
> >                 ", car_type='" + car_type + '\'' +
> >                 '}';
> >     }
> > }
> > 
> > ~~~
> >
> > 2.编写sql语句
> >
> > sql语句中“#{} ” 中的名字应该与pojo包中的类的属性名一致,mybatis 会自动去调用POJO对象中的get方法。
> >
> > 严格意义上说：如果使用POJO对象传递值的话，，#{} 这个大括号中写道是get方法的方法名去掉get，然后剩下的字母首字母小写。例如：getUSerName() ==> userName.
> >
> > ~~~xml
> >  <insert id="insertCar">
> >             insert into t_car(id,car_num,brand,guide_price,produce_time,car_type)
> >         value(#{id},#{car_num},#{brand},#{guide_price},#{produce_time},#{car_type})
> >     </insert>
> > ~~~
> >
> > 3.编写代码，执行sql语句
> >
> > ~~~java
> > 		Car car = new Car(null, "330", "大众", 12.30, "2021-05-30", "燃油车");
> >         SqlSession sqlSession = sqlSessionUtil.openSession();
> >         int num = sqlSession.insert("insertCar",car);
> >         sqlSession.commit();
> >         sqlSession.close();

### 3.2 delete（Delete）

1.编写sql语句

如果占位符只有一个，那么“ #{} ” 中可以随便写，但是最好见名知意

> ~~~
>  <delete id="deleteCar">
>         delete from t_car where id=#{id}
>     </delete>
> ~~~

2.执行sql语句

> ~~~java
> SqlSession sqlSession = sqlSessionUtil.openSession();
> //删除id为9的数据
>         int deleteCarNum = sqlSession.delete("deleteCar", 9);
>         sqlSession.commit();
>         sqlSession.close();
> ~~~

### 3.3 updata（Update）

1.编写sql语句

> ~~~xml
> <update id="updataCar">
>         update  t_car set car_num=#{car_num},brand=#{brand},guide_price=#{guide_price},produce_time=#{produce_time},car_type=#{car_type} where id = #{id}
>  </update>
> ~~~

2.执行sql语句

> ~~~java
>      SqlSession sqlSession = sqlSessionUtil.openSession();
>         Car car = new Car(7L, "330", "大众", 12.30, "2021-05-30", "燃油车");
>         sqlSession.update("updataCar",car);
>         sqlSession.commit();
>         sqlSession.close();
> 
> ~~~
>
> 小知识：7为int类型，想要Long类型，可以写7L

### 3.4 select（Retrieve)

查询单行记录

> 1.编写sql语句
>
> ~~~xml
> <select id="selectByid" resultType="com.fengshun.mybatis.pojo.Car">
>         select * from t_car where id=#{id}
>     </select>
> ~~~
>
> ​	注意：	
>
> ​			（1）select标签中resultType属性，这个属性来告诉mybatis，查询结果集封装成什么类型的java对象。
>
> ​			（2）resultType 通常写的是，全限定类名
>
> ​			（3）pojo中的类的属性名最好和表名一致
>
> 2.执行sql语句
>
> ~~~java
> SqlSession sqlSession = sqlSessionUtil.openSession();
>         Car selectByid = sqlSession.selectOne("selectByid", 10L);
>         System.out.println(selectByid.toString());
>         sqlSession.commit();
>         sqlSession.close();

查询所有记录

> 1.编写sql语句
>
> ~~~xml
>  <select id="selectAll" resultType="com.fengshun.mybatis.pojo.Car">
>         select * from t_car
>  </select>
> ~~~
>
> 2.执行sql函数
>
> ~~~java
>   @Test
>     public void testSelectAll(){
>         SqlSession sqlSession = sqlSessionUtil.openSession();
>         List<Car> selectAll = sqlSession.selectList("selectAll");
>         System.out.println(selectAll);
>         sqlSession.commit();
>         sqlSession.close();
>     }
> ~~~
>
> 

### 3.5 关于SQL Mapper的namespace

在sql Mapper.xml文件当中有一个namespace属性，这个属性是用来指定命名空间的。用来防止id重复。

CarMapper.xml文件：

~~~xml
<mapper namespace="car">
    <!--id是这条sql语句的唯一标识，这个id就代表了这个sql语句-->
    <insert id="insertCar">
        insert into t_car(id,car_num,brand,guide_price,produce_time,car_type)
        value(#{id},#{car_num},#{brand},#{guide_price},#{produce_time},#{car_type})
    </insert>
</mapper>
~~~

在java语句中运用CarMapper.xml文件中的 insertCar ：

~~~java
        int num = sqlSession.insert("car.insertCar", car);
~~~

## 四、MyBatis 核心配置文件详解

### 4.1 environment

介绍：

> （1）在 environments 标签中有多个 environment 子标签，每个environment 就代表着一个数据库，可以在environments中配置多个数据库。
>
> （2）在environments 标签中的有个default 属性，这个属性表示程序默认使用哪个数据库，属性的值来源于子标签environments中的id属性的值。
>
> （3）一个SQLSessionFactory 对应着一个数据库，可以使用build函数的第二个参数指定数据库id

代码实例：

~~~xml
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&amp;characterEncoding=utf-8&amp;useSSL=false&amp;serverTimezone=GMT"/>
                <property name="username" value="root"/>
                <property name="password" value="051727"/>
            </dataSource>
        </environment>
        <environment id="test">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/test?useUnicode=true&amp;characterEncoding=utf-8&amp;useSSL=false&amp;serverTimezone=GMT"/>
                <property name="username" value="root"/>
                <property name="password" value="051727"/>
            </dataSource>
        </environment>
    </environments>

~~~

使用默认数据库

~~~java
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"));
~~~

使用指定（test）数据库

~~~java
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"),"test");
~~~

### 4.2 transactionManager

配置事务管理器：指定MyBatis使用什么方式去管理事务

type属性值：

1. 第一种：JDBC事务管理器

   > （1）mybatis框架自己管理事务，自己采用原生的JDBC代码去管理事务：
   >
   > ​		conn.setAutoCommit(false);  开启事务
   >
   > ​		conn.commit();  手动提交
   >
   > （2）使用JDBC事物管理器的话，底层会创建事物管理对象：JdbcTransaction对象
   >
   > （3）如果想要自动提交事务，可以：SqlSession sqlSession = sqlSessionFactory.openSession(false);

2. 第二种：MANAGED事务管理器

   > mybatis不再负责事物的管理了。事物管理交给其他容器来负责。例如spring。

### 4.3 dataSource

dataSource ：

> （1）dataSource 被称为数据源。
>
> （2）dataSource的作用：为程序提供Connection对象（但凡是给Connection对象的都叫做数据源），
>
> （3）数据源实际上是一套规范。JDK中有这套规范：javax.sql.DataSource 接口。（这个数据源的规范，这套接口实际上是JDK		  规定的）
>
> （4）我们自己也可以编写数据源组件，只要实现javax.sql.DataSource 接口中所有的方法，就可以有自己的数据源。例如：写一个数据库连接池（数据库连接池是连接对象的，所有数据库连接池就是一个数据源）
>
> （5）其中type属性，就是指定数据源的类型，其实就是指定使用哪个数据库连接池来获取Connection对象

常见的数据源组件（数据库连接池）：

> 1.阿里巴巴的德鲁伊连接池：druid
>
> 2.C3p0
>
> 3.dbcp

type属性的值

> 1.UNPOOLED:不使用数据库连接池技术，每一次请求都是创建新的Connection对象
>
> 2.POOLED：使用mybatis自己实现的数据库连接池
>
> 3.JNDI：集成其他第三方的数据库连接池
>
> ​	JNDI是一套规范，大部分的web容器都实现了JNDI规范，例如：Tomcat、Jetty、WebLogic、WebSphere
>
> 不同类型的数据源有不同的属性，可以参考mybatis的说明文档。

![image-20230819095754585](.\图片\image-20230819095754585.png)

![image-20230819095741013](.\图片\image-20230819095741013.png)

### 4.4 properties

java.util.Properties 类。是一个map集合，key和value都是String类型

在properties中可以配置很多属性，可以通过property 标签配置多个属性，其中name相当于map中的name，value相当于map中的value。

作用：可以通过 properties 配置多个属性，提供给mybatis配置文件使用

1.在mybatis配置文件中配置多个属性

~~~xml
 <properties>
     	<property name="jdbc.driver" value="com.mysql.cj.jdbc.Driver"/>
     	<property name="jdbc.url" value="jdbc:mysql://localhost:3306/mybatis"/>
        <property name="jdbc.root" value="root"/>
        <property name="jdbc.psw" value="root"/>
    </properties>
~~~

2.使用“ ${} ” 接受参数

~~~xml
<environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.root}"/>
                <property name="password" value="${jdbc.psw}"/>
            </dataSource>
        </environment>
    </environments>
~~~

除了可以在mybatis的核心配置文件中配置多个属性以外，还能在类的根目录下新建一个properties 文件

> 1.新建一个 jdbc.properties 的配置文件到 resource文件夹中
>
> ![image-20230819101929226](.\图片\image-20230819101929226.png)
>
> 2.在mybatis核心配置文件中加载properties配置文件
>
> ~~~xml
>     <properties resource="jdbc.properties"/>  <!--resource 属性是相对路径，可以使用url属性编写绝对路径-->
> 
> ~~~
>
> 3.使用
>
> ![image-20230819102158894](.\图片\image-20230819102158894.png)

### 4.5 mapper

​	mapper的作用：通过mybatis配置文件，找到sql的映射文件。

## 五、手写MyBatis框架

### 5.1 dom4j 解析XML文件

第⼀步：引⼊dom4j的依赖

![image-20230819110320733](.\图片\image-20230819110320733.png)

第⼆步：编写配置⽂件godbatis-config.xml

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>

<configuration>
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/powernode"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
        <mappers>
            <mapper resource="sqlmapper.xml"/>
        </mappers>
    </environments>
</configuration>
~~~



第三步：解析godbatis-config.xml

~~~java
package com.powernode.dom4j;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.SAXReader;
import org.junit.Test;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * 使用dom4j解析XML文件
 */
public class ParseXMLByDom4j {
    @Test
    public void testGodBatisConfig() throws Exception{

        // 读取xml，获取document对象
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(Thread.currentThread().getContextClassLoader().getResourceAsStream("godbatis-config.xml"));

        // 获取<environments>标签的default属性的值
        Element environmentsElt = (Element)document.selectSingleNode("/configuration/environments");
        String defaultId = environmentsElt.attributeValue("default");
        System.out.println(defaultId);

        // 获取environment标签
        Element environmentElt = (Element)document.selectSingleNode("/configuration/environments/environment[@id='" + defaultId + "']");

        // 获取事务管理器类型
        Element transactionManager = environmentElt.element("transactionManager");
        String transactionManagerType = transactionManager.attributeValue("type");
        System.out.println(transactionManagerType);

        // 获取数据源类型
        Element dataSource = environmentElt.element("dataSource");
        String dataSourceType = dataSource.attributeValue("type");
        System.out.println(dataSourceType);

        // 将数据源信息封装到Map集合
        Map<String,String> dataSourceMap = new HashMap<>();
        dataSource.elements().forEach(propertyElt -> {
            dataSourceMap.put(propertyElt.attributeValue("name"), propertyElt.attributeValue("value"));
        });

        dataSourceMap.forEach((k, v) -> System.out.println(k + ":" + v));

        // 获取sqlmapper.xml文件的路径
        Element mappersElt = (Element) document.selectSingleNode("/configuration/environments/mappers");
        mappersElt.elements().forEach(mapper -> {
            System.out.println(mapper.attributeValue("resource"));
        });
    }
}
~~~

执行结果：

![image-20230824192508858](.\图片\image-20230824192508858.png)

第四步：编写配置⽂件sqlmapper.xml

~~~~xml
<?xml version="1.0" encoding="UTF-8" ?>

<mapper namespace="car">
    <insert id="insertCar">
        insert into t_car(id,car_num,brand,guide_price,produce_time,car_type) values(null,#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})
    </insert>
    <select id="selectCarByCarNum" resultType="com.powernode.mybatis.pojo.Car">
        select id,car_num carNum,brand,guide_price guidePrice,produce_time produceTime,car_type carType from t_car where car_num = #{carNum}
    </select>
</mapper>
~~~~



第五步：解析sqlmapper.xml

~~~java
@Test
public void testSqlMapper() throws Exception{
    // 读取xml，获取document对象
    SAXReader saxReader = new SAXReader();
    Document document = saxReader.read(Thread.currentThread().getContextClassLoader().getResourceAsStream("sqlmapper.xml"));

    // 获取namespace
    Element mapperElt = (Element) document.selectSingleNode("/mapper");
    String namespace = mapperElt.attributeValue("namespace");
    System.out.println(namespace);

    // 获取sql id
    mapperElt.elements().forEach(statementElt -> {
        // 标签名
        String name = statementElt.getName();
        System.out.println("name:" + name);
        // 如果是select标签，还要获取它的resultType
        if ("select".equals(name)) {
            String resultType = statementElt.attributeValue("resultType");
            System.out.println("resultType:" + resultType);
        }
        // sql id
        String id = statementElt.attributeValue("id");
        System.out.println("sqlId:" + id);
        // sql语句
        String sql = statementElt.getTextTrim();
        System.out.println("sql:" + sql);
    });
}
~~~

### 5.2 GodBatis

#### 第一步：IDEA中创建模块

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.god</groupId>
    <artifactId>godbatis</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <dependencies>
        <!--dom4j依赖-->
        <dependency>
            <groupId>org.dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>2.1.3</version>
        </dependency>
        <!--jaxen依赖-->
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>1.2.0</version>
        </dependency>
        <!--junit依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

</project>
~~~



#### 第二步：资源工具类，方便获取指向配置文件的输入流

~~~java
package org.god.core;

import java.io.InputStream;

/**
 * 资源工具类
 * @author 老杜
 * @version 1.0
 * @since 1.0
 */
public class Resources {

    /**
     * 从类路径中获取配置文件的输入流
     * @param config
     * @return 输入流，该输入流指向类路径中的配置文件
     */
    public static InputStream getResourcesAsStream(String config){
        return Thread.currentThread().getContextClassLoader().getResourceAsStream(config);
    }
}
~~~

#### 第三步：定义SqlSessionFaCtoryBuilder 类

~~~java
package org.god.core;

import java.io.InputStream;

/**
 * SqlSessionFactory对象构建器
 * @author 老杜
 * @version 1.0
 * @since 1.0
 */
public class SqlSessionFactoryBuilder {

    /**
     * 创建构建器对象
     */
    public SqlSessionFactoryBuilder() {
    }


    /**
     * 获取SqlSessionFactory对象
     * 该方法主要功能是：读取godbatis核心配置文件，并构建SqlSessionFactory对象
     * @param inputStream 指向核心配置文件的输入流
     * @return SqlSessionFactory对象
     */
    public SqlSessionFactory build(InputStream inputStream){
        // 解析配置文件，创建数据源对象
        // 解析配置文件，创建事务管理器对象
        // 解析配置文件，获取所有的SQL映射对象
        // 将以上信息封装到SqlSessionFactory对象中
        // 返回
        return null;
    }
}
~~~



#### 第四步：分析SQLSessionFactory 类中有哪些属性

- 事务管理器

- - GodJDBCTransaction

- SQL映射对象集合

- - Map<String, GodMappedStatement>

#### 第五步：定义GodJDBCTransaction

TransactionManager 类：

~~~java
package org.god.core;

import java.sql.Connection;

/**
 * 事务管理器接口
 * @author 老杜
 * @version 1.0
 * @since 1.0
 */
public interface TransactionManager {
    /**
     * 提交事务
     */
    void commit();

    /**
     * 回滚事务
     */
    void rollback();

    /**
     * 关闭事务
     */
    void close();

    /**
     * 开启连接
     */
    void openConnection();
    
    /**
     * 获取连接对象
     * @return 连接对象
     */
    Connection getConnection();
}

~~~

GodJDBCTransaction 类：

~~~java
package org.god.core;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

/**
 * 事务管理器
 * @author 老杜
 * @version 1.0
 * @since 1.0
 */
public class GodJDBCTransaction implements TransactionManager {
    /**
     * 连接对象，控制事务时需要
     */
    private Connection conn;

    /**
     * 数据源对象
     */
    private DataSource dataSource;

    /**
     * 自动提交标志：
     * true表示自动提交
     * false表示不自动提交
     */
    private boolean autoCommit;

    /**
     * 构造事务管理器对象
     * @param autoCommit
     */
    public GodJDBCTransaction(DataSource dataSource, boolean autoCommit) {
        this.dataSource = dataSource;
        this.autoCommit = autoCommit;
    }

    /**
     * 提交事务
     */
    public void commit(){
        try {
            conn.commit();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 回滚事务
     */
    public void rollback(){
        try {
            conn.rollback();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void close() {
        try {
            conn.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void openConnection() {
        try {
            this.conn = dataSource.getConnection();
            this.conn.setAutoCommit(this.autoCommit);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public Connection getConnection() {
        return conn;
    }
}
~~~



#### 第六步：事物管理器中需要数据源，定义 GodUNPOOLEDDataSource

~~~
package org.god.core;

import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.SQLFeatureNotSupportedException;
import java.util.logging.Logger;

/**
 * 数据源实现类，不使用连接池
 * @author 老杜
 * @version 1.0
 * @since 1.0
 */
public class GodUNPOOLEDDataSource implements javax.sql.DataSource{
    private String url;
    private String username;
    private String password;

    public GodUNPOOLEDDataSource(String driver, String url, String username, String password) {
        try {
            // 注册驱动
            Class.forName(driver);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
        this.url = url;
        this.username = username;
        this.password = password;
    }

    @Override
    public Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, username, password);
    }

    @Override
    public Connection getConnection(String username, String password) throws SQLException {
        return null;
    }

    @Override
    public PrintWriter getLogWriter() throws SQLException {
        return null;
    }

    @Override
    public void setLogWriter(PrintWriter out) throws SQLException {

    }

    @Override
    public void setLoginTimeout(int seconds) throws SQLException {

    }

    @Override
    public int getLoginTimeout() throws SQLException {
        return 0;
    }

    @Override
    public Logger getParentLogger() throws SQLFeatureNotSupportedException {
        return null;
    }

    @Override
    public <T> T unwrap(Class<T> iface) throws SQLException {
        return null;
    }

    @Override
    public boolean isWrapperFor(Class<?> iface) throws SQLException {
        return false;
    }
}
~~~

#### 第七步：定义GodMappedStatement

~~~java
package org.god.core;

/**
 * SQL映射实体类
 * @author 老杜
 * @version 1.0
 * @since 1.0
 */
public class GodMappedStatement {
    private String sqlId;
    private String resultType;
    private String sql;
    private String parameterType;

    private String sqlType;

    @Override
    public String toString() {
        return "GodMappedStatement{" +
                "sqlId='" + sqlId + '\'' +
                ", resultType='" + resultType + '\'' +
                ", sql='" + sql + '\'' +
                ", parameterType='" + parameterType + '\'' +
                ", sqlType='" + sqlType + '\'' +
                '}';
    }

    public String getSqlId() {
        return sqlId;
    }

    public void setSqlId(String sqlId) {
        this.sqlId = sqlId;
    }

    public String getResultType() {
        return resultType;
    }

    public void setResultType(String resultType) {
        this.resultType = resultType;
    }

    public String getSql() {
        return sql;
    }

    public void setSql(String sql) {
        this.sql = sql;
    }

    public String getParameterType() {
        return parameterType;
    }

    public void setParameterType(String parameterType) {
        this.parameterType = parameterType;
    }

    public String getSqlType() {
        return sqlType;
    }

    public void setSqlType(String sqlType) {
        this.sqlType = sqlType;
    }

    public GodMappedStatement(String sqlId, String resultType, String sql, String parameterType, String sqlType) {
        this.sqlId = sqlId;
        this.resultType = resultType;
        this.sql = sql;
        this.parameterType = parameterType;
        this.sqlType = sqlType;
    }
}


~~~



#### 第八步：完善SqlSessionFactory类
~~~java
package org.god.core;

import javax.sql.DataSource;
import java.util.List;
import java.util.Map;

/**
 * SqlSession工厂对象，使用SqlSessionFactory可以获取会话对象
 * @author 老杜
 * @version 1.0
 * @since 1.0
 */
public class SqlSessionFactory {
    private TransactionManager transactionManager;
    private Map<String, GodMappedStatement> mappedStatements;

    public SqlSessionFactory(TransactionManager transactionManager, Map<String, GodMappedStatement> mappedStatements) {
        this.transactionManager = transactionManager;
        this.mappedStatements = mappedStatements;
    }

    public TransactionManager getTransactionManager() {
        return transactionManager;
    }

    public void setTransactionManager(TransactionManager transactionManager) {
        this.transactionManager = transactionManager;
    }

    public Map<String, GodMappedStatement> getMappedStatements() {
        return mappedStatements;
    }

    public void setMappedStatements(Map<String, GodMappedStatement> mappedStatements) {
        this.mappedStatements = mappedStatements;
    }
}


~~~


#### 第九步：完善SqlSessionFactoryBuilder中的build⽅法
~~~java
package org.god.core;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import javax.sql.DataSource;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;

/**
 * SqlSessionFactory对象构建器
 *
 * @author 老杜
 * @version 1.0
 * @since 1.0
 */
public class SqlSessionFactoryBuilder {

    /**
     * 创建构建器对象
     */
    public SqlSessionFactoryBuilder() {
    }


    /**
     * 获取SqlSessionFactory对象
     * 该方法主要功能是：读取godbatis核心配置文件，并构建SqlSessionFactory对象
     *
     * @param inputStream 指向核心配置文件的输入流
     * @return SqlSessionFactory对象
     */
    public SqlSessionFactory build(InputStream inputStream) throws DocumentException {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(inputStream);
        Element environmentsElt = (Element) document.selectSingleNode("/configuration/environments");
        String defaultEnv = environmentsElt.attributeValue("default");
        Element environmentElt = (Element) document.selectSingleNode("/configuration/environments/environment[@id='" + defaultEnv + "']");
        // 解析配置文件，创建数据源对象
        Element dataSourceElt = environmentElt.element("dataSource");
        DataSource dataSource = getDataSource(dataSourceElt);
        // 解析配置文件，创建事务管理器对象
        Element transactionManagerElt = environmentElt.element("transactionManager");
        TransactionManager transactionManager = getTransactionManager(transactionManagerElt, dataSource);
        // 解析配置文件，获取所有的SQL映射对象
        Element mappers = environmentsElt.element("mappers");
        Map<String, GodMappedStatement> mappedStatements = getMappedStatements(mappers);
        // 将以上信息封装到SqlSessionFactory对象中
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactory(transactionManager, mappedStatements);
        // 返回
        return sqlSessionFactory;
    }

    private Map<String, GodMappedStatement> getMappedStatements(Element mappers) {
        Map<String, GodMappedStatement> mappedStatements = new HashMap<>();
        mappers.elements().forEach(mapperElt -> {
            try {
                String resource = mapperElt.attributeValue("resource");
                SAXReader saxReader = new SAXReader();
                Document document = saxReader.read(Resources.getResourcesAsStream(resource));
                Element mapper = (Element) document.selectSingleNode("/mapper");
                String namespace = mapper.attributeValue("namespace");

                mapper.elements().forEach(sqlMapper -> {
                    String sqlId = sqlMapper.attributeValue("id");
                    String sql = sqlMapper.getTextTrim();
                    String parameterType = sqlMapper.attributeValue("parameterType");
                    String resultType = sqlMapper.attributeValue("resultType");
                    String sqlType = sqlMapper.getName().toLowerCase();
                    // 封装GodMappedStatement对象
                    GodMappedStatement godMappedStatement = new GodMappedStatement(sqlId, resultType, sql, parameterType, sqlType);
                    mappedStatements.put(namespace + "." + sqlId, godMappedStatement);
                });

            } catch (DocumentException e) {
                throw new RuntimeException(e);
            }
        });
        return mappedStatements;
    }


    private TransactionManager getTransactionManager(Element transactionManagerElt, DataSource dataSource) {
        String type = transactionManagerElt.attributeValue("type").toUpperCase();
        TransactionManager transactionManager = null;
        if ("JDBC".equals(type)) {
            // 使用JDBC事务
            transactionManager = new GodJDBCTransaction(dataSource, false);
        } else if ("MANAGED".equals(type)) {
            // 事务管理器是交给JEE容器的
        }
        return transactionManager;
    }

    private DataSource getDataSource(Element dataSourceElt) {
        // 获取所有数据源的属性配置
        Map<String, String> dataSourceMap = new HashMap<>();
        dataSourceElt.elements().forEach(propertyElt -> {
            dataSourceMap.put(propertyElt.attributeValue("name"), propertyElt.attributeValue("value"));
        });

        String dataSourceType = dataSourceElt.attributeValue("type").toUpperCase();
        DataSource dataSource = null;
        if ("POOLED".equals(dataSourceType)) {

        } else if ("UNPOOLED".equals(dataSourceType)) {
            dataSource = new GodUNPOOLEDDataSource(dataSourceMap.get("driver"), dataSourceMap.get("url"), dataSourceMap.get("username"), dataSourceMap.get("password"));
        } else if ("JNDI".equals(dataSourceType)) {

        }
        return dataSource;
    }
}

~~~


#### 第十步：在SqlSessionFactory中添加openSession⽅法
~~~java
public SqlSession openSession(){
    transactionManager.openConnection();
    SqlSession sqlSession = new SqlSession(transactionManager, mappedStatements);
    return sqlSession;
}
~~~


#### 第十一步：编写SqlSession类中commit rollback close⽅法
~~~java
package org.god.core;

import java.sql.SQLException;
import java.util.Map;

/**
 * 数据库会话对象
 * @author 老杜
 * @version 1.0
 * @since 1.0
 */
public class SqlSession {
    private TransactionManager transactionManager;
    private Map<String, GodMappedStatement> mappedStatements;

    public SqlSession(TransactionManager transactionManager, Map<String, GodMappedStatement> mappedStatements) {
        this.transactionManager = transactionManager;
        this.mappedStatements = mappedStatements;
    }

    public void commit(){
        try {
            transactionManager.getConnection().commit();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public void rollback(){
        try {
            transactionManager.getConnection().rollback();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public void close(){
        try {
            transactionManager.getConnection().close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}

~~~


#### 第十二步：编写SqlSession类中的insert⽅法
~~~java
/**
 * 插入数据
 *
 * @param sqlId 要执行的sqlId
 * @param obj   插入的数据
 * @return
 */
public int insert(String sqlId, Object obj) {
    GodMappedStatement godMappedStatement = mappedStatements.get(sqlId);
    Connection connection = transactionManager.getConnection();
    // 获取sql语句
    // insert into t_car(id,car_num,brand,guide_price,produce_time,car_type) values(null,#{carNum},#{brand},#{guidePrice},#{produceTime},#{carType})
    String godbatisSql = godMappedStatement.getSql();
    // insert into t_car(id,car_num,brand,guide_price,produce_time,car_type) values(null,?,?,?,?,?)
    String sql = godbatisSql.replaceAll("#\\{[a-zA-Z0-9_\\$]*}", "?");

    // 重点一步
    Map<Integer, String> map = new HashMap<>();
    int index = 1;
    while (godbatisSql.indexOf("#") >= 0) {
        int beginIndex = godbatisSql.indexOf("#") + 2;
        int endIndex = godbatisSql.indexOf("}");
        map.put(index++, godbatisSql.substring(beginIndex, endIndex).trim());
        godbatisSql = godbatisSql.substring(endIndex + 1);
    }

    final PreparedStatement ps;
    try {
        ps = connection.prepareStatement(sql);

        // 给?赋值
        map.forEach((k, v) -> {
            try {
                // 获取java实体类的get方法名
                String getMethodName = "get" + v.toUpperCase().charAt(0) + v.substring(1);
                Method getMethod = obj.getClass().getDeclaredMethod(getMethodName);
                ps.setString(k, getMethod.invoke(obj).toString());
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        });
        int count = ps.executeUpdate();
        ps.close();
        return count;
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
}
~~~


#### 第十三步：编写SqlSession类中的selectOne⽅法
~~~java
/**
 * 查询一个对象
 * @param sqlId
 * @param parameterObj
 * @return
 */
public Object selectOne(String sqlId, Object parameterObj){
    GodMappedStatement godMappedStatement = mappedStatements.get(sqlId);
    Connection connection = transactionManager.getConnection();
    // 获取sql语句
    String godbatisSql = godMappedStatement.getSql();
    String sql = godbatisSql.replaceAll("#\\{[a-zA-Z0-9_\\$]*}", "?");
    // 执行sql
    PreparedStatement ps = null;
    ResultSet rs = null;
    Object obj = null;
    try {
        ps = connection.prepareStatement(sql);
        ps.setString(1, parameterObj.toString());
        rs = ps.executeQuery();
        if (rs.next()) {
            // 将结果集封装对象，通过反射
            String resultType = godMappedStatement.getResultType();
            Class<?> aClass = Class.forName(resultType);
            Constructor<?> con = aClass.getDeclaredConstructor();
            obj = con.newInstance();
            // 给对象obj属性赋值
            ResultSetMetaData rsmd = rs.getMetaData();
            int columnCount = rsmd.getColumnCount();
            for (int i = 1; i <= columnCount; i++) {
                String columnName = rsmd.getColumnName(i);
                String setMethodName = "set" + columnName.toUpperCase().charAt(0) + columnName.substring(1);
                Method setMethod = aClass.getDeclaredMethod(setMethodName, aClass.getDeclaredField(columnName).getType());
                setMethod.invoke(obj, rs.getString(columnName));
            }
        }
    } catch (Exception e) {
        throw new RuntimeException(e);
    } finally {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        try {
            ps.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
    return obj;
}
~~~






### 5.3 GodBatis 使用Maven打包

### 5.4 使用 GodBatis

### 5.5 总结MyBatis重要实现原理

## 六、在WEB应用中使用MyBatis（使用MVC模式）

### 6.1 项目开发

需求：实现银行转账功能，输入转账方账号和接收方账号、金额，实现转账功能

使用MVC模式，创建项目

![image-20230819222528892](.\图片\image-20230819222528892.png)

1. 定义sqlSessUtil 工具类，用来专门获取SqlSession对象

   ~~~java
   package com.fengshun.mybatis.utils;
   
   import org.apache.ibatis.io.Resources;
   import org.apache.ibatis.session.SqlSession;
   import org.apache.ibatis.session.SqlSessionFactory;
   import org.apache.ibatis.session.SqlSessionFactoryBuilder;
   
   import java.io.IOException;
   
   public class sqlSessionUtil {
       private static SqlSessionFactory sqlSessionFactory;
   
       // 工具类的构造方法一般都是私有化的
       // 工具类中的方法都是静态的，直接采用类名即可调用，不需要new
       // 为了防止new对象，构造方法私有化
       private sqlSessionUtil() {
       }
   
       // 类加载时执行，SqlSessionUtil工具类在进行第一次加载时，解析mybatis-config.xml文件，创建SqlSessionFactory对象
       static {
           // SqlSessionFactory 对象：一个SqlSessionFactory对象对应一个environment，一个environment通常是一个数据库。
           try {
               sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
           } catch (IOException e) {
   
               throw new RuntimeException(e);
           }
       }
       private static ThreadLocal<SqlSession> local = new ThreadLocal<>();
       //定义方法，返回SqlSession对象
       public static SqlSession openSession() {
           SqlSession sqlSession = local.get();
           if (sqlSession==null){
              sqlSession = sqlSessionFactory.openSession();
              //将SQLSession对象绑定到当前线程中
           }
           return sqlSession;
       }
       //关闭sqlSession对象(从当前线程中移除SqlSession对象)
       public static void close(SqlSession sqlSession){
           if (sqlSession!=null) {
               sqlSession.close();
               local.remove();
           }
   
       }
   }
   
   ~~~

2. 定义异常类，用来处理不同类型的异常

   > ~~~java
   > package com.fengshun.mybatis.exceptions;
   > /**余额不足时，触发的异常
   >  * */
   > public class MoneyNotEnoughException extends Exception{
   >     public MoneyNotEnoughException() {
   >     }
   > 
   >     public MoneyNotEnoughException(String message) {
   >         super(message);
   >     }
   > }
   > 
   > ~~~
   >
   > 运用：![image-20230819222801811](.\图片\image-20230819222801811.png)
   >
   > 如果发现异常，则跳转到其他页面或者返回错误信息。

3.使用Threadlocal管理事务

> 在传统的实现对数据库增删改查时，会编写多个方法，方法中会获取SqlSession对象，用于执行sql语句，每次方法结束就会关闭SqlSession对象，这意味着调用一次方法就是一个进程，就是一个事物。对于银行转账这样的功能来说，从查询转账方余额到转账结束，最好用同一个事物去管理，也就是使用一个SqlSession对象。
>
> 为了达到这种效果，在dao中就不能提交事物并关闭SqlSession，在一个线程中还需要使用同一个SqlSession对象。可以使用ThreadLocal达到这种目的。
>
> ![image-20230819223806418](.\图片\image-20230819223806418.png)
>
> 运用：让一个线程或者说一个功能使用一个事物。![image-20230819224215983](.\图片\image-20230819224215983.png)使用Threadlocal 之后，dao层的代码就变得非常的简介，根本不需要我们去编写dao层。

------

### 6.2MyBatis对象作⽤域以及事务问题

#### MyBatis核⼼对象的作⽤域

**SqlSessionFactoryBuilder**

​		这个类可以被实例化、使⽤和丢弃，⼀旦创建了 SqlSessionFactory，就不再需要它了。 因此SqlSessionFactoryBuilder 实例的最佳作⽤域是⽅法作⽤域（也就是局部⽅法变量）。 你可以重⽤SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但最好还是不要⼀直保留着它，以保证所有的 XML 解析资源可以被释放给更重要的事情。

**SqlSessionFactory**

​		SqlSessionFactory ⼀旦被创建就应该在应⽤的运⾏期间⼀直存在，没有任何理由丢弃它或重新创建另⼀个实例。 使⽤SqlSessionFactory 的最佳实践是在应⽤运⾏期间不要重复创建多次，多次重建 SqlSessionFactory被视为⼀种代码“坏习惯”。因此 SqlSessionFactory 的最佳作⽤域是应⽤作⽤域。 有很多⽅法可以做到，最简单的就是使⽤单例模式或者静态单例模式。

**SqlSession**

​		每个线程都应该有它⾃⼰的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作⽤域是请求或⽅法作⽤域。 绝对不能将 SqlSession 实例的引⽤放在⼀个类的静态域，甚⾄⼀个类的实例变量也不⾏。 也绝不能将 SqlSession 实例的引⽤放在任何类型的托管作⽤域中，⽐如 Servlet 框架中的HttpSession。 如果你现在正在使⽤⼀种 Web 框架，考虑将 SqlSession 放在⼀个和 HTTP 请求相似的作⽤域中。 换句话说，每次收到 HTTP 请求，就可以打开⼀个 SqlSession，返回⼀个响应后，就关闭它。 这个关闭操作很重要，为了确保每次都能执⾏关闭操作，你应该把这个关闭操作放到 finally 块中。下⾯的示例就是⼀个确保 SqlSession 关闭的标准模式：

~~~java
try(SqlSession sqlSession = sqlSessionFactory.openSession()){
		//应用逻辑代码
}
~~~

## 七、使用 javassist 生成类



### 7.1 使用 javassist 生成普通java类

1.导入依赖

~~~xml
<dependency>
            <groupId>org.javassist</groupId>
            <artifactId>javassist</artifactId>
            <version>3.20.0-GA</version>
  </dependency>
~~~

2.编写代码

~~~java
 public void testGenerateFristClass() throws Exception{
        //获取类池，这个类池就是用来生成class的
        ClassPool pool = ClassPool.getDefault();
        //制造类
        CtClass ctClass = pool.makeClass("com.fengshun.mybatis.dao.impl.AccountDaoImpl");
        //制造方法
        String methodCode = "public void insert(){ System.out.println(123);}";
        CtMethod ctMethod = CtMethod.make("methodCode", ctClass);
        //将方法添加到类中
        ctClass.addMethod(ctMethod);
        // 从内存中生成class
        ctClass.toClass();

        //类加载到JVM当中，返回AccountDaoImpl 类的字节码
        Class<?> aClass = Class.forName("com.fengshun.mybatis.dao.impl.AccountDaoImpl");
        // 创建对象
        Object obj = aClass.newInstance();
        // 获取AccountDaoImpl中的insert方法
        Method insertMethod = aClass.getDeclaredMethod("insert");
        //调用insert方法
        insertMethod.invoke(obj);
    }
~~~

运⾏要注意：加两个参数，要不然会有异常。

> （1）--add-opens java.base/java.lang=ALL-UNNAMED
>
> （2）--add-opens java.base/sun.net.util=ALL-UNNAMED

![image-20230820092840095](.\图片\image-20230820092840095.png)



### 7.2 使用 javassist 生成类并实现接口

~~~java
public void testGennerrateAccountDaoImpl() throws Exception {
        // 获取类池
        ClassPool classPool = ClassPool.getDefault();
        // 制造类
        CtClass ctClass = classPool.makeClass("com.fengshun.javassist.dao.impl.AccountDaoImpl");
        // 制造接口
        CtClass ctInterface = classPool.makeInterface("com.fengshun.javassist.dao.AccounrDao");
        // 实现接口
        ctClass.addInterface(ctInterface);
        // 获取接口中所有的方法
        Method[] methods = AccounrDao.class.getDeclaredMethods();
        Arrays.stream(methods).forEach(method -> {
            // method 就是接口中的方法
            // 把method抽象方法给实现了
            // 拼接字符串，构成完整的方法
            StringBuilder methodCode = new StringBuilder();
            methodCode.append("public ");// 追加修饰符列表
            methodCode.append(method.getReturnType().getName() + " ");
            methodCode.append(method.getName() + "(");// 追加方法名
            // 获取方法参数的类型
            Class<?>[] parameterTypes = method.getParameterTypes();
            for (int i = 0; i < parameterTypes.length; i++) {
                methodCode.append(parameterTypes[i].getName());
                methodCode.append(" arg" + i);
                if (i != parameterTypes.length - 1) {
                    methodCode.append(",");
                }
            }
            methodCode.append("){");
            // 拼接方法体中的代码
            methodCode.append("System.out.print(1265);");
            // 判断该方法是否需要返回值
            if ("void".equals(method.getReturnType().getSimpleName())) {

            } else if ("int".equals(method.getReturnType().getSimpleName())) {
                methodCode.append("return 1");
            } else if ("String".equals(method.getReturnType().getSimpleName())) {
                methodCode.append("return \"akdh\"");
            }
            methodCode.append("}");
            try {
                // 创建方法
                CtMethod ctMethod = CtMethod.make(methodCode.toString(), ctClass);
                // 将方法添加到类中
                ctClass.addMethod(ctMethod);
                // 创建对象
                Class toClass = ctClass.toClass();
                // 调用方法
                AccounrDao accounrDao = (AccounrDao) toClass.newInstance();
                accounrDao.insert("s");
            } catch (Exception e) {
                e.printStackTrace();
            }
        });

    } 
~~~

### 7.3使用javassist工具，动态生成dao层的接口的实现类

在dao层中，需要先定义接口，再在dao.impl包下定义类并实现接口，操作单独的一条sql语句。在service层中调用dao层中实现类的多个方法完成一个功能，这意味着这个功能从开始到结束，都只需要一个事物去管理。所以事物的提交或回滚将在servic中进行。并不在dao层。

dao层的代码如下：

~~~java
    public int updateActno(Act act) {
        SqlSession sqlSession = sqlSessionUtil.openSession();
        int updatenum = sqlSession.update("account.updateActMoney",act);
        return updatenum;
    }
~~~

可以看出dao层中的每个方法的方法体比较简单，只需要获取SqlSession对象，并执行SQL语句即可，其代码比较固定。为了简化程序，可以使用javassist动态生成类，生成的这个类去实现我们事先定义好的接口，并实现相应的方法。（简单来讲，就是通过javassist编写一个工具类，通过这个类帮助我们动态生成dao层中的实现类）。

代码如下：

~~~java
package com.fengshun.mybatis.utils;

import org.apache.ibatis.javassist.CannotCompileException;
import org.apache.ibatis.javassist.ClassPool;
import org.apache.ibatis.javassist.CtClass;
import org.apache.ibatis.javassist.CtMethod;
import org.apache.ibatis.mapping.SqlCommandType;
import org.apache.ibatis.session.SqlSession;

import java.lang.reflect.Method;
import java.util.Arrays;

// mybatis 号称轻量级，只需要一个jar包就行了
public class GenerateDaoProxy {
    /**
     * 生成dao接口实现类，并且将实现类的对象创建出来并返回
     */
    public static Object generate(SqlSession sqlSession, Class daoInterface) {
        // mybatis内置了javassist
        ClassPool pool = ClassPool.getDefault();
        // 制造类
        // 实际本质上就是在内存中动态生成一个代理类
        final CtClass ctClass = pool.makeClass(daoInterface.getName() + "impl");
        // 制造接口
        CtClass ctInterface = pool.makeInterface(daoInterface.getName());
        // 实现接口
        ctClass.addInterface(ctInterface);
        // 实现接口中所有的方法
        Method[] methods = daoInterface.getDeclaredMethods();
        Arrays.stream(methods).forEach(method -> {
            // mehtod 是抽象方法

            // 获取接口中的方法体，并且拼接字符串
            StringBuilder methodCode = new StringBuilder();
            methodCode.append("public ");
            // 获取返回值类型
            methodCode.append(method.getReturnType().getName());
            methodCode.append(" ");
            // 获取方法名
            methodCode.append(method.getName());
            methodCode.append("(");
            // 需要参数列表
            Class<?>[] parameterTypes = method.getParameterTypes();
            for (int i = 0; i < parameterTypes.length; i++) {
                Class<?> parameterType = parameterTypes[i];
                methodCode.append(parameterType.getName());
                methodCode.append(" ");
                methodCode.append("arg" + i);
                if (i != parameterTypes.length - 1) {
                    methodCode.append(",");
                }
            }
            methodCode.append(")");
            methodCode.append("{");
            // 方法体中的代码
            /**代码片段中的类，必须要包名*/
            methodCode.append("org.apache.ibatis.session.SqlSession sqlSession = com.fengshun.mybatis.utils.sqlSessionUtil.openSession();\n");
            // 需要知道是什么类型的sql语句
            /**
             * sql语句的id是框架使用者提供的，具有多变性，对于框架的开发人员来说，并不知道id
             * 既然框架开发者不知道sqlid，mybatis框架的开发者于是就出台了一个规定，凡是使用GenerateDaoProxy机制的
             * sqlid不能随便写，namespace必须是dao接口的全限定门窗。id必须是dao接口中的方法*/
            String sqlid  = daoInterface.getName()+"."+method.getName();
            SqlCommandType sqlCommandType = sqlSession.getConfiguration().getMappedStatement(sqlid).getSqlCommandType();
            if (sqlCommandType == SqlCommandType.DELETE) {
                methodCode.append("sqlSession.delete(\""+sqlid+"\",arg0)");
            }
            if (sqlCommandType == SqlCommandType.UPDATE) {
                methodCode.append(" return sqlSession.update(\""+sqlid+"\",arg0);");
            }
            if (sqlCommandType == SqlCommandType.SELECT) {
                methodCode.append("return ("+method.getReturnType().getName()+")sqlSession.select(\""+sqlid+"\",arg0)");
            }
            if (sqlCommandType == SqlCommandType.INSERT) {

            }
            methodCode.append("}");
            // 将method这个抽象方法进行实现
            try {
                CtMethod ctMethod = CtMethod.make(methodCode.toString(), ctClass);
                ctClass.addMethod(ctMethod);
            } catch (Exception e) {
                e.printStackTrace();
            }
        });
        // 创建对象
        Object obj = null;
        try {
            Class<?> clazz = ctClass.toClass();
            obj = clazz.newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return obj;
    }
}

~~~

这是我们自己通过javassist这个jar包，编写的工具类，用来动态生成代码。在mybatis中内置了 javassist ，mybatis也给我们提供了一个工具类，来帮助我们动态生成代码。



## 八、MyBatis中接口代理机制及使用

### 8.1 mybatis中的接口代理

说明：

> ​	在MyBatis中。MyBatis提供了相关的机制，也可以动态为我们生成dao接口的实现类。（代理类：dao解扣子的代理）
>
> ​	MyBatis当中实际上采用了代理模式，在内存中生成dao接口的代理类，然后创建代理类的实例。

<u>***使用mybatis这种代理机制的前提：SqlMapper.xml文件中，namespace 必须是dao接口的全限定名称 ( 包名加接口名 ) ，id必须是dao接口中的方法名。***</u>

语法：  接口名  接口对象名 =  SqlSession对象 . getMapper（ 接口名.class ）

~~~java
    private AccountDao accountDao  =sqlSessionUtil.openSession().getMapper(AccountDao.class);
/**AccountDao是一个接口
*sqlSessionUtil 是一个工具类，其中的openSession() 用来获取SqlSession对象
*/
~~~

### 8.2 使用接口代理机制实现CRUD

1.查询单个

> ~~~java
>     public void testSelectCar() {
>         SqlSession sqlSession = SqlSessionUtil.openSession();
>         CarDao mapper = sqlSession.getMapper(CarDao.class);
>         Car car = mapper.selectCar(10L);
>         System.out.println(car);
>         sqlSession.close();
>     }
> ~~~

2.查询所有

> ~~~java
>     public void testSelectAll() {
>         SqlSession sqlSession = SqlSessionUtil.openSession();
>         CarDao mapper = sqlSession.getMapper(CarDao.class);
>         List<Car> cars = mapper.selectAllCar();
>         System.out.println(cars.toString());
> 
>     }
> ~~~

3.更新

> ~~~java
>     public void testupdate() {
>         SqlSession sqlSession = SqlSessionUtil.openSession();
>         CarDao mapper = sqlSession.getMapper(CarDao.class);
>         Car car = new Car(11L, "30", "大众", 12.30, "2021-05-30", "燃油车");
>         mapper.updateCar(car);
>         sqlSession.commit();
>         sqlSession.close();
> 
>     }
> ~~~

4.添加

> ~~~java
>     public void testInsert() {
>         SqlSession sqlSession = SqlSessionUtil.openSession();
>         CarDao mapper = sqlSession.getMapper(CarDao.class);
>         Car car = new Car(2L, "330", "大众", 12.30, "2021-05-30", "燃油车");
>         int i = mapper.insertCar(car);
>         System.out.println(i);
>         sqlSession.commit();
>         sqlSession.close();
> 
>     }
> ~~~

5.删除

> ~~~java
>     public void testDelete() {
>         SqlSession sqlSession = SqlSessionUtil.openSession();
>         CarDao mapper = sqlSession.getMapper(CarDao.class);
>         int i = mapper.deleteCar(2L);
>         System.out.println(i);
>         sqlSession.commit();
>         sqlSession.close();
>     }
> ~~~

## 九、MyBatis小技巧

###  9.1 #{ } 和 ${ }

​	#{}：先编译sql语句，再给占位符传值，底层是PreparedStatement实现。可以防⽌sql注⼊，⽐较常⽤。

- ~~~sql
  select * from t_car where name=#{}
  ~~~

​	${}：先进⾏sql语句拼接，然后再编译sql语句，底层是Statement实现。存在sql注⼊现象。只有在需要进⾏sql语句关键字拼接的情况下才会⽤到。

- ~~~sql
  select * from t_car where name='${}'
  ~~~



什么情况下必须使⽤${}

> 当需要进⾏sql语句关键字拼接的时候。必须使⽤${}
>
> 1.排序
>
> > 需求：通过向sql语句中注⼊asc或desc关键字，来完成数据的升序或降序排列。
> >
> > 语句：
> >
> > ~~~sql
> > select * from t_car order by car_Num ${key} 
> > ~~~
> >
> > 如果使用 " #{}" ，那么给sql传递参数后，最后的sql语句就是这样的：
> >
> > ~~~sql
> > select * from t_car order by car_Num 'desc' //错误的
> > ~~~
>
> 2.拼接表名
>
> > ​		业务背景：实际开发中，有的表数据量⾮常庞⼤，可能会采⽤分表⽅式进⾏存储，⽐如每天⽣成⼀张表，表的名字与⽇期挂钩，例如：2022年8⽉1⽇⽣成的表：t_user20220108。2000年1⽉1⽇⽣成的表：t_user20000101。此时前端在进⾏查询的时候会提交⼀个具体的⽇期，⽐如前端提交的⽇期为：2000年1⽉1⽇，那么后端就会根据这个⽇期动态拼接表名为：t_user20000101。有了这个表名之后，将表名拼接到sql语句当中，返回查询结果。
> >
> > 语句：
> >
> > ~~~sql
> > select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as prod
> > uceTime,car_type as carType from ${tableName}
> > ~~~
>
> 3.批量删除
>
> > 需求：一次删除多条数据，例如前端页面选择多个列表，然后删除
> >
> > 对应的sql语句：
> >
> > ~~~sql
> > delete from t_user where id = 1 or id = 2 or id = 3;
> > //或者
> > delete from t_user where id in(1, 2, 3);
> > ~~~
> >
> > 动态语句：
> >
> > ~~~sql
> > delete from t_user where id in(${ids});
> > ~~~
> >
> > 对应的java程序
> >
> > ~~~java
> > int i = mapper.deleteCar("1,2,3");
> > ~~~
>
> 4.模糊查询
>
> > 需求：查询奔驰系列的汽⻋。【只要品牌brand中含有奔驰两个字的都查询出来。】
> >
> > (1)使用"#{ }"
> >
> > > 第一种：concat函数
> > >
> > > ~~~sql
> > > select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType
> > >  from
> > >  t_car
> > >  where
> > >  brand like concat('%',#{brand},'%')
> > > 
> > > ~~~
> > >
> > > 第二种：双引号方式
> > >
> > > ~~~sql
> > > select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType
> > >  from
> > >  t_car
> > >  where
> > >  brand like "%"#{brand}"%"
> > > ~~~
> > >
> > > 
> >
> > (2)使用"${ }"
> >
> > ~~~sql
> > select id,car_num as carNum,brand,guide_price as guidePrice,produce_time as produceTime,car_type as carType
> >  from
> >  t_car
> >  where
> >  brand like '%${brand}%'
> > ~~~
> >
> > 

### 9.2  typeAliases

~~~sql
    <select id="selectAllCar" resultType="com.fengshun.CRUD.pojo.Car">
        select * from t_car
    </select>
~~~

resultType属性⽤来指定查询结果集的封装类型，这个名字太⻓，可以起别名吗？可以。

在mybatis-config.xml⽂件中使⽤typeAliases标签来起别名，包括两种⽅式：

> 第一种：typeAlias
>
> > ~~~xml
> >     <typeAliases>
> >         <typeAlias type="com.fengshun.CRUD.pojo.Car" alias="Car"/>
> >         <typeAlias type="com.fengshun.CRUD.pojo.Account" alias="Account"/>
> >     </typeAliases>
> > ~~~
> >
> > type：指定给哪个类型起别名
> >
> > alias：指定别名
> >
> > - alias属性不是必须的，如果缺省的话，type属性指定的类型名的简类名作为别名。
> > - alias是⼤⼩写不敏感的。也就是说假设alias="Car"，再⽤的时候，可以CAR，也可以car，也可以Car，都⾏。
>
> 第二种：package
>
> > 如果⼀个包下的类太多，每个类都要起别名，会导致typeAlias标签配置较多，所以mybatis⽤提供package的配置⽅式，只需要指定包名，该包下的所有类都⾃动起别名，别名就是简类名。<u>**并且别名不区分⼤⼩写。**</u>
> >
> > ~~~xml
> >     <typeAliases>
> >         <package name="com.fengshun.CRUD.pojo"/>
> >     </typeAliases>
> > ~~~

注意标签位置：![image-20230821190135931](.\图片\image-20230821190135931.png)

使用：

~~~xml
    <select id="selectAllCar" resultType="Car">
        select * from t_car
    </select>
~~~

注意：namespace 不能使用别名，必须写全限定接口名称，带包名。

### 9.3 mappers

mapper 标签的属性可以有三个：

> - resource：这种方式是从类的根路径下开始查找资源。采用这种方式的话，配置文件需要放resources⽬录下或其⼦⽬录下。
>
> - url：这种方式是一种绝对路径的方式，这种方式不要求配置文件放在类路径当中。
>
> - class：这个位置提供的是mapper接口的全限定接口名，必须带有包名。
>
>   ​	如果使⽤这种⽅式必须满⾜以下条件：
>
>   ​			（1）SQL映射⽂件和mapper接⼝放在同⼀个⽬录下。
>
>   ​			（2）SQL映射⽂件的名字也必须和mapper接⼝名⼀致。

~~~xml
 <mappers>
        <mapper resource="CarMapper.xml"/>
        <mapper url=".\MyBatisIdeaProject\myBatis\mybatis_004_crud2\src\main\resources\CarMapper.xml"/>
        <mapper class="com.fengshun.CRUD.dao.CarDao"></mapper>
    </mappers>
~~~

### 9.4 插入数据时获取自动生成的主键

在向数据库表中插入一条数据，插入之后，自动生成主键，而这个主键需要在其他地方使用时，可以利用MyBatis获取。

sql映射文件：

~~~xml
    <insert id="insertCarUsegeneratedKey" useGeneratedKeys="true" keyProperty="id">
        insert into t_car(id,car_num,brand,guide_price,produce_time,car_type)
        value(null,#{car_num},#{brand},#{guide_price},#{produce_time},#{car_type})
    </insert>
~~~

属性：

> useGeneratedKeys="true"  表示：使用自动生成的主键值
>
> keyProperty="id"  表示：指定主键值赋值给对象的哪个属性，这个就表示将主键值赋值给id属性。

使用：

~~~java
    public void TestInsertCarUsegeneratedKey() {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarDao mapper = sqlSession.getMapper(CarDao.class);
        Car car = new Car(null, "330", "大众", 12.30, "2021-05-30", "燃油车");
        Long aLong = mapper.insertCarUsegeneratedKey(car);  //插入一条数据，返回为 1
        sqlSession.commit();
        System.out.println(car.getId()); //通过getter方法获取
    }
~~~

通过对象给数据库添加参数后，想要获取主键的值，也需要通过对象来获取。

## 十、MyBatis 参数处理

### 10.1 单个简单类型参数

简单类型包括：

> - byte short int long float double char
> - Byte Short Integer Long Float Double Character
> - String
> - java.util.Date
> - java.sql.Date

在SQL映射文件中，可以手动指定传递给SQL参数的类型，以及在java的pojo类中属性的类型，在mysql中参数的类型。

> parameterType="java.util.Date"  ：指定传递的参数的类型
>
> javaType=Date  ： 指定pojo类中属性的类型
>
> jdbcType=DATE  ：指定mysql中属性的类型

实例：

~~~XML
 <select id="selectByBrith" resultType="Student" parameterType="java.util.Date">
        select * from t_student where brith=#{brith,javaType=Date,jdbcType=DATE}
   </select>
~~~

对于简单类型参数来讲，mybatis能够自动识别，不需要我们再添加。

测试程序：

~~~java
    public  void testsSelectByBrith() throws ParseException {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        Date parse = new SimpleDateFormat("yyyy-MM-dd").parse("1980-10-11");
        List<Student> students = mapper.selectByBrith(parse);
        sqlSession.commit();
        sqlSession.close();
        System.out.println(students);
    }
~~~

### 10.2 Map 参数

当函数的参数为map对象时

sql语句：

~~~xml
    <select id="selectByMap" resultType="Student">
        select * from t_student where id=#{mapId} and name=#{mapName}
    </select>
~~~

执行代码：

~~~java
    public void testSselectByMapelectByMap(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        Map<String, Object> map = new HashMap<>();
        map.put("mapId",1L);
        map.put("mapName","张三");
        List<Student> students = mapper.selectByMap(map);
        sqlSession.commit();
        sqlSession.close();
        System.out.println(students);
    }
~~~

### 10.3 实体类参数

实体类参数，指的是方法的参数是一个实体类

抽象方法：

~~~java
    int insertStudent(Student student);
~~~

sql语句：

~~~xml
    <insert id="insertStudent" parameterType="com.fengshun.param.pojo.Student">
        insert into t_student value(#{id},#{age},#{name},#{height},#{brith},#{sex})
    </insert>
</mapper>
~~~

执行程序：

~~~java
    public void testInsertStudent() throws Exception{
        SqlSession sqlSession = SqlSessionUtil.openSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
        Date date = simpleDateFormat.parse("2006-05-18");
        Student student = new Student(3L,15,"王五",1.74,date,'女');
        int i = mapper.insertStudent(student);
        sqlSession.commit();
        sqlSession.close();
        System.out.println(i);
    }
~~~

### 10.4 多参数

如果函数是多个参数，mybatis框架会自动创建一个map集合并且map集合是以下形式存储参数的：

​			map.put(" arg0 ", id)

​			map.put(" arg1 ",  name)

抽象方法：

~~~java
    Student selectByNameAndId(Long id,String name);
~~~



sql 语句：

~~~xml
    <select id="selectByNameAndId" resultType="Student">
        select * from t_student where id=#{arg0} and name=#{arg1}
    </select>
~~~

测试程序：

~~~java
    public void testSelectByNameAndId(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        Student student = mapper.selectByNameAndId(1L, "张三");
        sqlSession.commit();
        sqlSession.close();
        System.out.println(student);
    }
~~~

注意：低版本的mybatis中使用的是#{0} #{1},高版本的mybatis使用的是 #{ arg0 } #{ arg1 } 或者#{ param0 } #{ param1 }

### 10.5 @Param 注解（命名参数）

可以不⽤arg0 arg1 param1 param2吗？这个map集合的key我们⾃定义可以吗？当然可以。使⽤@Param注解即可。这样可以增强可读性。

需求：根据id和name查询

抽象方法：

~~~java
    Student selectByIdAndName(@Param(value="id") Long id,@Param(value="name") String name);
//简写形式
    Student selectByIdAndName(@Param("id") Long id,@Param("name") String name);
~~~

sql语句：

~~~xml
    <select id="selectByIdAndName" resultType="Student">
        select * from t_student where id=#{id} and name=#{name}
    </select>
~~~

测试程序：

~~~java
    public void testSelectByIdAndName(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        Student student = mapper.selectByIdAndName(1L,"张三");
        sqlSession.commit();
        sqlSession.close();
        System.out.println(student);
    }
~~~

### 10.6 @Param 源码分析



## 十一、MyBatis查询语句专题

### 11.1 返回实体类

当查询的结果，有对应的实体类，并且查询结果条数只有一条

CarMapper 接口：

~~~java
public interface CarMapper {
    Car selectAll(Long id);
}
~~~

sql语句：

~~~xml
    <select id="selectAll" resultType="car">
        select * from t_car where id=#{id}
    </select>
~~~

测试程序：

~~~java
    public void testSelectAll(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Car car = mapper.selectAll(1L);
        sqlSession.commit();
        sqlSession.close();

    }
}
~~~



### 11.2返回List

当查询的记录条数是多条的时候，必须使⽤集合接收。如果使⽤单个实体类接收会出现异常。

CarMapper 接口：

~~~java
public interface CarMapper {
    List<Car> selectAll();
}
~~~

sql语句：

~~~xml
    <select id="selectAll" resultType="car">
        select * from t_car
    </select>
~~~

测试程序：

~~~java
    @Test
    public void testSelectAll2(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        List<Car> list = mapper.selectAll();
        sqlSession.commit();
        sqlSession.close();
        System.out.println(list);
    }
~~~



### 11..3 返回Map

当返回的数据，没有合适的实体类对应的话，可以采⽤Map集合接收。字段名做key，字段值做value。

***查询如果可以保证只有⼀条数据，则返回⼀个Map集合即可。***

CarMapper 接口：

~~~java
public interface CarMapper {
    Map<String,Object> selectRetMap(Long id);
}
~~~

sql语句：

~~~xml
    <select id="selectRetMap" resultType="map">
        select * from t_car where id=#{id}
    </select>
~~~

测试程序：

~~~java
    public void testSelectRetMap(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Map<String, Object> stringObjectMap = mapper.selectRetMap(1L);
        sqlSession.commit();
        sqlSession.close();
        System.out.println(stringObjectMap);
    }
~~~

![image-20230823112053722](.\图片\image-20230823112053722.png)

### 11.4 返回List< Map>

查询结果条数⼤于等于1条数据，则可以返回⼀个存储Map集合的List集合。List<Map>等同于List<Car>

![image-20230823112619857](.\图片\image-20230823112619857.png)

CarMapper 接口：

~~~java
public interface CarMapper {
    List<Map<String,Object>> selectRetListAndMap();
}
~~~

sql语句：

~~~xml
    <select id="selectRetListAndMap" resultType="map">
        select * from t_car
    </select>
~~~

测试程序：

~~~java
    @Test
    public void testSelectRetListAndMap() {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        List<Map<String, Object>> maps = mapper.selectRetListAndMap();
        sqlSession.commit();
        sqlSession.close();
        maps.forEach(map -> System.out.println(map));
    }
~~~



![image-20230823112449467](.\图片\image-20230823112449467.png)

### 11.5 返回Map<String,Map>

拿Car的id做key，以后取出对应的Map集合时更⽅便

![image-20230823133202078](.\图片\image-20230823133202078.png)

使用注解：@MapKey( " value " )

将来会将value的值作为外层map的key，value可以是每条记录的某个值

CarMapper 接口：

~~~java
public interface CarMapper {
    @MapKey("id")  //将查询结果的id最为整个大Map的key
    Map<Long,Map<String,Object>> selectRetMapByMap();
}
~~~

sql 语句：

~~~xml
    <select id="selectRetMapByMap" resultType="map">
        select * from t_car
    </select>
~~~

测试代码：
~~~java
    @Test
    public void testSelectRetMapByMap(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Map<Long, Map<String, Object>> longMapMap = mapper.selectRetMapByMap();
        sqlSession.commit();
        sqlSession.close();
        System.out.println(longMapMap);
    }
~~~

### 11.6 resultMap 结果映射

查询结果的列名和java对象的属性名对应不上怎么办？

> - 第⼀种⽅式：as 给列起别名
> - 第⼆种⽅式：使⽤ resultMap 进⾏结果映射
> - 第三种⽅式：是否开启驼峰命名⾃动映射（配置settings）

#### 使用resultMap 进行结果映射

CarMapper.xml文件：

~~~xml
<!--
 resultMap:
 id：这个结果映射的标识，作为select标签的resultMap属性的值。
 type：结果集要映射的类。可以使⽤别名。
-->
<resultMap id="carResultMap" type="car">
 <!--对象的唯⼀标识，官⽅解释是：为了提⾼mybatis的性能。建议写上。-->
    <!--如果数据库表中有主键，那么建议配上id属性-->
 	<id property="id" column="id"/>
 	<result property="carNum" column="car_num"/>
 	<!--当属性名和数据库列名⼀致时，可以省略。但建议都写上。-->
    <!--property属性：pojo类中的属性名
		column属性：数据库表中字段名-->
	<!--javaType⽤来指定属性类型。jdbcType⽤来指定列类型。⼀般可以省略。-->
 	<result property="brand" column="brand" javaType="string" jdbcType="VARCHAR"/>
 	<result property="guidePrice" column="guide_price"/>
 	<result property="produceTime" column="produce_time"/>
 	<result property="carType" column="car_type"/>
</resultMap>
<!--resultMap属性的值必须和resultMap标签中id属性值⼀致。-->
<select id="selectAllByResultMap" resultMap="carResultMap">
 		select * from t_car
</select>
~~~

#### 是否开启驼峰命名自动映射

使⽤这种⽅式的前提是：属性名遵循Java的命名规范，数据库表的列名遵循SQL的命名规范。

Java命名规范：⾸字⺟⼩写，后⾯每个单词⾸字⺟⼤写，遵循驼峰命名⽅式。

SQL命名规范：全部⼩写，单词之间采⽤下划线分割。

⽐如以下的对应关系：

![image-20230823134548645](.\图片\image-20230823134548645.png)

在mybatis-config.xml⽂件中进⾏配置

~~~xml
<!--放在properties标签后⾯-->
<settings>
	 <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
~~~



### 11.7 返回总记录条数

CarMapper接口：

~~~java
public interface CarMapper {
    Long selectTotal();
}
~~~

sql语句：

~~~xml
    <select id="selectTotal" resultType="Long">
        select count(*) from t_car
    </select>
~~~

测试代码：

~~~java
    public void testSelectTotal(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Long aLong = mapper.selectTotal();
        sqlSession.commit();
        sqlSession.close();
        System.out.println(aLong);
    }
~~~



## 十二、动态SQL	

### 12.1 if标签

需求：多条件查询。

可能的条件包括：品牌（brand）、指导价格（guide_price）、汽⻋类型（car_type）

CarMapper 接口：

~~~java
public interface CarMapper {

    List<Car> selectByMultiCondition(@Param("brand") String brand,
                                     @Param("guidePrice") Double guide_price,
                                     @Param("carType") String car_type);
}
~~~

sql语句：

~~~xml
    <select id="selectByMultiCondition" resultType="Car">
        select * from t_car where 
        <!--
        1.if标签中test属性是必须的
        2.if标签中test属性是false或者true
        3.如果test是true，则if标签中的sql语句就会拼接，反之不会
        4.test属性中可以使用的是：
            当使用了@Param注解，那么test中要出现的是@param注解指定的参数名。例如@Param(brand") 那么需要使用brand
            如果没有使用@Param 注解，那么test中要出现的是param1,param2,param3,arg0,arg1...
            当使用了POJO，那么test中出现的是POJO类的属性名
            5.在mybatis的动态sql中，不能使用&& 只能使用and
        -->
        <if test="brand !=null and brand !=''">
             brand like "%"#{brand}"%"
        </if>
        <if test="guidePrice !=null and guidePrice != ''">
           and guide_price > #{guidePrice}
        </if>
        <if test="carType != null and carType != ''">
          and  car_type  = #{carType}

        </if>
    </select>
~~~

测试程序：

~~~java
@Test
    public void testSelectByMultiCondition(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
       // 三个参数都不为空
        List<Car> cars = mapper.selectByMultiCondition("大众", 2.0, "燃油车");
        //三个参数都为空
        List<Car> cars2 = mapper.selectByMultiCondition(null, null, null);
        // 三个参数其中一个为空
        List<Car> cars3 = mapper.selectByMultiCondition(null, 2.0, "燃油车");
        //第一个参数不为空
        List<Car> cars4 = mapper.selectByMultiCondition("大众", null, "燃油车");
        sqlSession.commit();
        sqlSession.close();
    }
~~~

当三个参数都为空时，sql语句为： select  * from t_car where   	(×)

当第一个参数为空时，sql语句为：  select  * from t_car where  and guide_price >2.00 and car_type  =‘ 燃油车 '		（×）

为了避免以上错误发生，可以在where语句后面添加1=1 并且在第一个if标签中添加and

~~~xml
    <select id="selectByMultiCondition" resultType="Car">
        select * from t_car where 1=1
        <if test="brand !=null and brand !=''">
            and brand like "%"#{brand}"%"
        </if>
        <if test="guidePrice !=null and guidePrice != ''">
           and guide_price > #{guidePrice}
        </if>
        <if test="carType != null and carType != ''">
          and  car_type  = #{carType}

        </if>
    </select>
~~~

### 12.2 where标签

where标签的作⽤：让where⼦句更加动态智能。

作用：

> 所有条件都为空时，where标签保证不会⽣成where⼦句。
>
> ⾃动去除某些条件**前⾯**多余的and或or。

CarMapper 接口：

~~~java
 List<Car> selectByMultiConditionWithWhere(@Param("brand") String brand,
                                     @Param("guidePrice") Double guide_price,
                                     @Param("carType") String car_type);
~~~

sql语句：

~~~xml
<select id="selectByMultiConditionWithWhere" resultType="Car">
        select * from t_car
<!--        where标签是专门负责where子句动态生成的-->
        <where>
            <if test="brand !=null and brand !=''">
                 brand like "%"#{brand}"%"
            </if>
            <if test="guidePrice !=null and guidePrice != ''">
                and guide_price > #{guidePrice}
            </if>
            <if test="carType != null and carType != ''">
                and  car_type  = #{carType}

            </if>
        </where>

    </select>
~~~

测试代码：

~~~java
 @Test
    public void testSelectByMultiConditionWithWhere(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        // 三个参数都不为空
         List<Car> cars = mapper.selectByMultiConditionWithWhere("大众", 2.0, "燃油车");
        //三个参数都为空
        List<Car> cars2 = mapper.selectByMultiConditionWithWhere(null, null, null);
        // 三个参数其中一个为空
        List<Car> cars3 = mapper.selectByMultiConditionWithWhere(null, 2.0, "燃油车");
        //第一个参数不为空
         List<Car> cars4 = mapper.selectByMultiConditionWithWhere("大众", null, "燃油车");
        sqlSession.close();
    }
~~~

### 12.3 trim 标签

trim标签的属性：

> prefix：在trim标签中的语句前添加内容
>
> suffix：在trim标签中的语句后添加内容
>
> prefixOverrides：前缀去掉（覆盖掉）
>
> suffixOverrides：后缀去掉（覆盖掉）

~~~xml
 <trim prefix="" prefixOverrides="" suffix="" suffixOverrides=""></trim>
~~~

sql语句：

~~~~xml
<select id="selectByMultiConditionWithtrim" resultType="Car">
        select * from t_car
        <!--
        prefix="where" 表示在trim标签中的语句前面添加where
        suffixOverrides="and|or" 表示去除掉trim标签中的语句的末尾的and或者or
        -->
        <trim prefix="where"  suffixOverrides="and|or">
            <if test="brand !=null and brand !=''">
                brand like "%"#{brand}"%" and
            </if>
            <if test="guidePrice !=null and guidePrice != ''">
                 guide_price > #{guidePrice} and
            </if>
            <if test="carType != null and carType != ''">
                car_type  = #{carType} and
            </if>
        </trim>
    </select>
~~~~

测试代码：

~~~java
@Test
    public void testSelectByMultiConditionWithtrim(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        //三个参数都为空
        List<Car> cars2 = mapper.selectByMultiConditionWithWhere(null, null, null);
        //第一个参数不为空,后面两个参数都为空
        List<Car> cars4 = mapper.selectByMultiConditionWithWhere("大众", null, null);
        sqlSession.close();
    }
~~~

当trim标签中没有语句时，trim标签不会自动为我们添加where子句

### 12.4 set 标签

主要使⽤在update语句当中，⽤来⽣成set关键字，同时去掉最后多余的 “ , “

⽐如我们只更新提交的不为空的字段，如果提交的数据是空或者""，那么这个字段我们将不更新。

sql语句：
~~~xml
    <update id="updateById">
        update t_car
        <set>
            <if test="car_num != null and car_num != ''">car_num = #{car_num},</if>
            <if test="brand != null and brand != ''">brand = #{brand},</if>
            <if test="guide_price != null and guide_price != ''">guide_price = #{guide_price},</if>
            <if test="produce_time != null and produce_time != ''">produce_time = #{produce_time},</if>
            <if test="car_type != null and car_type != ''">car_type = #{car_type},</if>
        </set>
        where id=#{id}
    </update>
~~~

测试程序;

~~~java
    public void testUpdateByid(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        // Car car = new Car(1L,"200","比亚迪",12.50,"2002-12-10","电车");
        Car car = new Car(1L,null,null,18.50,null,null);
        int i = mapper.updateById(car);
        sqlSession.commit();
        sqlSession.close();

    }
~~~

### 12..5 choose when otherwise

语法格式：

~~~xml
<choose>
 	<when></when>
 	<when></when>
 	<when></when>
 	<otherwise></otherwise>
</choose>
~~~

等同于：

~~~java
if(){
    
}else if(){
    
}else if(){
    
}else if(){
    
}else{
    
}
~~~

表示只有一个分支会被选择

需求：根据brand或者guide_price查询，如果都没有，则根据car_type 查询

sql语句：

~~~xml
<select id="selectWithChoose" resultType="Car">
        select * from t_car
        <where>
            <choose>
                <when test="brand !=null and brand !=''">
                    and brand like "%"#{brand}"%"
                </when>
                <when test="guidePrice !=null and guidePrice != ''">
                    and guide_price > #{guidePrice}
                </when>
                <otherwise>
                    and car_type = #{carType}
                </otherwise>

            </choose>
        </where>
    </select>
~~~

测试代码：

~~~java
    @Test
    public void testSelectWithChoose() {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        List<Car> cars = mapper.selectWithChoose(null, 10.00, null);
        List<Car> cars1 = mapper.selectWithChoose("众", null, null);
        List<Car> cars2 = mapper.selectWithChoose(null, null, "电车");
        sqlSession.commit();
        sqlSession.close();
    }
~~~

### 12.6 foreach 标签

循环数组或集合，动态⽣成sql，⽐如这样的SQL：

> 批量删除：
>
> ~~~sql
> delete from t_car where id in(1,2,3);
> delete from t_car where id = 1 or id = 2 or id = 3;
> ~~~
>
> 批量添加：
>
> ~~~sql
> insert into t_car values
>  (null,'1001','凯美瑞',35.0,'2010-10-11','燃油⻋'),
>  (null,'1002','⽐亚迪唐',31.0,'2020-11-11','新能源'),
>  (null,'1003','⽐亚迪宋',32.0,'2020-10-11','新能源')
> ~~~

foreach 标签中有六个属性：

> - collection：指定集合或数组
> - item：集合或数组中的元素
> - separator：循环之间的分隔符
> - open：foreach循环拼接的所有sql语句中最前面以什么开始
> - close：foreach循环拼接的所有sql语句中最前面以什么结束
> - index：每条数据的索引

语法：

~~~xml
        <foreach collection="" item="" separator="" open="" close=""></foreach>
~~~



#### 批量删除

sql语句：

~~~xml
    <delete id="deletByIds">
        delete from t_car where id in
        <foreach collection="ids" item="id" separator="," open="(" close=")">
            #{id}
        </foreach>
    </delete>
~~~

测试代码：

~~~java
    @Test
    public void testDeletByIds(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Long[] ids = {10L,11L,12L,13L};
        int i = mapper.deletByIds(ids);
        sqlSession.commit();
        sqlSession.close();

    }
~~~

分隔符也能用or：

> ~~~xml
>     <delete id="deletByIds">
>         delete from t_car where
>         <foreach collection="ids" item="id" separator=" or ">
>             id=#{id}
>         </foreach>
>     </delete>
> ~~~



#### 批量添加

sql语句：

~~~xml
    <insert id="insert" parameterType="list">
        insert into t_car value
        <foreach collection="arg0" item="car" separator=",">
            (null,#{car.car_num},#{car.brand},#{car.guide_price},#{car.produce_time},#{car.car_type})
        </foreach>
    </insert>
~~~

测试语句：

~~~java
@Test
    public void testInsert(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        CarMapper mapper = sqlSession.getMapper(CarMapper.class);
        Car car0  = new Car(null,"501","奔驰",40.25,"2022-11-15","燃油车");
        Car car1  = new Car(null,"402","宝马",45.21,"2021-01-12","燃油车");
        Car car2  = new Car(null,"201","大众",30.25,"2023-05-25","电车");
        Car car3  = new Car(null,"324","比亚迪",5.21,"1999-11-23","油电混动");
        Car[] arrayCar = {car0,car1,car2,car3};
        List<Car> cars = Arrays.stream(arrayCar).toList();
        int insert = mapper.insert(cars);
        sqlSession.commit();
        sqlSession.close();
    }
~~~

### 12.7 sql标签与include标签

sql标签⽤来声明sql⽚段

include标签⽤来将声明的sql⽚段包含到某个sql语句当中

作⽤：代码复⽤。易维护。

~~~xml
    <sql id="carColumnNameSql">
        id,car_num carNum,brand,guide_price,guidePrice,produce_time,produceTime,car_type,carType
    </sql>
    <select id="selectBySql">
        select <include refid="carColumnNameSql"/> from t_car
    </select>
~~~

## 十三、MyBatis的高级映射及延迟加载

怎么区分主表和副标：

> 原则：谁在前谁就是主表
>
> 多对一：多在前，那么就是主表
>
> 一对多：一在前，那么一就是主表

### 13.1 多对一

在多对一中，多的一方为student，t_student为主表，t_clazz为副表。在VM当中的主对象为 Student 对象。

第一种方式：级联属性映射

pojo类Student中添加⼀个属性：Clazz clazz; 表示学⽣关联的班级对象。

通过 左连接的方式进行查询



pojo 实体类代码：

~~~java
public class Student {
    private Long sid;
    private String sname;
    private Clazz clazz;

    public Student(Long sid, String sname ) {
        this.sid = sid;
        this.sname = sname;
    }
    //getter and setter
    //toString
    }
~~~

SQL 映射文件中指定映射：

~~~xml
    <resultMap id="studentResultMap" type="Student">
        <id property="sid" column="sid"></id>
        <result property="sname" column="sname"></result>
        <result property="clazz.cid" column="cid"/>
        <result property="clazz.cname" column="cname"/>
    </resultMap>
~~~

sql语句：
~~~xml
    <select id="selectById" resultMap="studentResultMap">
        select s.sid,s.sname,c.cid,c.cname
        from t_stu s left join t_clazz c on s.cid=c.cid where s.sid=#{sid}
    </select>
~~~

测试程序：

~~~java
    public void testSelectById(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        Student student = mapper.selectById(1L);
        System.out.println(student);
        sqlSession.close();
    }
~~~

![image-20230823235703886](.\图片\image-20230823235703886.png)

##### 第二种方式：association

association 翻译为关联，在resultMap标签中使用该标签，可以到多对一映射。

```
它有两个属性：
        1. property 提供要映射的pojo类的属性名
        2.javaType：用来指定要映射的java类型
```

可以把 association 标签看作resultMap 来处理，只是父级标签的名字不同，其余都相同，在 association  标签中最好也有一个id标签

resultMap 标签

~~~xml
    <resultMap id="studentResultMapWhitAssociation" type="Student">
        <id property="sid" column="sid"/>
        <result property="sname" column="sname"/>
        <!--Association翻译为关联，  一个Student对象关联一个clazz对象
            它有两个属性：
                    1. property 提供要映射的pojo类的属性名
                    2.javaType：用来指定要映射的java类型
        -->
        <association property="clazz" javaType="com.fengshun.advanced.pojo.Clazz">
            <id property="cid" column="cid"/>
            <result property="cname" column="cname"/>
        </association>
    </resultMap>
~~~

sql语句：

~~~xml
    <select id="selectByIdWhitAssociation" resultMap="studentResultMapWhitAssociation">
        select s.sid,s.sname,c.cid,c.cname
        from t_stu s left join t_clazz  c on s.cid=c.cid
        where s.sid=#{sid}
    </select>
~~~

测试程序：

~~~java
    @Test
    public void testSelectByIdWhitAssociation(){
        SqlSession sqlSession = SqlSessionUtil.openSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        Student student = mapper.selectByIdWhitAssociation(1L);
        sqlSession.close();
        System.out.println(student);
    }
~~~



##### 第三种方式：分步查询

这种方式·需要有两条sql语句，进行分布查询，这种方式常用。

优点：可复用、支持懒加载（延迟加载）

> 1.复用性增强，可以重复利用，（大步拆成多个小碎步，每个小碎步更加可以重复利用
>
> 2.采用这种分布查询，可以充分利用他们的延迟加载、懒加载机制。

思路：

> 先查询出主表的全部信息，通过主表的副键，查询出副表相关联的所有信息。
>
> 需要准备主表的sql语句，和副表的sql语句。
>
> 在主表的sql映射文件中，也需要进行ResultMap配置，并且通过 association 标签为副表传递参数。
>
> association 标签的其他两个属性：
>
> > select 属性：这个属性中需要关联表的Sql语句的id，并且需要 namespace + sqlid
> >
> > column 属性：这个属性用来指定，从主表中查询出的副键的值，并将副键的值传递给副表的sql语句

第一步：根据学生id查询出学生的所有信息

> 接口方法：
>
> ~~~java
>     // 分布查询：第一步，先根据学生的sid查询学生的信息
>     Student selectByIdStep1(Long sid);
> ~~~
>
> Student.Mapper 映射文件（主表）：
>
> ```xml
> <resultMap id="studentResultMapByStep" type="Student">
>     <id property="sid" column="sid"/>
>     <result property="sname" column="sname"/>
>     <!--select 属性需要指定另一个sql语句的id-->
>     <association property="clazz" select="com.fengshun.advanced.Mapper.ClazzMapper.selectById" column="cid"/>
> </resultMap>
> 
> <select id="selectByIdStep1" resultMap="studentResultMapByStep">
>     select s.* from t_stu s where sid=#{sid}
> </select>
> ```

第二步：根据学生信息中的班级id，查询出所有班级信息

> 接口方法：
>
> ~~~java
>     // 分布查询第二步：根据cid查询班级信息
>     Clazz selectById(Long cid);
> ~~~
>
> sql映射文件：
>
> ```xml
> <select id="selectById" resultType="clazz">
>     select * from t_clazz where cid=#{cid}
> </select>
> ```

测试程序：

```java
public void testStudentResultMapByStep(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    Student student = mapper.selectByIdStep1(1L);
    sqlSession.close();
    System.out.println(student);
}
```

![image-20230824104308748](C:\Users\fengshun\AppData\Roaming\Typora\typora-user-images\image-20230824104308748.png)



### 13.2 多对一延迟加载

延迟加载（懒加载）：

> 延迟加载的核心原理：用的时候再执行查询语句，不用的时候不查询。
>
> 作用： 提高性能。尽可能不查，或者少查

开启延迟加载：

> **局部设置**：只对当前的 association  关联的sql语句起作用
>
> > 在 association 标签中添加 fetchType=“lazy” 属性
>
> **全局设置**：
>
> > 当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。
> >
> > 在mybatis的核心配置文件中设置：
> >
> > ~~~xml
> >     <settings>
> >         <!--延迟加载的全局开关，默认值为false表示不开启-->
> >         <!--表示所有带有分布查询的语句都要延迟加载-->
> >         <setting name="lazyLoadingEnabled" value="true"/>
> >     </settings>
> > ~~~
>
> 如果开启了全局延迟加载，但是针对某些程序并不需要延迟加载，<u>可以在 association   标签中添加 fetchType=“eager” 属性</u>
>
> **实际开发模式：把全局延迟加载打开，如果某个不需要延迟加载，添加 fetchType=“eager” 属性即可**。

在以上的基础上给 association 标签中添加 fetchType=“lazy” 属性：

```xml
<resultMap id="studentResultMapByStep" type="Student">
    <id property="sid" column="sid"/>
    <result property="sname" column="sname"/>
    <!--select 属性需要指定另一个sql语句的id-->
    <association property="clazz"
                 select="com.fengshun.advanced.Mapper.ClazzMapper.selectById"
                 column="cid"
                 fetchType="lazy"
    />
</resultMap>
```

注意：只有当使用到副表中的信息时才会执行两条sql语句

测试程序：

```java
public void testStudentResultMapByStep(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    Student student = mapper.selectByIdStep1(1L);
    sqlSession.close();
    // 需要调用到getClazz() 方法，就会执行两条sql语句
    System.out.println(student);
    // 一下两个输出语句都没有涉及到clazz中的属性，所以只执行一条sql语句
    System.out.println(student.getSid());
    System.out.println(student.getSname());
}
```



### 13.3 一对多

一对多的实现：一对多，一在前，一为主表，多为副表。

例如：一个班级对应多个学生，通过班级id会查询出多个学生，在java程序中使用 集合或者数组 存储班级中的学生信息。

#### 第一种方式：collection

collection 标签涉及到两个属性：

> ofTypey 属性：用来指定集合当中的元素类型
>
> property 属性： 提供要映射的pojo类的属性名

pojo类：Clazz类的属性：

```java
public class Clazz {
    private Long cid;
    private String cname;
    private List<Student> students;
    //构造方法
    //getter 和  setter
    //toString
}
```

ClazzMapper 接口中的抽象方法：

```java
// 根据班级编号查询班级信息
Clazz selectByCollection(Long cid);
```

ClazzMapper.xml Sql映射文件：

```xml
   <resultMap id="clazzResltMap" type="Clazz" >
       <id property="cid" column="cid"/>
       <result property="cname" column="cname"/>
<!--       一对多，这里是collection，collection是集合的意思-->
<!--       ofTypey 用来指定集合当中的元素类型-->
       <collection property="students" ofType="Student">
           <id property="sid" column="sid"/>
           <result property="sname" column="sname"/>
       </collection>
   </resultMap>

    <select id="selectByCollection" resultMap="clazzResltMap">
        select c.cid,c.cname,s.sid,s.sname
        from t_clazz c left join t_stu s on c.cid=s.cid
        where c.cid=#{cid}
    </select>
```

编写测试程序：

```java
public void testSelectByCollection(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    ClazzMapper mapper = sqlSession.getMapper(ClazzMapper.class);
    Clazz clazz = mapper.selectByCollection(1001L);
    sqlSession.close();
    System.out.println(clazz);
}
```

运行结果截图：

![image-20230824113836667](.\图片\image-20230824113836667.png)

#### 第二种方式：分步查询

运用collection中的其他两个属性实现：

> select 属性：这个属性中需要关联表的Sql语句的id，并且需要 namespace + sqlid
>
> column 属性：这个属性用来指定，从主表中查询出的副键的值，并将副键的值传递给副表的sql语句

第一步：根据班级id查询出班级信息

> 接口中的抽象方法：
>
> ```java
> // 分布查询的第一步
> Clazz selectByStep1(Long cid);
> ```
>
> ResultMap 映射
>
> ```xml
> <resultMap id="clazzResultStep1" type="Clazz">
>     <id property="cid" column="cid"/>
>     <result property="cname" column="cname"/>
>     <collection property="students" select="com.fengshun.advanced.Mapper.StudentMapper.selectByCid" column="cid"/>
> </resultMap>
> ```
>
> sql语句：
>
> ```xml
> <select id="selectByStep1" resultMap="clazzResultStep1">
>     select c.cid,c.cname from t_clazz c where c.cid=#{cid}
> </select>
> ```

第二步：根据班级id查询出所有的学生信息

> 接口中的抽象方法：
>
> ```java
> List<Student> selectByCid(Long cid);
> ```
>
> Sql语句
>
> ```xml
> <select id="selectByCid" resultType="Student">
>     select sid,sname from t_stu where cid=#{cid}
> </select>
> ```

### 13.3 一对多延迟加载

⼀对多延迟加载机制和多对⼀是⼀样的。同样是通过两种⽅式：

> 第⼀种：fetchType="lazy"
>
> 第⼆种：修改全局的配置setting，lazyLoadingEnabled=true，如果开启全局延迟加载，想让某个sql不使⽤延迟加载：fetchType="eager



## 十四、MyBatis的缓存

**缓存的英语单词**：cache

**缓存的定义**：提前把数据放到缓存中（内存中），下一次用的时候，直接从缓存中拿，效率高。



在计算机中有两个存储空间：

> 1.内存：临时存储数据的空间，断电后会消失
>
> 2.硬盘：持久化的数据再硬盘当中，文件也是存储在硬盘中。当计算机重启后，硬盘中的文件不会丢失。它是一个持久化设备。

实际上各大关系型的数据库的数据，都是存储在文件当中的，也就是存储在硬盘中。

通过java程序对数据库查询出记录后，会将这些记录封装成java对象



**MyBatis的缓存机制：**

>执行DQL（select语句） 的时候，将查询结构放到缓存当中（内存中）如果下一次还是执行完全相同的DQL语句，直接从缓存中拿数据，不再查数据库了，不再去硬盘上找数据。

这种缓存机制只针对DQL语句，如果对数据库当中的数据进行了修改（增、删、改），那么MyBatis会清空缓存中的数据。

**缓存机制的目的**：使用缓存机制，能够减少IO的方式来提高效率。

缓存通常是我们程序开发中优化程序的重要手段，接触的缓存技术：（1）字符串常量池 （2）整数型常量池 （3）线程池 （4） 连接池 

------

mybatis缓存包括：

> - ⼀级缓存：将查询到的数据存储到SqlSession中。
> - ⼆级缓存：将查询到的数据存储到SqlSessionFactory中。
> - 或者集成其它第三⽅的缓存：⽐如EhCache【Java语⾔开发的】、Memcache【C语⾔开发的】等。



### 14.1 一级缓存

一级缓存默认是开启的。不需要做任何的配置。

原理：只要使用同一个SqlSession对象，执行同一条SQL语句，就会走缓存

测试代码：

```java
public void testSelectAll() {
    SqlSession sqlSession = SqlSessionUtil.openSession();
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    List<Student> students1 = mapper.selectAll();
    students1.forEach(student -> System.out.println(student));
    List<Student> students2 = mapper.selectAll();
    students2.forEach(student -> System.out.println(student));
    sqlSession.close();
}
```

只执行了一次sql语句

![image-20230824152846876](.\图片\image-20230824152846876.png)

什么时候不走缓存：

> SqlSession对象不是同一个
>
> 查询条件不一样，肯定不走缓存

什么时候一级缓存失效？

> 第一次DQL和第二次DQL之间做了以下两件事中的任意一个，就会让一级缓存失效：
> 	1.执行了SqlSession的clearCache（）方法，这是手动清空缓存
>
> ​	2.执行了INSERT或UPDATE或DELETE语句， 不管是操作哪张表，都会清空一级缓存。

### 14.2 二级缓存

二级缓存的范围：SqlSessionFactory

使用二级缓存需要具备以下几个条件：

> 1. <setting name="cacheEnabled" value="true"> 全局性地开启或关闭所有映射器配置⽂件中已配置的任何缓存。默认就是true，⽆需设置。
> 2. 在需要使⽤⼆级缓存的SqlMapper.xml⽂件中添加配置：<cache />
> 3. 使⽤⼆级缓存的实体类对象必须是可序列化的，也就是必须实现java.io.Serializable接⼝
> 4. SqlSession对象关闭或提交之后，⼀级缓存中的数据才会被写⼊到⼆级缓存当中。此时⼆级缓存才可⽤。
>
> 默认情况下，二级缓存是开启的。

测试程序：

```java
    @Test
    public void testselectOne() throws Exception{
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsReader("mybatis-config.xml"));
        SqlSession sqlSession1 = sqlSessionFactory.openSession();
        StudentMapper mapper1 = sqlSession1.getMapper(StudentMapper.class);
        Student student = mapper1.selectOne(1L);
        sqlSession1.close();

        SqlSession sqlSession2 = sqlSessionFactory.openSession();
        StudentMapper mapper2 = sqlSession2.getMapper(StudentMapper.class);
        Student student1 = mapper2.selectOne(1L);
        sqlSession2.close();
    }
}
```

![image-20230824160229046](.\图片\image-20230824160229046.png)二级缓存失效：只要两次 查询之间出现了增删改操作，二级缓存就会失效。【一级缓存也会失效】

二级缓存的相关配置：

![image-20230824160614486](.\图片\image-20230824160614486.png)

1. eviction：指定从缓存中移除某个对象的淘汰算法。默认采⽤LRU策略。

> a. LRU：Least Recently Used。最近最少使⽤。优先淘汰在间隔时间内使⽤频率最低的对象。(其实还有⼀种淘汰算法LFU，最	不常⽤。)
>
> b. FIFO：First In First Out。⼀种先进先出的数据缓存器。先进⼊⼆级缓存的对象最先被淘汰。
>
> c. SOFT：软引⽤。淘汰软引⽤指向的对象。具体算法和JVM的垃圾回收算法有关。
>
> d. WEAK：弱引⽤。淘汰弱引⽤指向的对象。具体算法和JVM的垃圾回收算法有关。

2. flushInterval：

> a. ⼆级缓存的刷新时间间隔。单位毫秒。如果没有设置。就代表不刷新缓存，只要内存⾜够⼤，⼀直会向⼆级缓存中缓存数据。除⾮执⾏了增删改。

3. readOnly：

   > a. true：多条相同的sql语句执⾏之后返回的对象是共享的同⼀个。性能好。但是多线程并发可能会存在安全问题。
   >
   > b. false：多条相同的sql语句执⾏之后返回的对象是副本，调⽤了clone⽅法。性能⼀般。但安全。

4. size：

   > a. 设置⼆级缓存中最多可存储的java对象数量。默认值1024。
   
   

### 14.3 MyBatis 集成EHCache

集成EHCache是为了代替MyBatis自带的二级缓存。一级缓存是无法替代的。

mybatis对外提供了接⼝，也可以集成第三⽅的缓存组件。⽐如EhCache、Memcache等。都可以。

EhCache是Java写的。Memcache是C语⾔写的。所以mybatis集成EhCache较为常⻅，按照以下步骤操作，就可以完成集成：

> 第⼀步：引⼊mybatis整合ehcache的依赖。
>
> ~~~xml
> <!--mybatis集成ehcache的组件-->
> <dependency>
>  	<groupId>org.mybatis.caches</groupId>
>  	<artifactId>mybatis-ehcache</artifactId>
> 	 <version>1.2.2</version>
> </dependency>
> <!--ehcache需要slf4j的⽇志组件,log4j不好使-->
> <dependency>
>  	<groupId>ch.qos.logback</groupId>
>  	<artifactId>logback-classic</artifactId>
>  	<version>1.2.11</version>
>  	<scope>test</scope>
> </dependency>
> ~~~
>
> 第⼆步：在类的根路径下新建echcache.xml⽂件，并提供以下配置信息。
>
> ~~~xml
> <?xml version="1.0" encoding="UTF-8"?>
> <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>  xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
>  updateCheck="false">
>  <!--磁盘存储:将缓存中暂时不使⽤的对象,转移到硬盘,类似于Windows系统的虚拟内存-->
>  	<diskStore path="e:/ehcache"/>
>  
>  <!--defaultCache：默认的管理策略-->
>     
>  <!--eternal：设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断-->
>     
>  <!--maxElementsInMemory：在内存中缓存的element的最⼤数⽬-->
>     
>  <!--overflowToDisk：如果内存中数据超过内存限制，是否要缓存到磁盘上-->
>     
>  <!--diskPersistent：是否在磁盘上持久化。指重启jvm后，数据是否有效。默认为false-->
>     
>  <!--timeToIdleSeconds：对象空闲时间(单位：秒)，指对象在多⻓时间没有被访问就会失效。只对eternal为false的有效。默认值0，表示⼀直可以访问-->
>     
>  <!--timeToLiveSeconds：对象存活时间(单位：秒)，指对象从创建到失效所需要的时间。只对eternal为false的有效。默认值0，表示⼀直可以访问-->
>     
>  <!--memoryStoreEvictionPolicy：缓存的3 种清空策略-->
>     
>  <!--FIFO：first in first out (先进先出)-->
>     
>  <!--LFU：Less Frequently Used (最少使⽤).意思是⼀直以来最少被使⽤的。缓存的元素有⼀个hit 属性，hit 值最⼩的将会被清出缓存-->
>     
>  <!--LRU：Least Recently Used(最近最少使⽤). (ehcache 默认值).缓存的元素有⼀个时间戳，当缓存容量满了，⽽⼜需要腾出地⽅来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存-->
> 	<defaultCache eternal="false" maxElementsInMemory="1000" overflowToDisk="false" diskPersistent="false"
>  timeToIdleSeconds="0" timeToLiveSeconds="600" memoryStoreEvictionPolicy="LRU"/>
> </ehcache>
> ~~~
>
> 第三步：修改SqlMapper.xml⽂件中的<cache/>标签，添加type属性。
>
> ~~~xml
> <cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
> ~~~
>
> 第四步：编写测试程序使⽤。
>
> ~~~java
> @Test
> public void testSelectById2() throws Exception{
>  	SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
>  
>  SqlSession sqlSession1 = sqlSessionFactory.openSession();
>  CarMapper mapper1 = sqlSession1.getMapper(CarMapper.class);
>  Car car1 = mapper1.selectById(83L);
>  System.out.println(car1);
>  
>  sqlSession1.close();
>  
>  SqlSession sqlSession2 = sqlSessionFactory.openSession();
>  CarMapper mapper2 = sqlSession2.getMapper(CarMapper.class);
>  Car car2 = mapper2.selectById(83L);
>  System.out.println(car2);
> }

![image-20230824161925869](.\图片\image-20230824161925869.png)







## 十五、 MyBatis 的逆向工程

所谓的逆向工程是：根据数据库表逆向生成Java的pojo类，SqlMapper.xml文件，以及Mapper接口类等。

要完成这个工作，需要借助别人写好的逆向工程插件。

思考：使用这个插件的话，需要给这个插件配置哪些信息？

> - pojo类名、包名以及生成位置。
> - SqlMapper.xml文件名以及生成位置。
> - Mapper接口名以及生成位置。
> - 连接数据库的信息。
> - 指定哪些表参与逆向工程。
> - ......

### 15.1 逆向工程配置与生成

##### 第一步：基础环境准备

> 新建模块：mybatis-011-generator
>
> 打包方式：jar

##### 第二步：在pom中添加逆向工程插件

~~~xml
<!--定制构建过程-->
<build>
  <!--可配置多个插件-->
  <plugins>
    <!--其中的一个插件：mybatis逆向工程插件-->
    <plugin>
      <!--插件的GAV坐标-->
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-maven-plugin</artifactId>
      <version>1.4.1</version>
      <!--允许覆盖-->
      <configuration>
        <overwrite>true</overwrite>
      </configuration>
      <!--插件的依赖-->
      <dependencies>
        <!--mysql驱动依赖-->
        <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.30</version>
        </dependency>
      </dependencies>
    </plugin>
  </plugins>
</build>
~~~

##### 第三步：配置generatorConfig.xml文件

该文件名必须叫做：generatorConfig.xml

该文件必须放在类的根路径下。

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!--
        targetRuntime有两个值：
            MyBatis3Simple：生成的是基础版，只有基本的增删改查。
            MyBatis3：生成的是增强版，除了基本的增删改查之外还有复杂的增删改查。
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!--防止生成重复代码-->
        <plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin"/>
      
        <commentGenerator>
            <!--是否去掉生成日期-->
            <property name="suppressDate" value="true"/>
            <!--是否去除注释-->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <!--连接数据库信息-->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/powernode"
                        userId="root"
                        password="root">
        </jdbcConnection>

        <!-- 生成pojo包名和位置 -->
        <javaModelGenerator targetPackage="com.powernode.mybatis.pojo" targetProject="src/main/java">
            <!--是否开启子包-->
            <property name="enableSubPackages" value="true"/>
            <!--是否去除字段名的前后空白-->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- 生成SQL映射文件的包名和位置 -->
        <sqlMapGenerator targetPackage="com.powernode.mybatis.mapper" targetProject="src/main/resources">
            <!--是否开启子包-->
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

        <!-- 生成Mapper接口的包名和位置 -->
        <javaClientGenerator
                type="xmlMapper"
                targetPackage="com.powernode.mybatis.mapper"
                targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>

        <!-- 表名和对应的实体类名-->
        <table tableName="t_car" domainObjectName="Car"/>

    </context>
</generatorConfiguration>
~~~

##### 第四步：运行插件

双击运行插件：

![image-20230824172917033](.\图片\image-20230824172917033.png)

### 15.2测试逆向工程生成的是否好用

使用逆向工程生成增强版的文件目录：

![image-20230824173041974](.\图片\image-20230824173041974.png)

逆向工程只会帮助我们生成pojo类、Mapper接口、sql映射文件、日志。util工具类以及mybatis核心配置文件还是需要我们自己编写。

在POJO包中除了实体类以外，还有XxxExample 类，这个类封装了很多方法，是用来封装Sql语句的查询条件的。	

这个类采用的是QBC 风格：Query By Criteria 一种查询方式，比较面向对象，看不到sql语句。

通过StudentEXample对象封装查询条件：

> 第一步：先 new XxxExample对象
>
> ~~~java
>         StudentExample studentExample = new StudentExample();
> ~~~
>
> 第二步：利用 XxxExample 对象创建查询条件
>
> ~~~java
>         studentExample.createCriteria().andSidEqualTo(1L);
> ~~~
>
> 第三步：使用条件
>
> ```java
> List<Student> students1 = mapper.selectByExample(studentExample);
> ```

在XxxEXample 对象中封装了两个重要的方法：

> createCriteria() 方法：需要使用该方法才能添加查询条件。
>
> or() 方法：在该方法上可以继续添加查询条件，只不过与前面的查询条件使用 or 隔开。

添加条件：

> andXxxEquelTo( value ) :指定Xxx的值为什么，例如 andSidEqualTo(1L） 指定sid的值为1L
>
> andXxxBetween( value1，value2) :指定Xxx的值在value1和value2之间，就是sql语句中的 between..and..
>
> andXxxGereaterThan(value) : 指定Xxx的最大值为value
>
> andXxxLessThan(value) ： 指定Xxx的最小值为value
>
> andXxxGreaterThanOrEqualTo（value） ： 指定Xxx大于或等于value
>
> andXxxLessThanOrEqualTo（value）  ： 指定Xxx小于或等于value

多个条件可以拼接，例如：

```java
StudentExample studentExample = new StudentExample();
studentExample.createCriteria().andSidEqualTo(1001L).andCidGreaterThan(1000L);
studentExample.or().andSidEqualTo(1L);
//执行语句
List<Student> students1 = mapper.selectByExample(studentExample);
```

##### 第一步：环境准备 

- 依赖：mybatis依赖、mysql驱动依赖、junit依赖、logback依赖
- jdbc.properties
- mybatis-config.xml
- logback.xml

##### 第二步：编写测试程序

```java
@Test
public void testSelect(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    //根据主键查询
    Student student = mapper.selectByPrimaryKey(1L);
    System.out.println(student);

    // 查询所有(selectByExample 根据条件查询，如果没有条件就查询所有)
    List<Student> students = mapper.selectByExample(null);
    students.forEach(student1 -> System.out.println(student1));

    // 按照条件进行查询
    // 通过StudentEXample对象封装查询条件
    StudentExample studentExample = new StudentExample();
    // 调用studentExample.createCriteria() 创建查询条件
    studentExample.createCriteria().andSidBetween(1L,5L);
    
    studentExample.or().andCidEqualTo(1001L);

    List<Student> students1 = mapper.selectByExample(studentExample);
    students1.forEach(student1 -> System.out.println(student1));
    sqlSession.close();


}
```

![image-20230824175831731](.\图片\image-20230824175831731.png)

## 十六、MyBatis 使用 PageHelps

PageHelps 是专门为mybatis准备的分页的插件。

### 16.1 limit 分页

mysql的limit后面两个数字：

- 第一个数字：startIndex（起始下标。下标从0开始。）
- 第二个数字：pageSize（每页显示的记录条数）

假设已知页码pageNum，还有每页显示的记录条数pageSize，第一个数字可以动态的获取吗？

- startIndex = (pageNum - 1) * pageSize

所以，标准通用的mysql分页SQL：

~~~sql
select 
  * 
from 
  tableName ...... 
limit 
  (pageNum - 1) * pageSize, pageSize
~~~

实际上每一次进行分页请求发送的时候，都要发送两个数据:

> 页码pageNum要传递给服务器
>
> 每页显示的记录条数pageSize也要传给服务器

获取数据不难，难的是获取分页相关的数据比较难。可以借助mybatis的PageHelper插件

### 16.2 PageHelp 插件

#### 第一步：引入依赖

在pom.xml文件中引入依赖：

~~~xml
<dependency>
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId>
  <version>5.3.1</version>
</dependency>
~~~

#### 第二步：在mybatis-config.xml中配置插件

在typeAliases标签下面进行配置：

~~~xml
<plugins>
  <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
~~~

#### 第三步：编写java代码

关键点：

- 在查询语句之前开启分页功能。
- 在查询语句之后封装PageInfo对象。（PageInfo对象将来会存储到request域当中。在页面上展示。）
- 使用PageHelper.startPage(pageNum, pageSize); 开启分页功能
- ​    PageInfo<Car> pageInfo = new PageInfo<>(cars, 5); car为查询出来的数据，5为导航的页数（navigatepages)
- PageInfo对象 有诸多get方法，用来获取相关数据。例如：getNextPage()

测试代码：

~~~java
@Test
public void testPageHelper() throws Exception{
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
    SqlSession sqlSession = sqlSessionFactory.openSession();
    CarMapper mapper = sqlSession.getMapper(CarMapper.class);

    // 开启分页
    PageHelper.startPage(2, 2);

    // 执行查询语句
    List<Car> cars = mapper.selectAll();

    // 获取分页信息对象
    PageInfo<Car> pageInfo = new PageInfo<>(cars, 5);

    System.out.println(pageInfo);
}
~~~

执行结果：

~~~
PageInfo{pageNum=2, pageSize=2, size=2, startRow=3, endRow=4, total=6, pages=3, list=Page{count=true, pageNum=2, pageSize=2, startRow=2, endRow=4, total=6, pages=3, reasonable=false, pageSizeZero=false}[Car{id=86, carNum='1234', brand='丰田霸道', guidePrice=50.5, produceTime='2020-10-11', carType='燃油车'}, Car{id=87, carNum='1234', brand='丰田霸道', guidePrice=50.5, produceTime='2020-10-11', carType='燃油车'}], prePage=1, nextPage=3, isFirstPage=false, isLastPage=false, hasPreviousPage=true, hasNextPage=true, navigatePages=5, navigateFirstPage=1, navigateLastPage=3, navigatepageNums=[1, 2, 3]}
~~~

对执行结果进行格式化：

~~~
PageInfo{
  pageNum=2, pageSize=2, size=2, startRow=3, endRow=4, total=6, pages=3, 
  list=Page{count=true, pageNum=2, pageSize=2, startRow=2, endRow=4, total=6, pages=3, reasonable=false, pageSizeZero=false}
  [Car{id=86, carNum='1234', brand='丰田霸道', guidePrice=50.5, produceTime='2020-10-11', carType='燃油车'}, 
  Car{id=87, carNum='1234', brand='丰田霸道', guidePrice=50.5, produceTime='2020-10-11', carType='燃油车'}], 
  prePage=1, nextPage=3, isFirstPage=false, isLastPage=false, hasPreviousPage=true, hasNextPage=true, 
  navigatePages=5, navigateFirstPage=1, navigateLastPage=3, navigatepageNums=[1, 2, 3]
}
~~~

解释：

> pageNum：页码
>
> pageSize：每页多少条记录
>
> startRow：从第几条记录开始
>
> endRow：第几条记录结束
>
> total：表中总数据条数
>
> pages：页数
>
> prePage：上一页页码
>
> nextPage：下一页页码
>
> isFirstPage：是否为第一页
>
> isLa stPage：是否为最后一页
>
> hasPreviousPage；有没有上一页
>
> hasNextPage：有没有下一页
>
> navigatePages：导航页的页码
>
> navigateFirstPage：导航页的第一页为多少
>
> navigateLastPage：导航页的最后也为多少
>
> navigatepageNums：导航页的页数

## 十七、MyBatis的注解式开发

mybatis中也提供了注解式开发方式，采用注解可以减少Sql映射文件的配置。

当然，使用注解式开发的话，sql语句是写在java程序中的，这种方式也会给sql语句的维护带来成本。

官方是这么说的：

> 使用注解来映射简单语句会使代码显得更加简洁，但对于稍微复杂一点的语句，Java 注解不仅力不从心，还会让你本就复杂的 SQL 语句更加混乱不堪。 因此，如果你需要做一些很复杂的操作，最好用 XML 来映射语句。

使用注解编写复杂的SQL是这样的：

![image-20230824184507420](.\图片\image-20230824184507420.png)

原则：简单sql可以注解。复杂sql使用xml。

### 17.1 @Insert

抽象方法：

~~~java
    @Insert("insert into t_stu values(null,#{sname},#{cid})")
    int insert(Student student);
~~~

测试程序：

```java
@Test
public void testInsert(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    Student student = new Student(null,"老六",1001L);
    int insert = mapper.insert(student);
    sqlSession.commit();
    sqlSession.close();
}
```

### 17.2 @Delete

接口的抽象方法

```java
@Delete("delete from t_stu where sid=#{sid}")
int delete(Long sid);
```

测试代码：

```java
@Test
public void testDeleteById(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    int delete = mapper.delete(1L);
    sqlSession.commit();
    sqlSession.close();
}
```

### 17.3 @Update

接口的抽象方法

```java
@Update("update t_stu set sname=#{sname},cid=#{cid} where sid=#{sid}")
int update(Student student);
```

测试程序：

```java
   public void testUpdate(){
    SqlSession sqlSession = SqlSessionUtil.openSession();
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    Student student = new Student(2L,"张三",1002L);
    int update = mapper.update(student);
    sqlSession.commit();
    sqlSession.close();
}
```

### 17.4 @Select

使用Select注解，可以查询单条记录或者多条记录

查询单条记录

> 接口的抽象方法
>
> ```java
>     @Select("select * from t_stu where sid=#{sid}")
>    Student selectByID(Long sid);
> ```
>
> 测试程序：
>
> ```java
> @Test
>    public void testSelectByID(){
>     SqlSession sqlSession = SqlSessionUtil.openSession();
>     StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
>     Student student = mapper.selectByID(2L);
>     sqlSession.close();
>     System.out.println(student);
> }
> ```

查询多条记录

> 接口的抽象方法
>
> ```java
> @Select("select * from t_stu ")
> List<Student> selectByID();
> ```
>
> 测试程序：
>
> ```java
> @Test
>    public void testSelectByID(){
>     SqlSession sqlSession = SqlSessionUtil.openSession();
>     StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
>     List<Student> students = mapper.selectByID();
>     sqlSession.close();
>     students.forEach(student -> System.out.println(student));
> }
> ```
