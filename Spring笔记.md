## 一、Spring 启示录

前景：

> UserMapperImpl：
>
> ```java
> public class UserMapperImpl implements UserMapper {
>     @Override
>     public void deletById() {
> 
>     }
> }
> ```
>
> UserServiceImpl：
>
> ```java
> public class UserServiceImpl implements UserService {
>     private UserMapper userMapper = new UserMapperImpl();
>     @Override
>     public void deleteUser() {
>     }
> }
> ```
>
> UserAction：
>
> ```java
> public class UserAction {
>     private UserService userService = new UserServiceImpl();
> }
> ```

UserAction 依赖了具体的 UserServiceImpl

UserServiceImpl 依赖了具体的 USerMapperImpl

如果后期会对底层代码进行修改或拓展，那么就会影响整个程序。在这违背了开闭原则。

它们都违背了依赖倒置原则。凡是上依赖下的，都违背了依赖倒置原则。





### 1.1 OCP开闭原则

​	1.什么是OCP：OCP是软件七大开发原则当中最基本的一个原则，开闭原则，对拓展开放，对修改关闭。

​	2.OCP原则的核心，是最基本的，其他六个原则都是为了这个原则服务的

​	3.OCP开闭原则的核心：

> 只要你在扩展系统功能的时候，没有修改以前写好的代码，那么你就是符合OCP原则的。
>
> 反之，如果扩展系统功能的时候，你修改了之前写的代码，那么这个设计是失败的，违背了OCP原则

​	4.当进行系统功能扩展的时候，如果动了1之前的程序，修改了之前的程序，之前所有程序都需要进行重新测试，这是不想看到的，因为非常麻烦。

### 1.2 依赖导致原则 DIP

什么叫做符合依赖倒置原则？什么叫做遵守依赖导致原则？

​		——— 上层代码不再依赖底层代码。

依赖导致原则的核心：

> 倡导面向接口编程，面向抽象编程，不要面向具体编程。这样可以大大降低程序的耦合度，耦合度低了，扩展力就强了，同时代码复用性也会增强

<img src="C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\image-20230825091126742.png" alt="image-20230825091126742" style="zoom:50%;" />

确实已经面向接口编程了，但对象的创建是：new UserDaoImplForOracle()显然并没有完全面向接口编程，还是使用到了具体的接口实现类。什么叫做完全面向接口编程？什么叫做完全符合依赖倒置原则呢？请看以下代码：

<img src="C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\image-20230825091156071.png" alt="image-20230825091156071" style="zoom:50%;" />

如果代码是这样编写的，才算是完全面向接口编程，才符合依赖倒置原则。那你可能会问，这样userDao是null，在执行的时候就会出现空指针异常呀。你说的有道理，确实是这样的，所以我们要解决这个问题。解决空指针异常的问题，其实就是解决两个核心的问题：

> - 第一个问题：谁来负责对象的创建。【也就是说谁来：new UserDaoImplForOracle()/new UserDaoImplForMySQL()】
> - 第二个问题：谁来负责把创建的对象赋到这个属性上。【也就是说谁来把上面创建的对象赋给userDao属性】

### 1.3 控制反转 IoC

控制反转（Inversion of Control，缩写为IoC），是面向对象编程中的一种设计思想，可以用来降低代码之间的耦合度，符合依赖倒置原则。

反转了两件事：

> 1.不再采用硬编码的方式来new 对象了
>
> 2.不再采用硬编码来维护对象的关系了

控制反转的核心是：**将对象的创建权交出去，将对象和对象之间关系的管理权交出去，由第三方容器来负责创建与维护**。

控制反转常见的实现方式：依赖注入（Dependency Injection，简称DI）

通常，依赖注入的实现由包括两种方式：

> - set方法注入（执行set方法给属性赋值）
>
> - 构造方法注入（执行构造方法给属性赋值）

依赖注入：对象A和对象B之间的关系，靠注入的手段来维护，而注入包括set注入和构造方法注入

> - 依赖：对象之间关系
> - 注入：是一种手段，通过这种手段，可以让对象之间产生关系



而Spring框架就是一个实现了IoC思想的框架。

IoC可以认为是一种**全新的设计模式**，但是理论和时间成熟相对较晚，并没有包含在GoF中。（GoF指的是23种设计模式）

### 1.4 Spring框架

Spring框架实现了控制反转IoC这种思想

> Spring框架可以帮助我们new对象
>
> Spring框架可以帮助我们维护对象之间的关系。

Spring框架实现了IoC思想的容器

## 二、Spring 概述

### 2.1 Spring 简介

> Spring是一个开源框架，它由Rod Johnson创建。它是为了解决企业应用开发的复杂性而创建的。
>
> 从简单性、可测试性和松耦合的角度而言，任何Java应用都可以从Spring中受益。
>
> **Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器框架。**
>
> **Spring最初的出现是为了解决EJB臃肿的设计，以及难以测试等问题。**
>
> **Spring为简化开发而生，让程序员只需关注核心业务的实现，尽可能的不再关注非业务逻辑代码（事务控制，安全日志等）**

### 2.2 Spring 8大模块

<img src="C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\1663726169861-b5acb757-17e0-4d3d-a811-400eb7edd1b3.png" alt="image.png" style="zoom:67%;" />

Spring Core模块

> 这是Spring框架最基础的部分，它提供了依赖注入（DependencyInjection）特征来实现容器对Bean的管理。核心容器的主要组件是 BeanFactory，BeanFactory是工厂模式的一个实现，是任何Spring应用的核心。它使用IoC将应用配置和依赖从实际的应用代码中分离出来。

Spring Context模块

> 如果说核心模块中的BeanFactory使Spring成为容器的话，那么上下文模块就是Spring成为框架的原因。
>
> 这个模块扩展了BeanFactory，增加了对国际化（I18N）消息、事件传播、验证的支持。另外提供了许多企业服务，例如电子邮件、JNDI访问、EJB集成、远程以及时序调度（scheduling）服务。也包括了对模版框架例如Velocity和FreeMarker集成的支持

Spring AOP模块

> Spring在它的AOP模块中提供了对面向切面编程的丰富支持，Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中，可以自定义拦截器、切点、日志等操作。

Spring DAO模块

> 提供了一个JDBC的抽象层和异常层次结构，消除了烦琐的JDBC编码和数据库厂商特有的错误代码解析，用于简化JDBC。

Spring ORM模块

> Spring提供了ORM模块。Spring并不试图实现它自己的ORM解决方案，而是为几种流行的ORM框架提供了集成方案，包括Hibernate、JDO和iBATIS SQL映射，这些都遵从 Spring 的通用事务和 DAO 异常层次结构。

Spring Web MVC模块

> Spring为构建Web应用提供了一个功能全面的MVC框架。虽然Spring可以很容易地与其它MVC框架集成，例如Struts，但Spring的MVC框架使用IoC对控制逻辑和业务对象提供了完全的分离。

Spring WebFlux模块

> Spring Framework 中包含的原始 Web 框架 Spring Web MVC 是专门为 Servlet API 和 Servlet 容器构建的。反应式堆栈 Web 框架 Spring WebFlux 是在 5.0 版的后期添加的。它是完全非阻塞的，支持反应式流(Reactive Stream)背压，并在Netty，Undertow和Servlet 3.1+容器等服务器上运行。

Spring Web模块

> Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文，提供了Spring和其它Web框架的集成，比如Struts、WebWork。还提供了一些面向服务支持，例如：实现文件上传的multipart请求。

### 2.3 Spring特点

1. 轻量
   1. 从大小与开销两方面而言Spring都是轻量的。完整的Spring框架可以在一个大小只有1MB多的JAR文件里发布。并且Spring所需的处理开销也是微不足道的。
   1. Spring是非侵入式的：Spring应用中的对象不依赖于Spring的特定类。


2.控制反转

​	1. Spring通过一种称作控制反转（IoC）的技术促进了松耦合。当应用了IoC，一个对象依赖的其它对象会通过被动的方式传递进来，而不是这个对象自己创建或者查找依赖对象。你可以认为IoC与JNDI相反——不是对象从容器中查找依赖，而是容器在对象初始化时不等对象请求就主动将依赖传递给它。

3.面向切面

1. 1. Spring提供了面向切面编程的丰富支持，允许通过分离应用的业务逻辑与系统级服务（例如审计（auditing）和事务（transaction）管理）进行内聚性的开发。应用对象只实现它们应该做的——完成业务逻辑——仅此而已。它们并不负责（甚至是意识）其它的系统级关注点，例如日志或事务支持。

4.容器

1. 1. Spring包含并管理应用对象的配置和生命周期，在这个意义上它是一种容器，你可以配置你的每个bean如何被创建——基于一个可配置原型（prototype），你的bean可以创建一个单独的实例或者每次需要时都生成一个新的实例——以及它们是如何相互关联的。然而，Spring不应该被混同于传统的重量级的EJB容器，它们经常是庞大与笨重的，难以使用。

5.框架

1. 1. Spring可以将简单的组件配置、组合成为复杂的应用。在Spring中，应用对象被声明式地组合，典型地是在一个XML文件里。Spring也提供了很多基础功能（事务管理、持久化框架集成等等），将应用逻辑的开发留给了你。

所有Spring的这些特征使你能够编写更干净、更可管理、并且更易于测试的代码。它们也为Spring中的各种模块提供了基础支持。

## 三、Spring 入门程序

### 3.1 第一个Spring程序

1.引入依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>6.0.0</version>
</dependency>
```

当引入Spring context 依赖之后，表示将Spring的基础依赖引入了

如果你想要使用Spring的jdbc，或者说其他tx，那么还需要再次添加依赖

![image-20230825103304753](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230825103304753.png)

2.创建Spring配置文件

idea给我们提供了这文件的模板，一定要使用这个模板来创建。

![image-20230825104026055](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230825104026055.png)

这个文件名不一定交spring.xml,可以是其他名字。

这个文件最好是放在类路劲中，方便后期的移植。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    
</beans>
```

4.在com.fengshun.frist.bean包下新建一个实体类User

```java
public class User {
    private String uname;
    private String psw;
    //constructor
    //getter and setter
    //tostring
}
```

3.配置bean

只有配置bean，这样spring才可以帮助我们管理这个对象

bean标签的两个重要属性：

> id：就是这个的身份证号，不能重复，是唯一标识
>
> class：必须填写类的全路径，全限定类名

```xml
<bean id="userbean" class="com.fengshun.frist.bean.User"/>
```

4.编写测试代码

```java
@Test
public void testFristSpringCode(){
    // 第一步：获取Spring容器对象
    // ApplicationContext 翻译为应用上下文，其实就是Spring容器，它是一个接口
    // ApplicationContext 接口下有很多实现类，其中有个实现类：ClassPathXmlApplicationContext
    // ClassPathXmlApplicationContext 专门从类路径中华加载Spring配置文件的一个Spring上下文对象。
    // 这行代码只要执行：就相当于启动了Spring容器，解析Spring.xml文件，并且实例化所有的bean对象，放到Spring容器当中
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");

    // 第二步：更加bean的id从Spring容器中获取这个对象
    Object userBean = applicationContext.getBean("userBean");
    System.out.println(userBean);
}
```

### 3..2 第一个Spring程序的详细解剖

1.**bean标签中id属性不能重复**

2.**Sprin是怎么实例化对象的?**

> 默认情况下，Spring对通过反射机制，调用类的无参构造方法来实例化对象

~~~java
   Class<?> aClass = Class.forName("com.fengshun.frist.bean.User");
        aClass.newInstance();
~~~

所以需要被创建的对象，必须要有无参构造方法。

3.**把创建好的对象存储到了什么样的数据接口当中？**

<img src="C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\image-20230825112159028.png" alt="image-20230825112159028" style="zoom:67%;" />

4.**Spring配置文件的名字不是固定的**。

5.**像这样的Spring配置文件可以有多个**

> 例如现在又 bean.xml 和 spring.xml 两个配置文件
>
> 使用：
>
> ~~~java
> package com.powernode.spring6.test;
> 
> import org.junit.Test;
> import org.springframework.context.ApplicationContext;
> import org.springframework.context.support.ClassPathXmlApplicationContext;
> 
> public class Spring6Test {
> 
>     @Test
>     public void testFirst(){
>         // 初始化Spring容器上下文（解析beans.xml文件，创建所有的bean对象）
>         ApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans.xml","spring.xml","xml/userbean.xml");
> 
>         // 根据id获取bean对象
>         Object userBean = applicationContext.getBean("userBean");
>         Object vipBean = applicationContext.getBean("vipBean");
> 
>         System.out.println(userBean);
>         System.out.println(vipBean);
>     }
> }
> 

6.**在配置文件中配置的类必须是自定义的吗，可以使用JDK中的类吗，例如：java.util.Date？**

> 可以使用JDK中的类
>
> 测试：
>
> 配置bean：
>
> ~~~xml
>     <bean id="nowTime" class="java.util.Date"/>
> ~~~
>
> 测试程序;
>
> ```java
> public void testFristSpringCode(){
>     // 第一步：获取Spring容器对象
>     ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
> 
>     // 第二步：更加bean的id从Spring容器中获取这个对象
>     Object nowTime = applicationContext.getBean("nowTime");
>     System.out.println(nowTime);
> }
> ```

**7.getBean()方法调用时，如果指定的id不存在会怎样？**

> 如果bean的id不存在，不会返回null ，而是出现异常![image-20230825113059206](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230825113059206.png)

**8.getBean()方法返回的类型是Object，如果访问子类的特有属性和方法时，还需要向下转型，有其它办法可以解决这个问题吗？**

> 强制类型转换：
>
> ```java
> Date nowTime = (Date) applicationContext.getBean("nowTime");
> ```
>
> 不使用强制类型转换
>
> ```java
> Date nowTime1 = applicationContext.getBean("nowTime", Date.class);
> ```

**9.ClassPathXmlApplicationContext是从类路径中加载配置文件，如果没有在类路径当中，又应该如何加载配置文件呢？**

> 使用 FileSystemXmlApplicationContext 接收。
>
> ~~~java
> ApplicationContext applicationContext2 = new FileSystemXmlApplicationContext("d:/spring6.xml");
> Vip vip = applicationContext2.getBean("vipBean2", Vip.class);
> System.out.println(vip);

**10.ApplicationContext的超级父接口BeanFactory。**

> ~~~java
> BeanFactory beanFactory = new ClassPathXmlApplicationContext("spring.xml");
> Object vipBean = beanFactory.getBean("vipBean");
> System.out.println(vipBean);
> ~~~
>
> BeanFactory是Spring容器的超级接口。ApplicationContext是BeanFactory的子接口。
>
> BeanFactory 翻译为Bean工厂，就是能够生成Bean对象的一个工厂对象
>
> BeanFactory 是IoC容器的顶级接口
>
> Spring的IoC容器底层实际上使用了工厂模式
>
> Spring底层的IoC就是通过 xml解析+工厂模式+ 反射机制 实现的

**11.对象什么时候被创建**：

>   不是在调用getBean（） 方法的时候创建对象，执行以下代码的时候，就会实例化对象。
>
> ~~~java
>     @Test
>     public void test(){
>         new ClassPathXmlApplicationContext("spring.xml");
>     }
> ~~~



### 3.3 Spring6使用log4j2日志框架

从Spring5之后，Spring框架支持集成的日志框架是Log4j2.如何启用日志框架：

第一步：引入Log4j2的依赖：

~~~xml
<!--log4j2的依赖-->
<dependency>
  <groupId>org.apache.logging.log4j</groupId>
  <artifactId>log4j-core</artifactId>
  <version>2.19.0</version>
</dependency>
<dependency>
  <groupId>org.apache.logging.log4j</groupId>
  <artifactId>log4j-slf4j2-impl</artifactId>
  <version>2.19.0</version>
</dependency>
~~~



第二步：在类的根路径下提供log4j2.xml配置文件（文件名固定为：log4j2.xml，文件必须放到类根路径下。）

~~~xml
<?xml version="1.0" encoding="UTF-8"?>

<configuration>

    <loggers>
        <!--
            level指定日志级别，从低到高的优先级：
                ALL < TRACE < DEBUG < INFO < WARN < ERROR < FATAL < OFF
        -->
        <root level="DEBUG">
            <appender-ref ref="spring6log"/>
        </root>
    </loggers>

    <appenders>
        <!--输出日志信息到控制台-->
        <console name="spring6log" target="SYSTEM_OUT">
            <!--控制日志输出的格式-->
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss SSS} [%t] %-3level %logger{1024} - %msg%n"/>
        </console>
    </appenders>

</configuration>
~~~



第三步：使用日志框架

```java
public void test(){
    new ClassPathXmlApplicationContext("spring.xml");
    // 自己怎么使用log42记录日志信息呢？
    // 第一步:创建日志记录器对象
    // 获取FristSpringTest 类的日志记录器对象，也就是说只要是FristSpringTest嘞中的代码执行记录日志的话，就输出相关的日志信息。
    Logger logger = LoggerFactory.getLogger(FristSpringTest.class);
    // 第二步:根据不同的级别来输出日志信息
    logger.info("我是一条消息");
    logger.debug("我是一条调试信息");
    logger.error("我是一条错误信息");

}
```

## 四、Spring对IoC的实现

### 4.1 IoC的控制反转

- 控制反转是一种思想。
- 控制反转是为了降低程序耦合度，提高程序扩展力，达到OCP原则，达到DIP原则。
- 控制反转，反转的是什么？

- - 将对象的创建权利交出去，交给第三方容器负责。
  - 将对象和对象之间关系的维护权交出去，交给第三方容器负责。

- 控制反转这种思想如何实现呢？

- - DI（Dependency Injection）：依赖注入

### 4.2 依赖注入

依赖注入实现了控制反转的思想。

**Spring通过依赖注入的方式来完成Bean管理的。**

**Bean管理说的是：Bean对象的创建，以及Bean对象中属性的赋值（或者叫做Bean对象之间关系的维护）。**

依赖注入：

- 依赖指的是对象和对象之间的关联关系。
- 注入指的是一种数据传递行为，通过注入行为来让对象和对象产生关系。

依赖注入常见的实现方式包括两种：

- 第一种：set注入
- 第二种：构造注入

------

#### 4.2.1 set注入

set注入，基于set方法实现的，底层会通过反射机制调用属性对应的set方法然后给属性赋值。这种方式要求属性必须对外提供set方法。

set方法可以是idea自动生成的方法，也可以是自己定义的方法。

如果是自己定义的方法，那么方法名必须以set开始，参数必须为要被注入的对象

添加set方法

> idea自动生成的set方法：
>
> ~~~java
> public class UserService {
>     private UserDao userDao;
> 
>     //set注入，必须要有一个set方法
>     // Spring容器会调用这个set方法，来给userDao属性赋值
> 
> 
>     public void setUserDao(UserDao userDao) {
>         this.userDao = userDao;
>     }
>  }
> ~~~
>
> 手写set方法：
>
> ~~~java
> public class UserService {
>     private UserDao userDao;
> 
>     public void setSpringuser(UserDao userDao) {
>         this.userDao = userDao;
>     }
>  }

在Spring配置文件中配置：

> ```xml
>     <bean id="userDaoBean" class="com.fengshun.dependency.dao.UserDao"/>
>     <bean id="userServiceBean" class="com.fengshun.dependency.service.UserService">
>         <property name="userDao" ref="userDaoBean"/>
>     </bean>
> </beans>
> ```
>
> UserDao 和 UserService 两个类都需要实例化对象，所有都需要在Spring配置文件中配置。
>
> bean标签下还有property 子标签，property 标签中有两个属性：
>
> > - name属性：它就是需要被注入的类的set方法的方法名，但是需要去除set前缀，并且剩下的方法名需要采用小驼峰命名法，例如：setSpringUser ==> springUser;  setPassword ==> password
> > - ref 属性：指向的是需要被注入的类的bean标签的id属性。UserDao 需要在 UserService中被注入，那么ref就应该是UserDao的id属性的值

实现原理：

> 1. 通过property标签获取到属性名：userDao
> 2. 通过属性名推断出set方法名：setUserDao
> 3. 通过反射机制调用setUserDao()方法给属性赋值
> 4. property标签的name是属性名。
> 5. property标签的ref是要注入的bean对象的id。**(通过ref属性来完成bean的装配，这是bean最简单的一种装配方式。装配指的是：创建系统组件之间关联的动作)**

对象被创建的时机：调用set方法的时候对象才会被创建

#### 4.2.2构造注入

核心原理：通过构造方法来给属性赋值。

与set方法不同，构造注入是类被实例化时就调用其构造方法，就将嵌套的类实例化。

构造注入使用的标签是constructor-arg，它有两个重要的属性值:

> index 参数：指定构造方法的第几个参数，也就是索引号
>
> ref 参数：指向的是需要被注入的类的bean标签的id属性。

​	构造方法：

~~~java
public class CustomerService {
    private UserDao userDao;
    private VipDao vipDao;

    public CustomerService(UserDao userDao, VipDao vipDao) {
        this.userDao = userDao;
        this.vipDao = vipDao;
    }
  }
~~~

spring配置文件：

```xml
    
    <bean id="userDaoBean" class="com.fengshun.dependency.dao.UserDao"/>
    <bean id="vipDaoBean" class="com.fengshun.dependency.dao.VipDao"/>
    
    <bean id="customerServiceBean" class="com.fengshun.dependency.service.CustomerService">
        <!--构造注入-->
        <!--指定构造方法的第一个参数-->
        <constructor-arg index="0" ref="userDaoBean"/>
        <!--指定构造方法的第一个参数-->
        <constructor-arg index="1" ref="vipDaoBean"/>
    </bean>
</beans>
```

除了可以使用index标记传递的值以外，还能使用name属性：

~~~xml
    <bean id="customerServiceBean" class="com.fengshun.dependency.service.CustomerService">
        <!--构造注入-->
        <!--指定构造方法的第一个参数-->
        <constructor-arg name="userDao" ref="userDaoBean"/>
        <!--指定构造方法的第一个参数-->
        <constructor-arg name="vipDao" ref="vipDaoBean"/>
    </bean>
~~~

name 属性的值，需要和构造方法中参数名一致。

------

不指定下标，也不指定参数名，spring也能自己做类型匹配

> 这种方式实际上是根据类型进行注入的，sprin对自动根据类型来判断把ref注入给哪个参数。
>
> ```xml
> <bean id="customerServiceBean" class="com.fengshun.dependency.service.CustomerService">
>     <constructor-arg  ref="userDaoBean"/>
>     <constructor-arg  ref="vipDaoBean"/>
> </bean>
> ```

### 4.3 set注入专题

#### 4.3.1 注入外部Bean

在property标签中使用ref属性就叫外部bean

~~~xml
<!--声明bean-->
<bean id="orderDaoBean" class="com.fengshun.dependency.dao.OrderDao"/>

<bean id="orderServiceBean" class="com.fengshun.dependency.service.OrderService">
        <property name="orderDao" ref="orderDaoBean"/>
    </bean>
~~~

#### 4.3.2 注入内部Bean

在property标签中再嵌套一个bean标签，bean标签的属性使用class属性：

~~~xml
<!--声明bean-->
<bean id="orderDaoBean" class="com.fengshun.dependency.dao.OrderDao"/>
<!--使用内部bean-->
<bean id="orderServiceBean2" class="com.fengshun.dependency.service.OrderService">
        <property name="orderDao">
            <bean class="com.fengshun.dependency.dao.OrderDao"/>
        </property>
</bean>
~~~

#### 4.3.3 注入简单类型

如果给简单类型注入，那么在property标签中不再使用ref属性，而是value

pojo对象：

~~~java
package com.fengshun.dependency.pojo;

public class User {
    private String uname;
    private String password;
    private int age;


    public User() {
    }

    public void setUname(String uname) {
        this.uname = uname;
    }


    public void setPassword(String password) {
        this.password = password;
    }


    public void setAge(int age) {
        this.age = age;
    }
}

~~~

Spring 配置：

```xml
<bean id="userBean" class="com.fengshun.dependency.pojo.User">
    <property name="age" value="18"/>
    <property name="password" value="051727"/>
    <property name="uname" value="root"/>
</bean>
```

Spring框架判定简单类型的代码：

~~~java
	public static boolean isSimpleValueType(Class<?> type) {
		return (Void.class != type && void.class != type &&
				(ClassUtils.isPrimitiveOrWrapper(type) ||
				Enum.class.isAssignableFrom(type) ||
				CharSequence.class.isAssignableFrom(type) ||
				Number.class.isAssignableFrom(type) ||
				Date.class.isAssignableFrom(type) ||
				Temporal.class.isAssignableFrom(type) ||
				URI.class == type ||
				URL.class == type ||
				Locale.class == type ||
				Class.class == type));
	}
~~~



简单类型包括：

> - 基本数据类型
> - 基本数据类型对应的包装类
> - String或其他的CharSequence子类
> - Number子类
> - Date子类
> - Enum子类
> - URI
> - URL
> - Temporal子类
> - Locale
> - Class
> - 另外还包括以上简单值类型对应的数组类型。
>
> 在实际开发中，一般不会把Date当做简单类型，一般使用ref给Date赋值

------

#### 4.3.4 联级属性赋值

普通赋值：

~~~xml
<bean id="studentBean" class="com.fengshun.dependency.pojo.Student">
    <property name="name" value="张三"/>
    <property name="sid" value="1001"/>
    <property name="clazz" ref="clazzBean"/>
</bean>
<bean id="clazzBean" class="com.fengshun.dependency.pojo.Clazz">
    <property name="cid" value="1"/>
    <property name="cname" value="软件"/>
</bean>
~~~



采用级联属性赋值

```xml
<bean id="studentBean" class="com.fengshun.dependency.pojo.Student">
    <property name="name" value="张三"/>
    <property name="sid" value="1001"/>
    <property name="clazz" ref="clazzBean"/>
    <property name="clazz.cid" value="1"/>
    <property name="clazz.cname" value="软件"/>
</bean>
<bean id="clazzBean" class="com.fengshun.dependency.pojo.Clazz"/>
```

采用级联属性赋值，clazz中的属性不光需要有set方法，还需要有get方法。并且在StudentBean中的配置不能颠倒。

------

#### 4.3.5 注入数组

1.数组属于简单类型：

> User 实体类：
>
> ```java
> public class User {
> 
>     private String[] aiHao;
>     
>     private DingDan[] dingDans;
> 
> 
>     public void setAiHao(String[] aiHao) {
>         this.aiHao = aiHao;
>     }
> 
>     public void setDingDans(DingDan[] dingDans) {
>         this.dingDans = dingDans;
>     }
> 
>     public User() {
>     }
> 
> }
> ```
>
> Spring配置文件：
>
> ```xml
> <bean id="userArrayBean" class="com.fengshun.dependency.pojo.User">
>     <property name="aiHao">
>         <array>
>             <value>唱</value>
>         	<value>跳</value>
>         	<value>篮球</value>
>         </array>
>     </property>
> </bean>
> ```

2. 非简单类型数组注入：

> User 实体类：
>
> ~~~java
> public class User {
> 
>     private DingDan[] dingDans;
> 
>     public void setDingDans(DingDan[] dingDans) {
>         this.dingDans = dingDans;
>     }
> 
>     public User() {
>     }
> 
> }
> ~~~
>
> DingDan 实体类：
>
> ~~~java
> 
> public class DingDan {
>     private String name;
> 
>     public DingDan() {
>     }
> 
>     public void setName(String name) {
>         this.name = name;
>     }
>     
> }
> ~~~
>
> Spring 配置文件：
>
> ~~~xml
> <bean id="dingDanBean1" class="com.fengshun.dependency.pojo.DingDan">
>         <property name="name" value="订单1"/>
>     </bean>
>     
>     <bean id="dingDanBean2" class="com.fengshun.dependency.pojo.DingDan">
>         <property name="name" value="订单2"/>
>     </bean>
>     
>     <bean id="dingDanBean3" class="com.fengshun.dependency.pojo.DingDan">
>         <property name="name" value="订单3"/>
>     </bean>
> 
> 
>     <bean id="userArrayBean" class="com.fengshun.dependency.pojo.User">
>         <property name="dingDans">
>             <array>
>                 <ref bean="dingDanBean1"/>
>                 <ref bean="dingDanBean2"/>
>                 <ref bean="dingDanBean3"/>
>             </array>
>         </property>
>     </bean>
> ~~~
>
> 

------

#### 4.3.6 注入List集合

**<u>List 集合是有序可重复的。</u>**

```xml
<bean id="useListBean" class="com.fengshun.dependency.pojo.User">
    <property name="names">
        <list>
            <value>张三</value>
            <value>李四</value>
            <value>王五</value>
        </list>
    </property>
</bean>
```



当list中的数据类型为简单数据类型时，使用value标签对其赋值。

如果List 集合中的数据类型为复杂数据类型时，需要使用 ref 标签。

------

#### 4.3.7 注入Set集合

<u>**Set 集合为无序不重复**</u>

```xml
<bean id="useSetBean" class="com.fengshun.dependency.pojo.User">
    <property name="addresses">
        <set>
            <value>四川省</value>
            <value>重庆市</value>
            <value>北京市</value>
        </set>
    </property>
</bean>
```

Set集合 和上面的一样 如果是简单类型可以使用value 如果是复杂类型数据，需要使用ref

------

#### 4.3.8 注入Map集合

如果map中的数据是简单类型数据：可以在entry中使用key 和 value 属性。如果不是简单类型，那么需要使用key-ref  和 value-ref  他们等价于ref。

简单数据类型：

```xml
<bean id="userMapBean" class="com.fengshun.dependency.pojo.User">
    <property name="map">
        <map>
            <entry key="1" value="String1"/>
            <entry key="2" value="String2"/>
            <entry key="3" value="String3"/>
        </map>
    </property>
</bean>
```

key和value都不是简单数据类型：

```xml
<bean id="userMapBean" class="com.fengshun.dependency.pojo.User">
    <property name="map">
        <map>
            <entry key-ref="userBean" value-ref="vipBean"/>
        </map>
    </property>
</bean>
```



------

#### 4.3.9 注入 Properties

Properties 是注入属性类对象。

Properties 本质上也是一个map集合，

Properties 的父类是HashTable ，HashTable 实现了Map接口。

虽然Properties 也是一个map集合但是和map集合的注入方式相似，但是不是完全相同。

Properties 的key和value属性的值，只能是字符串

```java
public class User {

    private Properties properties;

    public void setProperties(Properties properties) {
        this.properties = properties;
    }
  }
```

spring 配置文件：

```xml
<bean id="userPropertiesBean" class="com.fengshun.dependency.pojo.User">
    <property name="properties">
        <props>
            <prop key="driver">com.mysql.cj.jdbc.Driver</prop>
            <prop key="url">jdbc:mysql://localhost:3306/spring</prop>
            <prop key="name">root</prop>
            <prop key="password">root</prop>
        </props>
    </property>
</bean>
```

------

#### 4.3.10 注入null和空字符串

注入null

> 1.不给属性注入，属性的默认值就是null
>
> 2.使用null标签
>
> ~~~xml
> <bean id="userNUllBean" class="com.fengshun.dependency.pojo.User">
>     <property name="map">
>         <null/>
>     </property>
> </bean>
> ~~~

注入空字符串

> 1.不给属性赋值
>
> ~~~xml
> <bean id="userStringBean" class="com.fengshun.dependency.pojo.User">
>     <property name="name" value=""/>
> </bean>
> ~~~
>
> 2.使用value标签
>
> ~~~xml
> <bean id="userStringBean" class="com.fengshun.dependency.pojo.User">
>     <property name="name">
>         <value/>
>     </property>
> </bean>
> ~~~

#### 4.3.11 注入的值含有特殊符号

ML中有5个特殊字符，分别是：< 、> 、'  、"  、&

以上5个特殊符号在XML中会被特殊对待，会被当做XML语法的一部分进行解析，如果这些特殊符号直接出现在注入的字符串当中，会报错。

解决方案包括两种：：

> 第一种：特殊符号使用转义字符代替。
>
> ​		5个特殊字符对应的转义字符分别是：
>
> ![image-20230827153309497](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230827153309497.png)
>
> ~~~xml
> <bean id="mathBean" class="com.powernode.spring6.beans.Math">
>         <property name="result" value="2 &lt; 3"/>
>     </bean>
> ~~~
>
> 
>
> 第二种：将含有特殊符号的字符串放到：<![CDATA[]]> 当中。因为放在CDATA区中的数据不会被XML文件解析器解析。
>
> 注意：使用 CDATA 只能使用value标签，不能用value属性

### 4.4 P命名空间注入

p命名空间注入底层还是set注入，只不过p命名空间注入可以让 spring 配置文件变得更简单。

第一步在spring配置文件头部添加p命名空间：

```
xmlns:p="http://www.springframework.org/schema/p"
```

第二步：使用

在bean标签中 使用  p:属性名  或者  p:属性名-ref

~~~xml
<!--name 为简单类型数据，clazz为非简单类型数据-->
<!---->
<bean id="clazzBean" clazz="com.fengshun.test.pojo.Clazz"/>
<bean id="studentBean" clazz="com.fengshun.test.pojo.Student" p:name="张三" p:clazz-ref="clazzBean"/>
~~~

### 4.5 C命名空间注入

c命名空间是简化构造方法注入的

使用c 命名空间需要两个条件：

> 第一：需要在xml配置文件头部添加信息：xmlns:c="http://www.springframework.org/schema/c"
>
> 第二：需要提供构造方法。

1.根据构造方法的参数下标注入：

~~~xml
<bean id="clazzBean" clazz="com.fengshun.test.pojo.Clazz"/>
<bean id="studentBean" clazz="com.fengshun.test.pojo.Student" c:_0="张三" c:_1-ref="clazzBean"/>
~~~

2.根据构造方法的参数名注入：

~~~xml
<bean id="clazzBean" clazz="com.fengshun.test.pojo.Clazz"/>
<bean id="studentBean" clazz="com.fengshun.test.pojo.Student" c:name="张三" c:clazz-ref="clazzBean"/>
~~~

### 4.6 util 命名空间注入

引入util命名空间，需要在spring的配置文件添加：

> 1. 在beans标签中添加属性配置
>
>    ~~~
>    xmlns:util="http://www.springframework.org/schema/util"
>
> 2. 在 xsi:schemaLocation 属性中添加xml约束：
>
>    ```
>    http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd
>    ```
>
> ```xml
> <beans xmlns="http://www.springframework.org/schema/beans"
>        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>        xmlns:util="http://www.springframework.org/schema/util"
>        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
>                            http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
> ```

util 命名空间能做到代码复用，比如在两条bean语句中，都需要使用相同的属性以及相同的值，那么就可以使用util 命名空间

~~~xml
	<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

    <util:properties id="prop">
        <prop key="driver">com.mysql.cj.jdbc.Driver</prop>
        <prop key="url">jdbc:mysql://localhost:3306/spring</prop>
        <prop key="username">root</prop>
        <prop key="password">123456</prop>
    </util:properties>

    <bean id="dataSource1" class="com.powernode.spring6.beans.MyDataSource1">
        <property name="properties" ref="prop"/>
    </bean>

    <bean id="dataSource2" class="com.powernode.spring6.beans.MyDataSource2">
        <property name="properties" ref="prop"/>
    </bean>
</beans>
~~~

util 命名空间注入，不光可以使用properties 还能使用List集合等。

### 4.7 基于XML的自动装配

Spring还可以完成自动化的注入，自动化注入又被称为自动装配。它可以根据**名字**进行自动装配，也可以根据**类型**进行自动装配。

#### 4.7.1 根据名称自动装配

java类：

~~~java
package com.powernode.spring6.service;

import com.powernode.spring6.dao.UserDao;

public class UserService {

    private UserDao userDao;

    // 这个set方法非常关键
    public void setUserDao(UserDao aaa) {
        this.aaa = aaa;
    }

    public void save(){
        aaa.insert();
    }
}

~~~

spring 配置文件：

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userService" class="com.powernode.spring6.service.UserService" autowire="byName"/>
    
    <bean id="userDao" class="com.powernode.spring6.dao.UserDao"/>

</beans>
~~~

注意：

> 1. 想要使用自动装配，也必须要有属性的set方法
> 2. 根据名字进行自动装配的时候，被注入的对象的bean的id不能随便写，需要该对象的set方法，去掉set，剩下首字母小写。
> 3.  autowire="byName" 属性表示，根据名称进行自动装配

#### 4.7.2 根据类型自动装配

自动装配是基于set方法

~~~xml
    <!--byType表示根据类型自动装配-->
    <bean id="accountService" class="com.powernode.spring6.service.AccountService" autowire="byType"/>

    <bean class="com.powernode.spring6.dao.AccountDao"/>
    
~~~

根据类型进行自动装配的时候，在有效的配置文件中，某个类型的实例只能有一个。

例如：(这样会报错)

~~~xml
    <bean id="x" class="com.powernode.spring6.dao.AccountDao"/>
    <bean id="y" class="com.powernode.spring6.dao.AccountDao"/>
~~~



### 4.8 Spring引入外部属性配置文件

编写jdbc.properties 配置文件：

~~~
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://3306/spring
jdbc.root=root
jdbc.password=051727
~~~

第一步：引入context命名空间：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
```

第二步：使用标签 context:property-placeholder 的 location 属性来指定属性配置文件的路径

> location 默认从类的根路径下开始加载资源
>
> ~~~xml
> <context:property-placeholder location="jdbc.properties"/>
> ~~~

第三步：使用配置文件中的信息

> 使用${} 中编写配置文件中的key即可
>
> ~~~xml
> <bean id="dataSource" class="com.powernode.spring6.beans.MyDataSource">
>         <property name="driver" value="${driver}"/>
>         <property name="url" value="${url}"/>
>         <property name="username" value="${username}"/>
>         <property name="password" value="${password}"/>
> </bean>
> ~~~

## 五、Bean的作用域

spring在默认情况下，是如何管理bean的：

> 默认情况下，Bean是单例的。
>
> 在Spring上下文初始化的时候实例化。
>
> 每一次调用getBean() 的时候都会返回那个单例的对象。

### 5.1 singleton（单例）与  prototype (原型）

scope 默认值为singlaton ，表示bean是单例的，在spring上下文初始化的时候实例化。

当将bean的scope属性设置为prototype：

> bean 就是多例的。
>
> spring上下文初始化的时候，并不会初始化这些peototype 的bean
>
> 每一次调用getBea（）方法的时候，实例化bean对象
>
> peototype 翻译为 ：原型
>
> ```xml
> <bean id="userBean" class="com.fengshun.dependency.pojo.User" scope="prototype">
> ```

### 5.2 其他scope

scope 属性的值除了 singleton 和 prototype 以外，在web项目中，还有request 和 session

request 和 session 要求项目必须是一个web应用，当你引入springmvc 框架的时候，这两个值就可以使用了。

request ： 一次请求中只有一个bean

session ：一次会话中只有一个bean



**scope属性的值不止两个，它一共包括8个选项：**

> - singleton：默认的，单例。
> - prototype：原型。每调用一次getBean()方法则获取一个新的Bean对象。或每次注入的时候都是新对象。
> - request：一个请求对应一个Bean。**仅限于在WEB应用中使用**。
> - session：一个会话对应一个Bean。**仅限于在WEB应用中使用**。
> - global session：**portlet应用中专用的**。如果在Servlet的WEB应用中使用global session的话，和session一个效果。（portlet和servlet都是规范。servlet运行在servlet容器中，例如Tomcat。portlet运行在portlet容器中。）
> - application：一个应用对应一个Bean。**仅限于在WEB应用中使用。**
> - websocket：一个websocket生命周期对应一个Bean。**仅限于在WEB应用中使用。**
> - 自定义scope：很少使用。

##  六、GoF之工厂模式

### 6.1 工厂模式的三种形态

工厂模式通常有三种形态：

- 第一种：**简单工厂模式（Simple Factory）：不属于23种设计模式之一。简单工厂模式又叫做：静态 工厂方法模式。简单工厂模式是工厂方法模式的一种特殊实现。**
- 第二种：工厂方法模式（Factory Method）：是23种设计模式之一。
- 第三种：抽象工厂模式（Abstract Factory）：是23种设计模式之一。

### 6.2 简单工厂模式

抽象产品角色：

> Weapon 类：
>
> ~~~java
> package com.powernode.factory;
> 
> /**
>  * 武器（抽象产品角色）
>  * @author 动力节点
>  * @version 1.0
>  * @className Weapon
>  * @since 1.0
>  **/
> public abstract class Weapon {
>     /**
>      * 所有的武器都有攻击行为
>      */
>     public abstract void attack();
> }
> ~~~
>
> 具体产品角色：
>
> > Tank 类：
> >
> > ~~~java
> > package com.powernode.factory;
> > 
> > /**
> >  * 坦克（具体产品角色）
> >  * @author 动力节点
> >  * @version 1.0
> >  * @className Tank
> >  * @since 1.0
> >  **/
> > public class Tank extends Weapon{
> >     @Override
> >     public void attack() {
> >         System.out.println("坦克开炮！");
> >     }
> > }
> > 
> > ~~~
> >
> > Fighter 类：
> >
> > ~~~java
> > package com.powernode.factory;
> > 
> > /**
> >  * 战斗机（具体产品角色）
> >  * @author 动力节点
> >  * @version 1.0
> >  * @className Fighter
> >  * @since 1.0
> >  **/
> > public class Fighter extends Weapon{
> >     @Override
> >     public void attack() {
> >         System.out.println("战斗机投下原子弹！");
> >     }
> > }
> > 
> > ~~~
> >
> > Dagger 类：
> >
> > ~~~java
> > package com.powernode.factory;
> > 
> > /**
> >  * 匕首（具体产品角色）
> >  * @author 动力节点
> >  * @version 1.0
> >  * @className Dagger
> >  * @since 1.0
> >  **/
> > public class Dagger extends Weapon{
> >     @Override
> >     public void attack() {
> >         System.out.println("砍他丫的！");
> >     }
> > }
> > 
> > ~~~
> >
> > 
>
> 工厂类角色：
>
> > WeaponFactory 类：
> >
> > ~~~java
> > package com.powernode.factory;
> > 
> > /**
> >  * 工厂类角色
> >  * @author 动力节点
> >  * @version 1.0
> >  * @className WeaponFactory
> >  * @since 1.0
> >  **/
> > public class WeaponFactory {
> >     /**
> >      * 根据不同的武器类型生产武器
> >      * @param weaponType 武器类型
> >      * @return 武器对象
> >      */
> >     public static Weapon get(String weaponType){
> >         if (weaponType == null || weaponType.trim().length() == 0) {
> >             return null;
> >         }
> >         Weapon weapon = null;
> >         if ("TANK".equals(weaponType)) {
> >             weapon = new Tank();
> >         } else if ("FIGHTER".equals(weaponType)) {
> >             weapon = new Fighter();
> >         } else if ("DAGGER".equals(weaponType)) {
> >             weapon = new Dagger();
> >         } else {
> >             throw new RuntimeException("不支持该武器！");
> >         }
> >         return weapon;
> >     }
> > }
> > 
> > ~~~

简单工厂模式的优点：

- 客户端程序不需要关心对象的创建细节，需要哪个对象时，只需要向工厂索要即可，初步实现了责任的分离。客户端只负责“消费”，工厂负责“生产”。生产和消费分离。

简单工厂模式的缺点：

- 缺点1：工厂类集中了所有产品的创造逻辑，形成一个无所不知的全能类，有人把它叫做上帝类。显然工厂类非常关键，不能出问题，一旦出问题，整个系统瘫痪。
- 缺点2：不符合OCP开闭原则，在进行系统扩展时，需要修改工厂类。

**Spring中的BeanFactory就使用了简单工厂模式。**



### 6.3 工厂方法模式

工厂方法模式既保留了简单工厂模式的优点，同时又解决了简单工厂模式的缺点。

工厂方法模式的角色包括：

- **抽象工厂角色**
- **具体工厂角色**
- 抽象产品角色
- 具体产品角色

抽象产品角色：

> ~~~java
> package com.powernode.factory;
> 
> /**
>  * 武器类（抽象产品角色）
>  * @author 动力节点
>  * @version 1.0
>  * @className Weapon
>  * @since 1.0
>  **/
> public abstract class Weapon {
>     /**
>      * 所有武器都有攻击行为
>      */
>     public abstract void attack();
> }
> 
> ~~~

具体产品角色：



> Gun 类：
>
> ~~~java
> package com.powernode.factory;
> 
> /**
>  * 具体产品角色
>  * @author 动力节点
>  * @version 1.0
>  * @className Gun
>  * @since 1.0
>  **/
> public class Gun extends Weapon{
>     @Override
>     public void attack() {
>         System.out.println("开枪射击！");
>     }
> }
> 
> ~~~
>
> Fighter 类：
>
> ~~~java
> package com.powernode.factory;
> 
> /**
>  * 具体产品角色
>  * @author 动力节点
>  * @version 1.0
>  * @className Fighter
>  * @since 1.0
>  **/
> public class Fighter extends Weapon{
>     @Override
>     public void attack() {
>         System.out.println("战斗机发射核弹！");
>     }
> }
> 
> ~~~
>
> 

抽象工厂角色：

~~~java
package com.powernode.factory;

/**
 * 武器工厂接口(抽象工厂角色)
 * @author 动力节点
 * @version 1.0
 * @className WeaponFactory
 * @since 1.0
 **/
public interface WeaponFactory {
    Weapon get();
}

~~~

具体工厂角色：

> GunFactoey 工厂类：
>
> ~~~java
> package com.powernode.factory;
> 
> /**
>  * 具体工厂角色
>  * @author 动力节点
>  * @version 1.0
>  * @className GunFactory
>  * @since 1.0
>  **/
> public class GunFactory implements WeaponFactory{
>     @Override
>     public Weapon get() {
>         return new Gun();
>     }
> }
> 
> ~~~
>
> FighterFactory 工厂类：
>
> ~~~java
> package com.powernode.factory;
> 
> /**
>  * 具体工厂角色
>  * @author 动力节点
>  * @version 1.0
>  * @className FighterFactory
>  * @since 1.0
>  **/
> public class FighterFactory implements WeaponFactory{
>     @Override
>     public Weapon get() {
>         return new Fighter();
>     }
> }
> 
> ~~~
>
> 

我们可以看到在进行功能扩展的时候，不需要修改之前的源代码，显然工厂方法模式符合OCP原则。

工厂方法模式的优点：

- 一个调用者想创建一个对象，只要知道其名称就可以了。 
- 扩展性高，如果想增加一个产品，只要扩展一个工厂类就可以。
- 屏蔽产品的具体实现，调用者只关心产品的接口。

工厂方法模式的缺点：

- 每次增加一个产品时，都需要增加一个具体类和对象实现工厂，使得系统中类的个数成倍增加，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。这并不是什么好事。

### 6.4 抽象工厂模式

抽象工厂模式相对于工厂方法模式来说，就是工厂方法模式是针对一个产品系列的，而抽象工厂模式是针对多个产品系列的，即工厂方法模式是一个产品系列一个工厂类，而抽象工厂模式是多个产品系列一个工厂类。

抽象工厂模式特点：抽象工厂模式是所有形态的工厂模式中最为抽象和最具一般性的一种形态。抽象工厂模式是指当有多个抽象角色时，使用的一种工厂模式。抽象工厂模式可以向客户端提供一个接口，使客户端在不必指定产品的具体的情况下，创建多个产品族中的产品对象。它有多个抽象产品类，每个抽象产品类可以派生出多个具体产品类，一个抽象工厂类，可以派生出多个具体工厂类，每个具体工厂类可以创建多个具体产品类的实例。每一个模式都是针对一定问题的解决方案，工厂方法模式针对的是一个产品等级结构；而抽象工厂模式针对的是多个产品等级结果。

抽象工厂中包含4个角色：

- 抽象工厂角色
- 具体工厂角色
- 抽象产品角色
- 具体产品角色

抽象工厂模式的类图如下：



![img](C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\1665370116084-46b714b8-95d2-45c5-89b6-564057c45694.png)

抽象工厂模式的优缺点：

- 优点：当一个产品族中的多个对象被设计成一起工作时，它能保证客户端始终只使用同一个产品族中的对象。
- 缺点：产品族扩展非常困难，要增加一个系列的某一产品，既要在AbstractFactory里加代码，又要在具体的里面加代码。

## 七、Bean的实例化方式

Spring为Bean提供了多种实例化方式，通常包括4种方式。（也就是说在Spring中为Bean对象的创建准备了多种方案，目的是：更加灵活）

- 第一种：通过构造方法实例化
- 第二种：通过简单工厂模式实例化
- 第三种：通过factory-bean实例化
- 第四种：通过FactoryBean接口实例化

------

### 7.1 通过构造方法实例化

之前一直使用的就是这种方式。默认情况下，会调用Bean的无参数构造方法。

User 类：

~~~java
package com.powernode.spring6.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className User
 * @since 1.0
 **/
public class User {
    public User() {
        System.out.println("User类的无参数构造方法执行。");
    }
}

~~~

spring 配置文件：

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userBean" class="com.powernode.spring6.bean.User"/>

</beans>
~~~



### 7.2 通过简单工厂模式实例化

Spring 提供的实例化方式第二种：通过简单工厂模式，需要在spring配置为文件中告诉spring 框架，调用哪个类的哪个方法获取Bean

factory-method 属性指定的是工厂类当中的静态方法，也就是告诉Spring框架，调用这个方法可以获取bean

第一步：定义一个Bean

> ~~~Java
> package com.powernode.spring6.bean;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className Vip
>  * @since 1.0
>  **/
> public class Vip {
> }
> 
> ~~~

第二步：编写简单工厂模式当中的工厂类

> 静态方法实例化
>
> ~~~java
> package com.powernode.spring6.bean;
> 
> public class VipFactory {
>     public static Vip get(){
>         return new Vip();
>     }
> }
> 
> ~~~

第三步：在Spring配置文件中指定创建该Bean的方法（使用factory-method属性指定）

> ~~~java
> <bean id="vipBean" class="com.powernode.spring6.bean.VipFactory" factory-method="get"/>
> ~~~

第四步：编写测试程序

> ~~~java
> @Test
> public void testSimpleFactory(){
>     ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
>     Vip vip = applicationContext.getBean("vipBean", Vip.class);
>     System.out.println(vip);
> }
> ~~~
>
> 

### 7.3 通过factory-bean实例化（工厂方法模式）

Spring提供的实例化方式，第三种：通过工厂方法模式，通过factory-bean属性 和 factory-method 属性来共同完成

告诉Spring框架，调用哪个对象的哪个方法来获取Bean。

factory-bean属性 告诉spring调用哪个对象，  factory-method 属性告诉spring调用该对象的哪个方法

第一步：定义一个Bean

> ~~~java
> package com.powernode.spring6.bean;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className Order
>  * @since 1.0
>  **/
> public class Order {
> }
> 
> ~~~

第二步：定义具体工厂类，工厂类中定义实例方法

> 普通方法
>
> ~~~java
> package com.powernode.spring6.bean;
> 
> public class OrderFactory {
>     public Order get(){
>         return new Order();
>     }
> }
> 
> ~~~

第三步：在Spring配置文件中指定factory-bean以及factory-method

> ~~~java
> <bean id="orderFactory" class="com.powernode.spring6.bean.OrderFactory"/>
> <bean id="orderBean" factory-bean="orderFactory" factory-method="get"/>
> ~~~

第四步：编写测试程序

> ~~~java
> @Test
> public void testSelfFactoryBean(){
>     ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
>     Order orderBean = applicationContext.getBean("orderBean", Order.class);
>     System.out.println(orderBean);
> }
> ~~~



### 7.4 通过FactoryBean 接口实例化

以上的第三种方式中，factory-bean是我们自定义的，factory-method也是我们自己定义的。

在Spring中，当你编写的类直接    <u>实现FactoryBean接口</u>  之后，factory-bean不需要指定了，factory-method也不需要指定了。

factory-bean会自动指向实现FactoryBean接口的类，factory-method会自动指向getObject()方法。

第一步：定义一个Bean

> ~~~java
> package com.powernode.spring6.bean;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className Person
>  * @since 1.0
>  **/
> public class Person {
> }
> 
> ~~~

第二步：编写一个类实现FactoryBean接口

> ~~~java
> package com.powernode.spring6.bean;
> 
> import org.springframework.beans.factory.FactoryBean;
> 
> 
> public class PersonFactoryBean implements FactoryBean<Person> {
> 
>     @Override
>     public Person getObject() throws Exception {
>         return new Person();
>     }
> 
>     @Override
>     public Class<?> getObjectType() {
>         return null;
>     }
> 
>     @Override
>     public boolean isSingleton() {
>         // true表示单例
>         // false表示原型
>         return true;
>     }
> }
> 
> ~~~

第三步：在Spring配置文件中配置FactoryBean

> ~~~xml
> <bean id="personBean" class="com.powernode.spring6.bean.PersonFactoryBean"/>
> ~~~

测试程序：

> ~~~java
> @Test
> public void testFactoryBean(){
>     ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
>     Person personBean = applicationContext.getBean("personBean", Person.class);
>     System.out.println(personBean);
> 
>     Person personBean2 = applicationContext.getBean("personBean", Person.class);
>     System.out.println(personBean2);
> }
> ~~~
>
> 

### 7.5 BeanFactory 和 FactoryBean 的区别

#### 7.5.1 BeanFactory 

Spring IoC容器的顶级对象，BeanFactory被翻译为“Bean工厂”，在Spring的IoC容器中，“Bean工厂”负责创建Bean对象。

BeanFactory是工厂。

#### 7.5.2 FactoryBean 

FactoryBean：它是一个Bean，是一个能够**辅助Spring**实例化其它Bean对象的一个Bean。

在Spring中，Bean可以分为两类：

- 第一类：普通Bean
- 第二类：工厂Bean（记住：工厂Bean也是一种Bean，只不过这种Bean比较特殊，它可以辅助Spring实例化其它Bean对象。）

#### 7.6 注入自定义Date

java.util.Date 在Spring当中被当做简单类型，但是简单类型的话，注入的日期字符串格式有要求

java.util.Date 在Spring中也可以被当做非简单类型。

我们前面说过，java.util.Date 在Spring中被当做简单类型，简单类型在注入的时候可以直接使用value属性或value标签来完成。但我们之前已经测试过了，对于Date类型来说，采用value属性或value标签赋值的时候，对日期字符串的格式要求非常严格，必须是这种格式的：Mon Oct 10 14:30:26 CST 2022。其他格式是不会被识别的。

使用FactoryBean来完成

> 编写Student类，其中有个Date类型属性：
>
> ~~~java
> package com.powernode.spring6.bean;
> 
> import java.util.Date;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className Student
>  * @since 1.0
>  **/
> public class Student {
>     private Date birth;
> 
>     public void setBirth(Date birth) {
>         this.birth = birth;
>     }
> 
>     @Override
>     public String toString() {
>         return "Student{" +
>                 "birth=" + birth +
>                 '}';
>     }
> }
> 
> 
> ~~~
>
> 编写DateFactoryBean实现FactoryBean接口：
>
> ~~~java
> package com.powernode.spring6.bean;
> 
> import org.springframework.beans.factory.FactoryBean;
> 
> import java.text.SimpleDateFormat;
> import java.util.Date;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className DateFactoryBean
>  * @since 1.0
>  **/
> public class DateFactoryBean implements FactoryBean<Date> {
> 
>     // 定义属性接收日期字符串
>     private String date;
> 
>     // 通过构造方法给日期字符串属性赋值
>     public DateFactoryBean(String date) {
>         this.date = date;
>     }
> 
>     @Override
>     public Date getObject() throws Exception {
>         SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
>         return sdf.parse(this.date);
>     }
> 
>     @Override
>     public Class<?> getObjectType() {
>         return null;
>     }
> }
> 
> ~~~
>
> 编写spring配置文件：
>
> ~~~xml
> <bean id="dateBean" class="com.powernode.spring6.bean.DateFactoryBean">
>   <constructor-arg name="date" value="1999-10-11"/>
> </bean>
> 
> <bean id="studentBean" class="com.powernode.spring6.bean.Student">
>   <property name="birth" ref="dateBean"/>
> </bean>
> ~~~

## 八、Bean的生命周期

### 8.1 什么是Bean的生命周期

> Spring其实就是一个管理Bean对象的工厂。它负责对象的创建，对象的销毁等。
>
> 所谓的生命周期就是：对象从创建开始到最终销毁的整个过程。
>
> 什么时候创建Bean对象？
>
> 创建Bean对象的前后会调用什么方法？
>
> Bean对象什么时候销毁？
>
> Bean对象的销毁前后调用什么方法？

### 8.2 为什么要知道Bean的生命周期

> 其实生命周期的本质是：在哪个时间节点上调用了哪个类的哪个方法。
>
> 我们需要充分的了解在这个生命线上，都有哪些特殊的时间节点。
>
> 只有我们知道了特殊的时间节点都在哪，到时我们才可以确定代码写到哪。
>
> 我们可能需要在某个特殊的时间点上执行一段特定的代码，这段代码就可以放到这个节点上。当生命线走到这里的时候，自然会被调用。

### 8.3 Bean的生命周期之五步

Bean生命周期的管理，可以参考Spring的源码：**AbstractAutowireCapableBeanFactory类的doCreateBean()方法****。**

Bean生命周期可以粗略的划分为五大步：

> - 第一步：实例化Bean（调用无参构造方法）
> - 第二步：Bean属性赋值（调用set方法）
> - 第三步：初始化Bean（会调用Bean的init方法。注意：这个init方法需要自己写，自己配）
> - 第四步：使用Bean
> - 第五步：销毁Bean（会调用Bean的destroy方法。注意：这个destroy方法需要自己写，字节配）

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665388735200-444405f6-283d-4b3a-8cdf-8c3e01743618.png)

定义一个Bean：

~~~java
package com.powernode.spring6.bean;


public class User {
    private String name;

    public User() {
        System.out.println("1.实例化Bean");
    }

    public void setName(String name) {
        this.name = name;
        System.out.println("2.Bean属性赋值");
    }

    public void initBean(){
        System.out.println("3.初始化Bean");
    }

    public void destroyBean(){
        System.out.println("5.销毁Bean");
    }

}

~~~

Spring 配置文件：

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
    init-method属性指定初始化方法。
    destroy-method属性指定销毁方法。
    -->
    <bean id="userBean" class="com.powernode.spring6.bean.User" init-method="initBean" destroy-method="destroyBean">
        <property name="name" value="zhangsan"/>
    </bean>

</beans>
~~~

测试程序:

~~~java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.User;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;


public class BeanLifecycleTest {
    @Test
    public void testLifecycle(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        User userBean = applicationContext.getBean("userBean", User.class);
        System.out.println("4.使用Bean");
        // 只有正常关闭spring容器才会执行销毁方法
        ClassPathXmlApplicationContext context = (ClassPathXmlApplicationContext) applicationContext;
        context.close();
    }
}

~~~

执行结果：![image-20230828164000937](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230828164000937.png)

需要注意的：

- 第一：只有正常关闭spring容器，bean的销毁方法才会被调用。
- 第二：ClassPathXmlApplicationContext类才有close()方法。
- 第三：配置文件中的init-method指定初始化方法。destroy-method指定销毁方法。

### 8.4 Bean生命周期之7步

在以上的5步中，第3步是初始化Bean，如果你还想在初始化前和初始化后添加代码，可以加入“Bean后处理器”。

编写一个类实现BeanPostProcessor类，并且重写before和after方法：

~~~java
package com.powernode.spring6.bean;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

/**
 * @author 动力节点
 * @version 1.0
 * @className LogBeanPostProcessor
 * @since 1.0
 **/
public class LogBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("Bean后处理器的before方法执行，即将开始初始化");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("Bean后处理器的after方法执行，已完成初始化");
        return bean;
    }
}

~~~

在spring.xml文件中配置“Bean后处理器”：

~~~xml
<!--配置Bean后处理器。这个后处理器将作用于当前配置文件中所有的bean。-->
<bean class="com.powernode.spring6.bean.LogBeanPostProcessor"/>
~~~

**一定要注意：在spring.xml文件中配置的Bean后处理器将作用于当前配置文件中所有的Bean。**

如果加上Bean后处理器的话，Bean的生命周期就是7步了：

![img](C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\1665393936765-0ea5dcdd-859a-4ac5-9407-f06022c498b9.png)

### 8.5 Bean生命周期之10步

如果根据源码跟踪，可以划分更细腻的步骤，10步：

<img src="C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\1665394697870-15de433a-8d50-4b31-9b75-b2ca7090c1c6.png" alt="image.png" style="zoom: 80%;" />

 上图中检查Bean是否实现了Aware的相关接口是什么意思？

Aware相关的接口包括：BeanNameAware、BeanClassLoaderAware、BeanFactoryAware

- 当Bean实现了BeanNameAware，Spring会将Bean的名字传递给Bean。
- 当Bean实现了BeanClassLoaderAware，Spring会将加载该Bean的类加载器传递给Bean。
- 当Bean实现了BeanFactoryAware，Spring会将Bean工厂对象传递给Bean。

测试以上10步，可以让User类实现5个接口，并实现所有方法：

- BeanNameAware
- BeanClassLoaderAware
- BeanFactoryAware
- InitializingBean
- DisposableBean

代码如下：

user 类实现BeanNameAware, BeanClassLoaderAware, BeanFactoryAware, InitializingBean, DisposableBean接口

~~~java
package com.powernode.spring6.bean;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.*;

/**
 * @author 动力节点
 * @version 1.0
 * @className User
 * @since 1.0
 **/
public class User implements BeanNameAware, BeanClassLoaderAware, BeanFactoryAware, InitializingBean, DisposableBean {
    private String name;

    public User() {
        System.out.println("1.实例化Bean");
    }

    public void setName(String name) {
        this.name = name;
        System.out.println("2.Bean属性赋值");
    }

    public void initBean(){
        System.out.println("6.初始化Bean");
    }

    public void destroyBean(){
        System.out.println("10.销毁Bean");
    }

    @Override
    public void setBeanClassLoader(ClassLoader classLoader) {
        System.out.println("3.类加载器：" + classLoader);
    }

    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        System.out.println("3.Bean工厂：" + beanFactory);
    }

    @Override
    public void setBeanName(String name) {
        System.out.println("3.bean名字：" + name);
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("9.DisposableBean destroy");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("5.afterPropertiesSet执行");
    }
}

~~~

LogBeanPostProcessor 类：

~~~java
package com.powernode.spring6.bean;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

/**
 * @author 动力节点
 * @version 1.0
 * @className LogBeanPostProcessor
 * @since 1.0
 **/
public class LogBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("4.Bean后处理器的before方法执行，即将开始初始化");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("7.Bean后处理器的after方法执行，已完成初始化");
        return bean;
    }
}

~~~

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665395466500-d95b1a58-24e1-46f0-b72a-aa7764b0336a.png)

### 8.6 Bean的作用域不同，管理方式不同

Spring容器只对singleton 的Bean进行完整的生命周期管理。

如果是prototype 作用域的Bean，Spring容器只负责将该Bean初始化完毕。等客户端程序一旦获得该Bean之后，Spring容器就不再管理该对象的生命周期了。

Spring 根据Bean的作用域来选择管理方式。

> - 对于singleton作用域的Bean，Spring 能够精确地知道该Bean何时被创建，何时初始化完成，以及何时被销毁；
> - 而对于 prototype 作用域的 Bean，Spring 只负责创建，当容器创建了 Bean 的实例后，Bean 的实例就交给客户端代码管理，Spring 容器将不再跟踪其生命周期。

### 8.7 自己new的对象如何让Spring管理

有些时候可能会遇到这样的需求，某个java对象是我们自己new的，然后我们希望这个对象被Spring容器管理，怎么实现？

User 类：

~~~java
package com.powernode.spring6.bean;

public class User {
}

~~~

RegisterBeanTest 测试类：

~~~java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.User;
import org.junit.Test;
import org.springframework.beans.factory.support.DefaultListableBeanFactory;

/**
 * @author 动力节点
 * @version 1.0
 * @className RegisterBeanTest
 * @since 1.0
 **/
public class RegisterBeanTest {

    @Test
    public void testBeanRegister(){
        // 自己new的对象
        User user = new User();
        System.out.println(user);

        // 创建 默认可列表BeanFactory 对象
        DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
        // 注册Bean
        factory.registerSingleton("userBean", user);
        // 从spring容器中获取bean
        User userBean = factory.getBean("userBean", User.class);
        System.out.println(userBean);
    }
}

~~~

## 九、Bean的循环依赖问题

### 9.1 什么是Bean的循环依赖

A对象中有B属性。B对象中有A属性。这就是循环依赖。我依赖你，你也依赖我。

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665452274046-82594b87-2974-4e08-a6ab-2218d001d14f.png)

### 9.2 singleton下的set注入产生的循环依赖

编写程序，测试一下在singleton+setter的模式下产生的循环依赖，Spring是否能够解决？

Husband 丈夫类：

~~~java
package com.powernode.spring6.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className Husband
 * @since 1.0
 **/
public class Husband {
    private String name;
    private Wife wife;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setWife(Wife wife) {
        this.wife = wife;
    }

    // toString()方法重写时需要注意：不能直接输出wife，输出wife.getName()。要不然会出现递归导致的栈内存溢出错误。
    @Override
    public String toString() {
        return "Husband{" +
                "name='" + name + '\'' +
                ", wife=" + wife.getName() +
                '}';
    }
}

~~~

Wife 妻子类：

~~~java
package com.powernode.spring6.bean;

/**
 * @author 动力节点
 * @version 1.0
 * @className Wife
 * @since 1.0
 **/
public class Wife {
    private String name;
    private Husband husband;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setHusband(Husband husband) {
        this.husband = husband;
    }

    // toString()方法重写时需要注意：不能直接输出husband，输出husband.getName()。要不然会出现递归导致的栈内存溢出错误。
    @Override
    public String toString() {
        return "Wife{" +
                "name='" + name + '\'' +
                ", husband=" + husband.getName() +
                '}';
    }
}

~~~

Spring配置文件：

~~~xml

    <bean id="husbandBean" class="com.powernode.spring6.bean.Husband" scope="singleton">
        <property name="name" value="张三"/>
        <property name="wife" ref="wifeBean"/>
    </bean>
    <bean id="wifeBean" class="com.powernode.spring6.bean.Wife" scope="singleton">
        <property name="name" value="小花"/>
        <property name="husband" ref="husbandBean"/>
    </bean>
~~~

测试程序：

~~~java
@Test
    public void testSingletonAndSet(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        Husband husbandBean = applicationContext.getBean("husbandBean", Husband.class);
        Wife wifeBean = applicationContext.getBean("wifeBean", Wife.class);
        System.out.println(husbandBean);
        System.out.println(wifeBean);
    }
~~~

singleton 表示在spring容器中是单例的，是独一无二的。

在singleton + setter 模式下，为什么循环依赖不会出现问题？

> 主要原因是：在这种模式下，Spring对Bean的管理主要分为清晰的两个阶段：
>
> > 第一阶段：在Spring容器加载的时候，实例化Bean，只要其中任意一个Bean实例化之后，马上进行 “ 曝光 ”【不等属性赋值就曝光】
> >
> > 第二阶段：曝光之后，再进行属性的赋值。
>
> 核心解决方案：实例化对象和对象的属性赋值分两个阶段来完成
>
> 注意：只有在 singleton + setter 模式 的情况下，Bean才会采取提前“ 曝光 ”的措施

### 9.3 prototype下的set注入产生的循环依赖

prototype + setter 模式下的循环依赖，存在问题，会出现异常。

但是，只有当两个Bean的scope都是prototype的时候，才会出现异常，如果其中任意一个是 singleton ，就不会出异常。

执行测试程序：发生了异常，异常信息如下：

![image-20230829101511052](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230829101511052.png)

为什么两个Bean都是prototype时会出错呢？

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665454469042-69668f45-5d71-494f-8537-18142d354abd.png)

### 9.4 singleton下的构造注入产生的循环依赖

基于构造注入的方式产生的循环依赖也是无法解决的。

和上一个测试结果相同，都是提示产生了循环依赖，并且Spring是无法解决这种循环依赖的。

为什么呢？

**主要原因是因为通过构造方法注入导致的：因为构造方法注入会导致实例化对象的过程和*对象属性赋值的过程没有分离开，必须在一起完成导致的。**

------

### 9.5 Spring解决循环依赖的机理 



Spring为什么可以解决set + singleton模式下循环依赖？

根本的原因在于：这种方式可以做到将“实例化Bean”和“给Bean属性赋值”这两个动作分开去完成。

实例化Bean的时候：调用无参数构造方法来完成。**此时可以先不给属性赋值，可以提前将该Bean对象“曝光”给外界。**

给Bean属性赋值的时候：调用setter方法来完成。

两个步骤是完全可以分离开去完成的，并且这两步不要求在同一个时间点上完成。

也就是说，Bean都是单例的，我们可以先把所有的单例Bean实例化出来，放到一个集合当中（我们可以称之为缓存），所有的单例Bean全部实例化完成之后，以后我们再慢慢的调用setter方法给属性赋值。这样就解决了循环依赖的问题。

那么在Spring框架底层源码级别上是如何实现的呢？请看：

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665456331018-18c45ae3-fa4c-4cd8-aabf-d9bace567693.png)

在以上类中包含三个重要的属性：

> ***Cache of singleton objects: bean name to bean instance.\*** **单例对象的缓存：key存储bean名称，value存储Bean对象【一级缓存】**
>
> ***Cache of early singleton objects: bean name to bean instance.\*** **早期单例对象的缓存：key存储bean名称，value存储早期的Bean对象【二级缓存】**
>
> ***Cache of singleton factories: bean name to ObjectFactory.\*** **单例工厂缓存：key存储bean名称，value存储该Bean对应的ObjectFactory对象【三级缓存】**

这三个缓存其实本质上是三个Map集合。

我们再来看，在该类中有这样一个方法addSingletonFactory()，这个方法的作用是：将创建Bean对象的ObjectFactory对象提前曝光。

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665460724682-2222366d-cc07-43db-a8d0-fb27712b20a4.png)

再分析下面的源码：

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665460240687-3d0794c4-e6ed-4653-9463-767a7f943ff9.png)

从源码中可以看到，spring会先从一级缓存中获取Bean，如果获取不到，则从二级缓存中获取Bean，如果二级缓存还是获取不到，则从三级缓存中获取之前曝光的ObjectFactory对象，通过ObjectFactory对象获取Bean实例，这样就解决了循环依赖的问题。

**总结：**

​	**Spring只能解决setter方法注入的单例bean之间的循环依赖。ClassA依赖ClassB，ClassB又依赖ClassA，形成依赖闭环。Spring在创建ClassA对象后，不需要等给属性赋值，直接将其曝光到bean缓存当中。在解析ClassA的属性时，又发现依赖于ClassB，再次去获取ClassB，当解析ClassB的属性时，又发现需要ClassA的属性，但此时的ClassA已经被提前曝光加入了正在创建的bean的缓存中，则无需创建新的的ClassA的实例，直接从缓存中获取即可。从而解决循环依赖问题。**

## 十、回顾反射机制

调用一个方法，一般涉及到4个要素：

> - 调用哪个对象的（systemService）
> - 哪个方法（login）
> - 传什么参数（"admin", "admin123"）
> - 返回什么值（success）

### 10.1 回顾

1.获取类

> 要获取方法Method，首先你需要获取这个类Class:
>
> ~~~java
> Class clazz = Class.forName("com.powernode.reflect.SystemService");
> ~~~

2.获取方法

> 当拿到Class之后，调用getDeclaredMethod()方法可以获取到方法。
>
> 获取一个方法，需要告诉Java程序，你要获取的方法的名字是什么，这个方法上每个形参的类型是什么。这样Java程序才能给你拿到对应的方法。
>
> 这样的设计也非常合理，因为在同一个类当中，方法是支持重载的，也就是说方法名可以一样，但参数列表一定是不一样的，所以获取一个方法需要提供方法名以及每个形参的类型。
>
> 假如你要获取这个方法：login(String username, String password)
>
> ~~~java
> Method loginMethod = clazz.getDeclaredMethod("login", String.class, String.class);
> ~~~
>
> 假如你要获取到这个方法：login(String password)
>
> ~~~java
> Method loginMethod = clazz.getDeclaredMethod("login", String.class);
> ~~~

3.调用方法

> 获取到方法之后，需要给方法传递参数，并且告诉jvm这个方法是哪个对象的。
>
> 通过无参构造方法，获取对象：
>
> ~~~java
> Constructor<?> con = clazz.getDeclaredConstructor();
> Object obj = con.newInstance();
> 
> //或者使用已经过时的方式获取对象
> clazz.newInstance();
> ~~~
>
> 给方法传递参数以及方法对应的对象：
>
> ~~~java
> loginMethod.invoke(obj,"张三",250)
> ~~~

4.返回值

> invoke方法会自动返回一个Object对象
>
> ~~~java
> Object retValue =loginMethod.invoke(obj,"张三",250)
> ~~~

### 10.2 假设知道属性名

假设已知一下信息：

> 有这样一个类，类名叫做：com.powernode.reflect.User
>
> 这个类符合javabean规范，属性私有化，对外提供getter和setter方法
>
> 还知道这个类中有个属性，属性名叫 age
>
> 知道 age 属性的类型为 int 类型

使用反射机制调用set方法，给User对象的age属性赋值：

~~~java
@Test
    public void userTest() throws Exception {
        String className = "com.fengshun.dependency.pojo.User";
        String propertyName ="age";
        // 获取类
        Class<?> aClass = Class.forName(className);
        // 根据属性名，获取属性
        Field age = aClass.getDeclaredField(propertyName);
        // 获取方法名
        String methodName = "set"+propertyName.toUpperCase().charAt(0)+propertyName.substring(1);
        // 获取方法
        Method declaredMethod = aClass.getDeclaredMethod(methodName, age.getType());
        // 获取构造方法
        Constructor<?> constructor = aClass.getDeclaredConstructor();
        // 获取对象
        Object obj = constructor.newInstance();
        // 调用方法
        declaredMethod.invoke(obj,18);
        System.out.println(obj);
    }
~~~

获取类中的属性：

~~~java
Field age = aClass.getDeclaredField("age");
~~~

获取属性的类型：

~~~
age.getType()
~~~

## 十一、手写Spring框架



```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
CustomerService customerServiceBean = applicationContext.getBean("customerServiceBean", CustomerService.class);
```

思路:

> 在spring框架中通过 ApplicationContext 接口获取bean对象 以及 ClassPathXmlApplicationContext类解析xml文件并且初始化bean，所以我们在手写spring框架时也需要定义这两个类。
>
> ClassPathXmlApplicationContext 被new的时候，就已经把bean对象创建出来并初始化了，这一系列操作都是在构造方法中完成的。

步骤：

> 1. 先解析 xml 文件，获取bean的id 和 class 属性的值，根据这两个值，通过反射机制创建对象，并存到map集合中
> 2. 获取bean的子标签 property 标签中的name属性以及value属性或者ref属性的值，根据这些值，通过反射机制，调用其set方法，为属性赋值

ApplicationContext 接口：

~~~java
public interface ApplicationContext {
    // 根据Bean的名称获取bean对象
    public Object getBean(String beanName);

}
~~~

ClassPathXmlApplicationContext 类：

~~~java
package org.myspringframework.core;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.Node;
import org.dom4j.io.SAXReader;

import java.lang.reflect.Constructor;
import java.lang.reflect.Method;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * @author 动力节点
 * @version 1.0
 * @className ClassPathXmlApplicationContext
 * @since 1.0
 **/
public class ClassPathXmlApplicationContext implements ApplicationContext{
    /**
     * 存储bean的Map集合
     */
    private Map<String,Object> beanMap = new HashMap<>();

    /**
     * 在该构造方法中，解析myspring.xml文件，创建所有的Bean实例，并将Bean实例存放到Map集合中。
     * @param resource 配置文件路径（要求在类路径当中）
     */
    public ClassPathXmlApplicationContext(String resource) {
        try {
            SAXReader reader = new SAXReader();
            Document document = reader.read(ClassLoader.getSystemClassLoader().getResourceAsStream(resource));
            // 获取所有的bean标签
            List<Node> beanNodes = document.selectNodes("//bean");
            // 遍历集合（这里的遍历只实例化Bean，不给属性赋值。为什么要这样做？）
            beanNodes.forEach(beanNode -> {
                Element beanElt = (Element) beanNode;
                // 获取id
                String id = beanElt.attributeValue("id");
                // 获取className
                String className = beanElt.attributeValue("class");
                try {
                    // 通过反射机制创建对象
                    Class<?> clazz = Class.forName(className);
                    Constructor<?> defaultConstructor = clazz.getDeclaredConstructor();
                    Object bean = defaultConstructor.newInstance();
                    // 存储到Map集合
                    beanMap.put(id, bean);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            });
            // 再重新遍历集合，这次遍历是为了给Bean的所有属性赋值。
            // 思考：为什么不在上面的循环中给Bean的属性赋值，而在这里再重新遍历一次呢？
            // 通过这里你是否能够想到Spring是如何解决循环依赖的：实例化和属性赋值分开。
            beanNodes.forEach(beanNode -> {
                Element beanElt = (Element) beanNode;
                // 获取bean的id
                String beanId = beanElt.attributeValue("id");
                // 获取所有property标签
                List<Element> propertyElts = beanElt.elements("property");
                // 遍历所有属性
                propertyElts.forEach(propertyElt -> {
                    try {
                        // 获取属性名
                        String propertyName = propertyElt.attributeValue("name");
                        // 获取属性类型
                        Class<?> propertyType = beanMap.get(beanId).getClass().getDeclaredField(propertyName).getType();
                        // 获取set方法名
                        String setMethodName = "set" + propertyName.toUpperCase().charAt(0) + propertyName.substring(1);
                        // 获取set方法
                        Method setMethod = beanMap.get(beanId).getClass().getDeclaredMethod(setMethodName, propertyType);
                        // 获取属性的值，值可能是value，也可能是ref。
                        // 获取value
                        String propertyValue = propertyElt.attributeValue("value");
                        // 获取ref
                        String propertyRef = propertyElt.attributeValue("ref");
                        Object propertyVal = null;
                        if (propertyValue != null) {
                            // 该属性是简单属性
                            String propertyTypeSimpleName = propertyType.getSimpleName();
                            switch (propertyTypeSimpleName) {
                                case "byte": case "Byte":
                                    propertyVal = Byte.valueOf(propertyValue);
                                    break;
                                case "short": case "Short":
                                    propertyVal = Short.valueOf(propertyValue);
                                    break;
                                case "int": case "Integer":
                                    propertyVal = Integer.valueOf(propertyValue);
                                    break;
                                case "long": case "Long":
                                    propertyVal = Long.valueOf(propertyValue);
                                    break;
                                case "float": case "Float":
                                    propertyVal = Float.valueOf(propertyValue);
                                    break;
                                case "double": case "Double":
                                    propertyVal = Double.valueOf(propertyValue);
                                    break;
                                case "boolean": case "Boolean":
                                    propertyVal = Boolean.valueOf(propertyValue);
                                    break;
                                case "char": case "Character":
                                    propertyVal = propertyValue.charAt(0);
                                    break;
                                case "String":
                                    propertyVal = propertyValue;
                                    break;
                            }
                            setMethod.invoke(beanMap.get(beanId), propertyVal);
                        }
                        if (propertyRef != null) {
                            // 该属性不是简单属性
                            setMethod.invoke(beanMap.get(beanId), beanMap.get(propertyRef));
                        }
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                });
            });

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public Object getBean(String beanId) {
        return beanMap.get(beanId);
    }
}

~~~

## 十二、Spring IoC注解式开发

### 12.1 回顾注解

1.自定义注解

~~~java
package com.powernode.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

//标注注解的注解叫做元注解，@Target标记@Component 可以出现的位置
//大括号内可以有多个ElementType，ElementType.TYPE 表示类或接口 ElementTypr.FIELD 表示方法
@Target(value = {ElementType.TYPE})
//使用某个注解，如果注解的属性名是value的话，value可以省略。
//如果注解的值是数组，并且只有一个元素时，大括号可以省略
@Retention(value = RetentionPolicy.RUNTIME)
public @interface Component {
    String value();
}
~~~

该注解上面修饰的注解包括：Target注解和Retention注解，这两个注解被称为元注解。

Target注解用来设置Component注解可以出现的位置，以上代表表示Component注解只能用在类和接口上。

Retention注解用来设置Component注解的保持性策略，用来标注@Component 注解最终保留在class文件当中，以上代表Component注解可以被反射机制读取。

2. 使用注解

   ~~~java
   package com.powernode.bean;
   
   import com.powernode.annotation.Component;
   
   //@Component(value = "userBean")
   @Component("userBean")
   public class User {
   }
   
   ~~~

3. 通过反射机制读取注解

   ~~~java
   //获取类
   ClaSS<?> aClass = Class.forName("com.fengshun.bean.User");
   //判断类上面有没有这个注解
   if(aClass.isAnnotationPresent(Component.class)){
       //获取类上的注解
       Component annotation = aClass.getAnnotation(Component.class);
       //访问注解的属性
       System.out.println(annotation.value);
                                              
   }

4. 假设我们现在只知道包名：com.powernode.bean。至于这个包下有多少个Bean我们不知道。哪些Bean上有注解，哪些Bean上没有注解，这些我们都不知道，如何通过程序全自动化判断。

   ~~~java
    @Test
       public void testAnnotation() {
           Map<String,Object> map = new HashMap<>();
   
           // 包名
           String packageName = "com.fengshun.test";
           // 替换为相对路径
           String packagePath = packageName.replaceAll("\\.", "/");
           URL url = ClassLoader.getSystemClassLoader().getResource(packagePath);
           // 获取绝对路径
           String path = url.getPath();
           System.out.println(path);
           // 获取绝对路径下的所有文件
           File file = new File(path);
           File[] files = file.listFiles();
           Arrays.stream(files).forEach(f -> {
               try {
                   // 获取类名
                   String className = packageName + "." + f.getName().split("\\.")[0];
                   Class<?> aClass = Class.forName(className);
                   if (aClass.isAnnotationPresent(Component.class)) {
                       // 获取注解
                       Component annotation = aClass.getAnnotation(Component.class);
                       // 获取注解上的value值
                       String value = annotation.value();
                       // 有这个注解的都要创建对象
                       Constructor<?> constructor = aClass.getDeclaredConstructor();
                       Object obj = constructor.newInstance();
                       // 将这个对象存到map集合中
                       map.put(value,obj);
                   }
               } catch (Exception e) {
                   e.printStackTrace();
               }
           });
   
       }
   ~~~

   


### 12.2 声明Bean的注解

负责声明Bean的注解，常见的包括四个：

- @Component      (组件)
- @Controller       （控制）
- @Service          （service）
- @Repository     （dao）

以上四个组件任意一个都可以

源码分许：

> @Component   
>
> ~~~java
> package com.powernode.annotation;
> 
> import java.lang.annotation.ElementType;
> import java.lang.annotation.Retention;
> import java.lang.annotation.RetentionPolicy;
> import java.lang.annotation.Target;
> 
> @Target(value = {ElementType.TYPE})
> @Retention(value = RetentionPolicy.RUNTIME)
> public @interface Component {
>     String value();
> }
> 
> ~~~
>
> @Controller
>
> ~~~java
> 
> package org.springframework.stereotype;
> 
> import java.lang.annotation.Documented;
> import java.lang.annotation.ElementType;
> import java.lang.annotation.Retention;
> import java.lang.annotation.RetentionPolicy;
> import java.lang.annotation.Target;
> import org.springframework.core.annotation.AliasFor;
> 
> @Target({ElementType.TYPE})
> @Retention(RetentionPolicy.RUNTIME)
> @Documented
> @Component
> public @interface Controller {
>     @AliasFor(
>         annotation = Component.class
>     )
>     String value() default "";
> }
> 
> ~~~
>
> @Service
>
> ~~~java
> package org.springframework.stereotype;
> 
> import java.lang.annotation.Documented;
> import java.lang.annotation.ElementType;
> import java.lang.annotation.Retention;
> import java.lang.annotation.RetentionPolicy;
> import java.lang.annotation.Target;
> import org.springframework.core.annotation.AliasFor;
> 
> @Target({ElementType.TYPE})
> @Retention(RetentionPolicy.RUNTIME)
> @Documented
> @Component
> public @interface Service {
>     @AliasFor(
>         annotation = Component.class
>     )
>     String value() default "";
> }
> 
> ~~~
>
> @Repository
>
> ~~~java
> package org.springframework.stereotype;
> 
> import java.lang.annotation.Documented;
> import java.lang.annotation.ElementType;
> import java.lang.annotation.Retention;
> import java.lang.annotation.RetentionPolicy;
> import java.lang.annotation.Target;
> import org.springframework.core.annotation.AliasFor;
> 
> @Target({ElementType.TYPE})
> @Retention(RetentionPolicy.RUNTIME)
> @Documented
> @Component
> public @interface Repository {
>     @AliasFor(
>         annotation = Component.class
>     )
>     String value() default "";
> }
> 
> ~~~

@Component  注解是最主要的，其他三个都是它的别名。这样能够提高代码的可读性。

### 12.3 Spring注解的使用

- 第一步：加入aop的依赖

  > 只需要引入spring-context 依赖·，mvc会自动帮我们引入aop 依赖
  >
  > ![image-20230830104249542](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230830104249542.png)

- 第二步：在配置文件中添加context命名空间

  > ![image-20230830104437532](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230830104437532.png)

- 第三步：在配置文件中指定扫描的包

  ~~~xml
  <!--    给spring框架指定要扫描哪些包中的类-->
      <context:component-scan base-package="com.fengshun.bean"/>
  ~~~

- 第四步：在Bean类上使用注解

  > ~~~java
  > @Component("userBean")
  > // @Component(value = "userBean")
  > public class User {
  > }
  > 
  > ~~~
  >
  > value 就是 bean 的 id
  > 如果不指定value 的值，也可以使用，spring会将类名的首字母小写的形式作为value，例如  User —> user

  

如果是多个包下的类需要注入：

> 第一种：在配置文件中指定多个包，用逗号隔开
>
> ~~~xml
>     <context:component-scan base-package="com.fengshun.bean,com.fengshun.service"/>
> ~~~
>
> 第二种：指定多个包的父包
>
> 但是这肯定要牺牲一部分效率。
>
> ~~~xml
>     <context:component-scan base-package="com.fengshun"/>
> ~~~

  

### 12.4 选择性实例化Bean

假设在某个包下有很多Bean，有的Bean上标注了Component，有的标注了Controller，有的标注了Service，有的标注了Repository，现在由于某种特殊业务的需要，只允许其中所有的Controller参与Bean管理，其他的都不实例化。这应该怎么办呢？

类集合：

~~~java
package com.powernode.spring6.bean3;

import org.springframework.stereotype.Component;
import org.springframework.stereotype.Controller;
import org.springframework.stereotype.Repository;
import org.springframework.stereotype.Service;

@Component
public class A {
    public A() {
        System.out.println("A的无参数构造方法执行");
    }
}

@Controller
class B {
    public B() {
        System.out.println("B的无参数构造方法执行");
    }
}

@Service
class C {
    public C() {
        System.out.println("C的无参数构造方法执行");
    }
}

@Repository
class D {
    public D() {
        System.out.println("D的无参数构造方法执行");
    }
}

@Controller
class E {
    public E() {
        System.out.println("E的无参数构造方法执行");
    }
}

@Controller
class F {
    public F() {
        System.out.println("F的无参数构造方法执行");
    }
}
~~~

spring.xml 文件中指定：

> 第一种方式：
>
> 
>
> > ~~~xml
> >     <context:component-scan base-package="com.powernode.spring6.bean3" use-default-filters="false">
> >         <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
> >     </context:component-scan>
> > ~~~
> >
> > **use-default-filters="false"** 这个属性是false 表示这个包下所有带有声明bean的注解全部失效.
> >
> > **context:include-filter** 标签代表的是让某个注解生效
> >
> > **type="annotation"** 表示类型为注解
> >
> > **expression** 属性的值必须是全限定类名。expression="org.springframework.stereotype.Controller" 的意思是只有@Controller 这个注解才能使用。
> >
> > 含义：先让所有注解失效，最后让某个注解生效。
>
> 第二种方式：
>
> > **use-default-filters="true"** 如果这个属性的值是true，表示某个包下的所有带有声明bean的注解全部生效。 默认值为true，可以不用写。
> >
> > ~~~xml
> >     <context:component-scan base-package="com.fengshun.bean2" use-default-filters="true">
> >         <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
> >     </context:component-scan>
> > ~~~
> >
> > **context:exclude-filter** 代表让某个注解失效。
> >
> >  含义：先让所有注解生效，然后让某个注解失效。



### 12.5 负责注入的注解

@Component @Controller @Service @Repository 这四个注解是用来声明Bean的，声明后这些Bean将被实例化。接下来我们看一下，如何给Bean的属性赋值。给Bean属性赋值需要用到这些注解：

- @Value
- @Autowired
- @Qualifier
- @Resource

#### 12.5.1 @Value

这个注解是专门用来注入简单类型的，用来代替 < property name="" value=""/>

注意：

> 首先要将类声明为bean，其次在属性上添加@Value 注解。
>
> 使用Value注解注入的话，可以用在属性上，并且可以不需要set方法。

~~~java
@Component()
public class User {
    @Value(value = "张三")
    private String name;
    
    @Value(value = "18")
    private int age;

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
~~~

开启包扫描：

~~~xml
<context:component-scan base-package="com.fengshun.spring6.bean2"/>
~~~

测试代码：

~~~java
@Test
public void testValue(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-injection.xml");
    Object user = applicationContext.getBean("user");
    System.out.println(user);
}
~~~



@Value注解也可以用在set方法上：

> ~~~java
> @Component()
> public class User {
>     
>     private String name;
>     
>     private int age;
>     
>     @Value(value = "张三")
>     public void setName(String name) {
>         this.name = name;
>     }
>     
>     @Value(value = "18")
>     public void setAge(int age) {
>         this.age = age;
>     }
> }
> ~~~

@Value 注解还能加到构造方法上：

> ~~~JAVA
> 
> @Component()
> public class User {
>     private String name;
>     private int age;
> 
>     public User() {
>     }
> 
>     public User(@Value("张三") String name, @Value("15") int age) {
>         this.name = name;
>         this.age = age;
>     }
> }
> 
> ~~~

#### 12.5.2 @Autowired与@Qualifier

@Autowired注解可以用来注入**非简单类型**。被翻译为：自动连线的，或者自动装配。

单独使用@Autowired注解，**默认根据类型装配**。【默认是byType】

根据名字进行自动装配的话，需要 @Autowired 与 @Qualifier 联合使用，如果想要根据类型自动装配，可以单独使用 @Autowired 注解。

@Autowired源码

~~~java
package org.springframework.beans.factory.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Autowired {
	boolean required() default true;
}
~~~

源码中有两处需要注意：

- 第一处：该注解可以标注在哪里？

- - 构造方法上
  - 方法上
  - 形参上
  - 属性上
  - 注解上

- 第二处：该注解有一个required属性，默认值是true，表示在注入的时候要求被注入的Bean必须是存在的，如果不存在则报错。如果required属性设置为false，表示注入的Bean存在或者不存在都没关系，存在的话就注入，不存在的话，也不报错。

@Autowired 注解使用的时候，不需要任何的属性，直接使用这个注解即可。

根据类型进行自动装配：

> 定义UserDao接口：
>
> ~~~java
> package com.fengshun.bean3.dao;
> public interface UserDao {
>     void insert();
> }
> ~~~
>
> 定义UserDaoImpl类实现UserDao接口：
>
> ~~~java
> package com.fengshun.bean3.dao;
> 
> import org.springframework.stereotype.Repository;
> 
> @Repository
> public class UserDaoImpl implements UserDao{
>     @Override
>     public void insert() {
>         System.out.println("数据库插入操作");
>     }
> }
> ~~~
>
> 定义 UserService 类：
>
> ~~~java
> package com.fengshun.bean3.service;
> 
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.stereotype.Service;
> 
> @Service
> public class UserService {
>     //在属性上注入
>     @Autowired
>     private UserDao userDao;
> 
>     void save(){
>         userDao.insert();
>     }
> }
> 
> ~~~
>
> 配置包扫描：
>
> ~~~xml
> <context:component-scan base-package="com.fengshun.bean3.dao,com.fengshun.bean3.service"/>
> ~~~
>
> 经过以上测试，set 方法 和 构造方法都没有注入，程序依然执行了。
>
> 注意：如果只根据类型进行自动注入，那么如果接口的实现类有多个，那么程序会出现问题。
>
> 如果想解决这个问题，只能根据名字进行装配。也就是@Autowired 和 @Qualifier 联合使用

根据名字进行自动装配：

> 定义UserDaoImpl类实现UserDao接口：
>
> ~~~java
> package com.fengshun.bean3;
> 
> import org.springframework.stereotype.Repository;
> 
> @Repository("userDaoBean")
> public class UserDaoImpl implements UserDao{
>     @Override
>     public void insert() {
>         System.out.println("数据库插入操作");
>     }
> }
> ~~~
>
> 定义 UserService 类：
>
> ~~~java
> package com.fengshun.bean3;
> 
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.beans.factory.annotation.Qualifier;
> import org.springframework.stereotype.Service;
> 
> @Service
> public class UserService {
>     @Autowired
>     @Qualifier("userDaoBean")
>     private UserDao userDao;
> 
>     void save(){
>         userDao.insert();
>     }
> }
> 
> ~~~

@Autowire可以出现在属性上，构造方法上，set方法上，构造方法属性上。

如果一个类中构造方法只有一个，并且构造方法上的参数和属性能够对应上， @Autowire可以省略。

#### 12.5.3 @Resource

@Resource注解也可以完成非简单类型注入。那它和@Autowired注解有什么区别？

> - @Resource注解是JDK扩展包中的，也就是说属于JDK的一部分。所以该注解是标准注解，更加具有通用性。(JSR-250标准中制定的注解类型。JSR是Java规范提案。)
> - @Autowired注解是Spring框架自己的。
> - **@Resource注解默认根据名称装配byName，未指定name时，使用属性名作为name。通过name找不到的话会自动启动通过类型byType装配。**
> - **@Autowired注解默认根据类型装配byType，如果想根据名称装配，需要配合@Qualifier注解一起用。**
> - **@Resource注解只适合用在属性上、setter方法上。**
> - @Autowired注解用在属性上、setter方法上、构造方法上、构造方法参数上。

@Resource注解属于JDK扩展包，所以不在JDK当中，需要额外引入以下依赖：

> Spring6+版本请使用这个依赖:
>
> ~~~xml
> <dependency>
>   <groupId>jakarta.annotation</groupId>
>   <artifactId>jakarta.annotation-api</artifactId>
>   <version>2.1.1</version>
> </dependency>
> ~~~
>
> spring5-版本请使用这个依赖:
>
> ~~~xml
> <dependency>
>   <groupId>javax.annotation</groupId>
>   <artifactId>javax.annotation-api</artifactId>
>   <version>1.3.2</version>
> </dependency>
> ~~~
>
> **如果你用Spring6，要知道Spring6不再支持JavaEE，它支持的是JakartaEE9。（Oracle把JavaEE贡献给Apache了，Apache把JavaEE的名字改成JakartaEE了，大家之前所接触的所有的  javax.\*  包名统一修改为  jakarta.\*包名了。）**

@Resource注解的源码如下：

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665565515435-2ad5614a-8572-4c6f-80c1-efa236dbe35f.png)

使用：

> 定义 StudentDaoImpl 类实现 StudentDao 接口：
>
> ~~~java
> @Repository("studentDaoImpl")
> public class StudentDaoImpl implements StudentDao{
>     @Override
>     public void deleteById() {
>         System.out.println("学生信息删除");
>     }
> }
> 
> ~~~
>
> 定义 StudentService 类：
>
> 使用在属性上：
>
> ~~~java
> @Service("studentService")
> public class StudentService {
>     @Resource(name = "studentDaoImpl")
>     private StudentDao studentDao;
> 
>     public void save() {
>         studentDao.deleteById();
>     }
> }
> ~~~
>
> 使用在set方法上：
>
> ~~~java
> @Service("studentService")
> public class StudentService {
> 
>     private StudentDao studentDao;
> 
>     @Resource(name = "studentDaoImpl")
>     public void setStudentDao(StudentDao studentDao) {
>         this.studentDao = studentDao;
>     }
> 
>     public void save() {
>         studentDao.deleteById();
>     }
> }
> ~~~
>
> spring 配置文件，配置包扫描：
>
> ~~~xml
> <context:component-scan base-package="com.fengshun.bean4.dao,com.fengshun.bean4.service"/>
> ~~~
>
> 测试程序：
>
> ~~~java
> public void testRsource(){
>         ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
>         StudentService studentService = applicationContext.getBean("studentService", StudentService.class);
>         studentService.save();
>     }
> ~~~

### 12.6 全注解式开发

所谓的全注解开发就是不再使用spring配置文件了。写一个配置类来代替配置文件。

编写一个配置类代替spring框架的配置文件：
~~~java
package com.fengshun.bean4;


import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan({"com.fengshun.bean4.dao", "com.fengshun.bean4.service"})
public class SpringConfig {
}

~~~

编写测试程序：不再new ClassPathXmlApplicationContext()对象了。

~~~java
@Test
public void testNoXml(){
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(Spring6Configuration.class);
    UserService userService = applicationContext.getBean("userService", UserService.class);
    userService.save();
}
~~~

## 十三、JDBCTemplate

JdbcTemplate是Spring提供的一个JDBC模板类，是对JDBC的封装，简化JDBC代码。

当然，你也可以不用，可以让Spring集成其它的ORM框架，例如：MyBatis、Hibernate等。

接下来我们简单来学习一下，使用JdbcTemplate完成增删改查。

### 13.1 环境准备

引入spring-jdbc 依赖：

~~~xml
<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>6.0.0</version>
</dependency>
<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-jdbc</artifactId>
		<version>6.0.0</version>
 </dependency>
~~~

准备实体类：

~~~java
package com.fengshun.spring6.bean;

public class Act {
    private Long id;
    private String actno;
    private Double balance;

    public Act() {
    }

    public Act(Long id, String actno, Double balance) {
        this.id = id;
        this.actno = actno;
        this.balance = balance;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public Double getBalance() {
        return balance;
    }

    public void setBalance(Double balance) {
        this.balance = balance;
    }

    @Override
    public String toString() {
        return "Act{" +
                "id=" + id +
                ", actno='" + actno + '\'' +
                ", balance=" + balance +
                '}';
    }
}


~~~

编写Spring配置文件：

> JdbcTemplate是Spring提供好的类，这类的完整类名是：org.springframework.jdbc.core.JdbcTemplate
>
> 我们怎么使用这个类呢？new对象就可以了。怎么new对象，Spring最在行了。直接将这个类配置到Spring配置文件中，纳入Bean管理即可
>
> JdbcTemplate中有一个DataSource属性，这个属性是数据源，我们都知道连接数据库需要Connection对象，而生成Connection对象是数据源负责的。所以我们需要给JdbcTemplate设置数据源属性。
>
> 所有的数据源都是要实现javax.sql.DataSource接口的。这个数据源可以自己写一个，也可以用写好的，比如：阿里巴巴的德鲁伊连接池，c3p0，dbcp等。我们这里自己先手写一个数据源。
>
> 编写数据源：
>
> ~~~java
> package com.fengshun.spring6.bean;
> 
> import javax.sql.DataSource;
> import java.io.PrintWriter;
> import java.sql.Connection;
> import java.sql.DriverManager;
> import java.sql.SQLException;
> import java.sql.SQLFeatureNotSupportedException;
> import java.util.logging.Logger;
> 
> public class MyDataSource implements DataSource {
>     private String driver;
>     private String url;
>     private String userName;
>     private String password;
> 
>     public void setDriver(String driver) {
>         this.driver = driver;
>     }
> 
>     public void setUrl(String url) {
>         this.url = url;
>     }
> 
>     public void setUserName(String userName) {
>         this.userName = userName;
>     }
> 
>     public void setPassword(String password) {
>         this.password = password;
>     }
> 
>     @Override
>     public Connection getConnection() throws SQLException {
>         try {
>             // 注册驱动
>             Class.forName(driver);
>             // 获取数据库连接对象
>             Connection connection = DriverManager.getConnection(url, userName, password);
>             return connection;
>         } catch (Exception e) {
>             e.printStackTrace();
>         }
>         return null;
>     }
> 
>     @Override
>     public Connection getConnection(String username, String password) throws SQLException {
>         return null;
>     }
> 
>     @Override
>     public PrintWriter getLogWriter() throws SQLException {
>         return null;
>     }
> 
>     @Override
>     public void setLogWriter(PrintWriter out) throws SQLException {
> 
>     }
> 
>     @Override
>     public void setLoginTimeout(int seconds) throws SQLException {
> 
>     }
> 
>     @Override
>     public int getLoginTimeout() throws SQLException {
>         return 0;
>     }
> 
>     @Override
>     public Logger getParentLogger() throws SQLFeatureNotSupportedException {
>         return null;
>     }
> 
>     @Override
>     public <T> T unwrap(Class<T> iface) throws SQLException {
>         return null;
>     }
> 
>     @Override
>     public boolean isWrapperFor(Class<?> iface) throws SQLException {
>         return false;
>     }
> }
> 
> ~~~
>
> 把这个数据源传递给JdbcTemplate。因为JdbcTemplate中有一个DataSource属性：
>
> ~~~xml
>    <bean id="dataSource" class="com.fengshun.spring6.bean.MyDataSource">
>        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
>        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
>        <property name="userName" value="root"/>
>        <property name="password" value="051727"/>
>    </bean>
> 
>     <bean id="jdbcTemplete" class="org.springframework.jdbc.core.JdbcTemplate">
>         <!--注入数据源-->
>         <property name="dataSource" ref="dataSource"/>
>     </bean>
> ~~~

### 13.2 新增

注意：无论是增、删、改、查，都是使用 jdbcTemplate.update() 方法。

~~~java
 @Test
    public void  testiInsert(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql ="insert into t_act(id,actno,balance) values (null,?,?)";
        int act003 = jdbcTemplate.update(sql, "act003", 4500.00);
        System.out.println(act003);
    }
~~~

### 13.3 更新

~~~java
 @Test
    public void testUpdate(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql = "update t_act set actno=? , balance=? where id=?";
        int act004 = jdbcTemplate.update(sql, "act004", 6450.00, 4);
        System.out.println(act004);
    }
~~~

### 13..4 删除

~~~java
@Test
    public void testDelete(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql =" delete from t_act where id=?";
        jdbcTemplate.update(sql,4);
    }
~~~

### 13.5 查询一条记录

查询一条记录需要使用 jdbcTemplate.queryForObject() 方法，并且第二个参数是 BeanPropertyRowMapper 对象

~~~java
@Test
    public void testQueryOne(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql ="select id,actno,balance from t_act where id=?";
        Act act = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(Act.class), 4);
        System.out.println(act);
    }
~~~

### 13.6 查询多个对象

查询多个对象，需要使用 jdbcTemplate.query() 方法，它会自动返回List集合。

~~~java
@Test
    public void testQuertAll(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql ="select id,actno,balance from t_act ";
        List<Act> actList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(Act.class));
        actList.forEach(act-> System.out.println(act));
    }
~~~

### 13.7 查询一个值

~~~java
    @Test
    public void  testQueryOneValue(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
        String sql= "select count(1) from t_act";
        Integer integer = jdbcTemplate.queryForObject(sql, int.class);
        System.out.println(integer);
    }
~~~

### 13..8 批量添加

 使用 jdbcTemplate.batchUpdate() 方法实现批量添加。

~~~java
@Test
public void testAddBatch(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 批量添加
    String sql = "insert into t_act(id,actno,balance) values(?,?,?)";

    Object[] objs1 = {null, "act005", 2450.00};
    Object[] objs2 = {null, "act006", 4521.00};
    Object[] objs3 = {null, "act007", 2288.00};
    List<Object[]> list = new ArrayList<>();
    list.add(objs1);
    list.add(objs2);
    list.add(objs3);

    int[] count = jdbcTemplate.batchUpdate(sql, list);
    System.out.println(Arrays.toString(count));
}
~~~

### 13.9 批量更新

~~~java
@Test
public void testUpdateBatch(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 批量修改
    String sql = "update t_act set actno = ?, balance = ? where id = ?";
    Object[] objs1 = {"act008", 1046.00, 2};
    Object[] objs2 = {"act015", 1216.00, 3};
    Object[] objs3 = {"act012", 9784.00, 4};
    List<Object[]> list = new ArrayList<>();
    list.add(objs1);
    list.add(objs2);
    list.add(objs3);

    int[] count = jdbcTemplate.batchUpdate(sql, list);
    System.out.println(Arrays.toString(count));
}
~~~

### 13..10 批量删除

~~~java
@Test
public void testDeleteBatch(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    // 批量删除
    String sql = "delete from t_act where id = ?";
    Object[] objs1 = {2};
    Object[] objs2 = {3};
    Object[] objs3 = {4};
    List<Object[]> list = new ArrayList<>();
    list.add(objs1);
    list.add(objs2);
    list.add(objs3);
    int[] count = jdbcTemplate.batchUpdate(sql, list);
    System.out.println(Arrays.toString(count));
}
~~~

### 13.11 使用回调函数

如果想写jdbc 代码，可以使用后 callback 回调函数

~~~java
@Test
public void testCallback(){
    // 获取JdbcTemplate对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    String sql = "select id, real_name, age from t_user where id = ?";

    User user = jdbcTemplate.execute(sql, new PreparedStatementCallback<User>() {
        @Override
        public User doInPreparedStatement(PreparedStatement ps) throws SQLException, DataAccessException {
            User user = null;
            ps.setInt(1, 5);
            ResultSet rs = ps.executeQuery();
            if (rs.next()) {
                user = new User();
                user.setId(rs.getInt("id"));
                user.setRealName(rs.getString("real_name"));
                user.setAge(rs.getInt("age"));
            }
            return user;
        }
    });
    System.out.println(user);
}
~~~

### 13.12 使用德鲁伊连接池

第一步：引入德鲁伊连接池的依赖

~~~xml
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.1.8</version>
</dependency>
~~~

第二步：将德鲁伊中的数据源配置到spring配置文件中。和配置我们自己写的一样。

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/spring6"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>

    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="druidDataSource"/>
    </bean>
</beans>
~~~

使用：

```java
@Test
public void  testQueryOneValue(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    JdbcTemplate jdbcTemplate = applicationContext.getBean("jdbcTemplate", JdbcTemplate.class);
    String sql= "select count(1) from t_act";
    Integer integer = jdbcTemplate.queryForObject(sql, int.class);
    System.out.println(integer);
}
```

## 十四、 GoF之代理模式

![对代理模式的理解](C:\Users\fengshun\Desktop\自学\spring\图片\对代理模式的理解.png)

**业务场景**：系统中有A、B、C三个模块，使用这些模块的前提是需要用户登录，也就是说在A模块中要编写判断登录的代码，B模块中也要编写，C模块中还要编写，这些判断登录的代码反复出现，显然代码没有得到复用，可以为A、B、C三个模块提供一个代理，在代理当中写一次登录判断即可。代理的逻辑是：请求来了之后，判断用户是否登录了，如果已经登录了，则执行对应的目标，如果没有登录则跳转到登录页面。【在程序中，目标不但受到保护，并且代码也得到了复用。】

### 14.1 静态代理

现在有这样一个接口和实现类：

> OrderService接口：
>
> ~~~java
> package com.powernode.mall.service;
> 
> /**
>  * 订单接口
>  * @author 动力节点
>  * @version 1.0
>  * @className OrderService
>  * @since 1.0
>  **/
> public interface OrderService {
>     /**
>      * 生成订单
>      */
>     void generate();
> 
>     /**
>      * 查看订单详情
>      */
>     void detail();
> 
>     /**
>      * 修改订单
>      */
>     void modify();
> }
> 
> ~~~
>
> OrderService接口的实现类
>
> ~~~java
> package com.powernode.mall.service.impl;
> 
> import com.powernode.mall.service.OrderService;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className OrderServiceImpl
>  * @since 1.0
>  **/
> public class OrderServiceImpl implements OrderService {
>     @Override
>     public void generate() {
>         try {
>             Thread.sleep(1234);
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>         System.out.println("订单已生成");
>     }
> 
>     @Override
>     public void detail() {
>         try {
>             Thread.sleep(2541);
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>         System.out.println("订单信息如下：******");
>     }
> 
>     @Override
>     public void modify() {
>         try {
>             Thread.sleep(1010);
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>         System.out.println("订单已修改");
>     }
> }
> 
> ~~~

其中Thread.sleep()方法的调用是为了模拟操作耗时。

项目已上线，并且运行正常，只是客户反馈系统有一些地方运行较慢，要求项目组对系统进行优化。于是项目负责人就下达了这个需求。首先需要搞清楚是哪些业务方法耗时较长，于是让我们统计每个业务方法所耗费的时长。如果是你，你该怎么做呢？

第一种方案：直接修改Java源代码，在每个业务方法中添加统计逻辑，如下：

> ~~~java
> package com.powernode.mall.service.impl;
> 
> import com.powernode.mall.service.OrderService;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className OrderServiceImpl
>  * @since 1.0
>  **/
> public class OrderServiceImpl implements OrderService {
>     @Override
>     public void generate() {
>         long begin = System.currentTimeMillis();
>         try {
>             Thread.sleep(1234);
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>         System.out.println("订单已生成");
>         long end = System.currentTimeMillis();
>         System.out.println("耗费时长"+(end - begin)+"毫秒");
>     }
> 
>     @Override
>     public void detail() {
>         long begin = System.currentTimeMillis();
>         try {
>             Thread.sleep(2541);
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>         System.out.println("订单信息如下：******");
>         long end = System.currentTimeMillis();
>         System.out.println("耗费时长"+(end - begin)+"毫秒");
>     }
> 
>     @Override
>     public void modify() {
>         long begin = System.currentTimeMillis();
>         try {
>             Thread.sleep(1010);
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>         System.out.println("订单已修改");
>         long end = System.currentTimeMillis();
>         System.out.println("耗费时长"+(end - begin)+"毫秒");
>     }
> }
> ~~~
>
> 需求可以满足，但显然是违背了OCP开闭原则。这种方案不可取。

第二种方案：编写一个子类继承OrderServiceImpl，在子类中重写每个方法，代码如下：

> OrderServiceImpl的子类：
>
> ~~~java
> package com.powernode.mall.service.impl;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className OrderServiceImplSub
>  * @since 1.0
>  **/
> public class OrderServiceImplSub extends OrderServiceImpl{
>     @Override
>     public void generate() {
>         long begin = System.currentTimeMillis();
>         super.generate();
>         long end = System.currentTimeMillis();
>         System.out.println("耗时"+(end - begin)+"毫秒");
>     }
> 
>     @Override
>     public void detail() {
>         long begin = System.currentTimeMillis();
>         super.detail();
>         long end = System.currentTimeMillis();
>         System.out.println("耗时"+(end - begin)+"毫秒");
>     }
> 
>     @Override
>     public void modify() {
>         long begin = System.currentTimeMillis();
>         super.modify();
>         long end = System.currentTimeMillis();
>         System.out.println("耗时"+(end - begin)+"毫秒");
>     }
> }
> ~~~
>
> 这种方式可以解决，但是存在两个问题：
>
> - 第一个问题：假设系统中有100个这样的业务类，需要提供100个子类，并且之前写好的创建Service对象的代码，都要修改为创建子类对象。
> - 第二个问题：由于采用了继承的方式，导致代码之间的耦合度较高。

第三种方案：使用代理模式（这里采用静态代理）；

> 可以为OrderService接口提供一个代理类。
>
> ~~~java
> package com.powernode.mall.service;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className OrderServiceProxy
>  * @since 1.0
>  **/
> public class OrderServiceProxy implements OrderService{ // 代理对象
> 
>     // 目标对象
>     private OrderService orderService;
> 
>     // 通过构造方法将目标对象传递给代理对象
>     public OrderServiceProxy(OrderService orderService) {
>         this.orderService = orderService;
>     }
> 
>     @Override
>     public void generate() {
>         long begin = System.currentTimeMillis();
>         // 执行目标对象的目标方法
>         orderService.generate();
>         long end = System.currentTimeMillis();
>         System.out.println("耗时"+(end - begin)+"毫秒");
>     }
> 
>     @Override
>     public void detail() {
>         long begin = System.currentTimeMillis();
>         // 执行目标对象的目标方法
>         orderService.detail();
>         long end = System.currentTimeMillis();
>         System.out.println("耗时"+(end - begin)+"毫秒");
>     }
> 
>     @Override
>     public void modify() {
>         long begin = System.currentTimeMillis();
>         // 执行目标对象的目标方法
>         orderService.modify();
>         long end = System.currentTimeMillis();
>         System.out.println("耗时"+(end - begin)+"毫秒");
>     }
> }
> 
> ~~~
>
> 这种方式的优点：符合OCP开闭原则，同时采用的是关联关系，所以程序的耦合度较低。所以这种方案是被推荐的。

![类和类之间的关系](C:\Users\fengshun\Desktop\自学\spring\图片\类和类之间的关系.png)

静态代理的缺点：当需要代理的对象较多时，需要创建多个代理类。

可以使用动态代理解决，动态代理还是代理模式，只不过添加了字节码生成技术，可以在内存中为我们动态的生成一个class字节码，这个字节码就是代理类。在内存中动态生成字节码代理类的技术，叫做动态代理。

### 14.2 动态代理

在程序运行阶段，在内存中动态生成代理类，被称为动态代理，目的是为了减少代理类的数量。解决代码复用的问题。

在内存当中动态生成类的技术常见的包括：

> - **JDK动态代理技术**：只能代理接口。
> - **CGLIB动态代理技术**：CGLIB(Code Generation Library)是一个开源项目。是一个强大的，高性能，高质量的Code生成类库，它可以在运行期扩展Java类与实现Java接口。它既可以代理接口，又可以代理类，**底层是通过继承的方式实现的**。性能比JDK动态代理要好。**（底层有一个小而快的字节码处理框架ASM。）**
> - **Javassist动态代理技术**：Javassist是一个开源的分析、编辑和创建Java字节码的类库。是由东京工业大学的数学和计算机科学系的 Shigeru Chiba （千叶 滋）所创建的。它已加入了开放源代码JBoss 应用服务器项目，通过使用Javassist对字节码操作为JBoss实现动态"AOP"框架。

#### 14.2.1 JDK 动态代理

先准备一个接口，一个实现类：

> OrderService接口：
>
> ~~~java
> package com.powernode.mall.service;
> 
> /**
>  * 订单接口
>  * @author 动力节点
>  * @version 1.0
>  * @className OrderService
>  * @since 1.0
>  **/
> public interface OrderService {
>     /**
>      * 生成订单
>      */
>     void generate();
> 
>     /**
>      * 查看订单详情
>      */
>     void detail();
> 
>     /**
>      * 修改订单
>      */
>     void modify();
> }
> 
> ~~~
>
> OrderService接口实现类：
>
> ~~~java
> package com.powernode.mall.service.impl;
> 
> import com.powernode.mall.service.OrderService;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className OrderServiceImpl
>  * @since 1.0
>  **/
> public class OrderServiceImpl implements OrderService {
>     @Override
>     public void generate() {
>         try {
>             Thread.sleep(1234);
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>         System.out.println("订单已生成");
>     }
> 
>     @Override
>     public void detail() {
>         try {
>             Thread.sleep(2541);
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>         System.out.println("订单信息如下：******");
>     }
> 
>     @Override
>     public void modify() {
>         try {
>             Thread.sleep(1010);
>         } catch (InterruptedException e) {
>             e.printStackTrace();
>         }
>         System.out.println("订单已修改");
>     }
> }
> 
> ~~~

在客户端程序中，也需要进行三步：

> 第一步：创建目标对象
>
> >使用向上转型，创建目标对象
> >
> >~~~java
> >OrderService target = new OrderServiceImpl();
> >~~~
>
> 第二步：创建代理对象
>
> >需要使用 javax.lang.reflect.Proxy 类的 newProxyInstance () 方法 来帮助我们动态生成。
> >
> >~~~java
> >Object proxyibj = Proxy.newProxyInstance(目标类的类加载器，代理类要实现的接口，调用处理器)
> >~~~
> >
> >newProxyInstance  翻译为：创建代理对象。就是说，通过调用这个方法可以创建代理对象。本质上，这个 Proxy.newProxyInstance() 方法的执行，做了两件事：
> >
> >- 第一件事：在内存中动态生成了一个代理类的字节码class
> >- 第二件事：new 对象了。通过内存中生成的代理类在这个代码，实例化了代理对象。
> >
> >关于  newProxyInstance() 方法的三个重要参数：
> >
> >1. ClassLoader loader
> >
> >  类加载器：当内存中生成的字节码也是class文件，要执行也得先加载到内存中。加载类就需要类加载器。所有这里需要指定类加载器，并且JDK 要求，<u>**目标类的类加载器必须和代理类的类加载器使用同一个。**</u>
> >
> >2. Class<?>[] interfaces
> >
> >  代理类和目标类要实现同一个或同一些接口。在内存中生成代理类的时候，这个代理类是需要你告诉它实现哪个接口的。
> >
> >3. InvocationHandler h 
> >
> >  InvocationHandler  被翻译为：调用处理器，是一个接口。
> >
> >  在调用处理器接口中编写的是增强代码，因为具体要增强什么代码，JDK动态代理技术它是猜不到的。
> >
> >  既然是接口，就要写接口的实现类。这个调用处理器写一次就好。
> >
> >
> >
> >实现 InvocationHandler 接口，必须实现 invoke 方法。
> >
> >这个方法什么时候被调用？
> >
> >​	—— 当代理对象调用代理方法的时候，注册在 InvocationHandler  调用处理器当中的invoke() 方法就会被调用。
> >
> >invoke 方法是JDK负责调用的，所有JDK调用这个方法的时候会自动给我们传过来三个参数。
> >
> >invoke 方法的三个参数：
> >
> >> 第一个参数： Object  proxy   代理对象的引用。（这个参数使用较少）
> >>
> >> 第二个参数：Method  method 	目标对象上的目标方法。（要执行的目标方法就是它）
> >>
> >> 第三个参数：Object[ ]  args	 	目标方法上的实参。
> >
> >定义 TimerInvocationhandler 类实现 InvocationHandler 接口：
> >
> >~~~java
> >package com.powernode.mall.service;
> >
> >import java.lang.reflect.InvocationHandler;
> >import java.lang.reflect.Method;
> >
> >/**
> > * @author 动力节点
> > * @version 1.0
> > * @className TimerInvocationHandler
> > * @since 1.0
> > **/
> >public class TimerInvocationHandler implements InvocationHandler {
> >    // 目标对象
> >    private Object target;
> >
> >    // 通过构造方法来传目标对象
> >    public TimerInvocationHandler(Object target) {
> >        this.target = target;
> >    }
> >
> >    @Override
> >    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
> >        // 目标执行之前增强。
> >        long begin = System.currentTimeMillis();
> >        // 调用目标对象的目标方法
> >        Object retValue = method.invoke(target, args);
> >        // 目标执行之后增强。
> >        long end = System.currentTimeMillis();
> >        System.out.println("耗时"+(end - begin)+"毫秒");
> >        // 一定要记得返回哦。
> >        return retValue;
> >    }
> >}
> >
> >~~~
> >
> >
> >
> >  ~~~java
> >  OrderService proxyibj = (OrderService)Proxy.newProxyInstance(target.getClass().getClassLoader()，OrderService.Class，new TimerInvocationhandler)
> >  ~~~
> >
> >
>
> 第三步：调用代理对象的方法。
>
> ~~~java
>  // 调用代理对象的代理方法
>         orderServiceProxy.detail();
>         orderServiceProxy.modify();
>         orderServiceProxy.generate();
> ~~~

#### 14.2.2 CGLIB 动态代理

CGLIB既可以代理接口，又可以代理类。底层采用继承的方式实现。所以被代理的目标类不能使用final修饰。

使用CGLIB，需要引入它的依赖：

~~~xml
<dependency>
  <groupId>cglib</groupId>
  <artifactId>cglib</artifactId>
  <version>3.3.0</version>
</dependency>
~~~

~~~java
package com.powernode.mall;

import com.powernode.mall.service.UserService;
import net.sf.cglib.proxy.Enhancer;

/**
 * @author 动力节点
 * @version 1.0
 * @className Client
 * @since 1.0
 **/
public class Client {
    public static void main(String[] args) {
        // 创建字节码增强器
        //这个对象是CGLIB库当中的核心对象，就是依靠它来生成代理类
        Enhancer enhancer = new Enhancer();
        // 告诉cglib要继承哪个类
        enhancer.setSuperclass(UserService.class);
        // 设置回调接口（等同于JDK动态代理中的调用处理器）
        //在CGLIB 中不是InvocationHandler 接口，是方法拦截器接口：MethodInterceptor
        enhancer.setCallback(方法拦截器对象);
        // 生成源码，编译class，加载到JVM，并创建代理对象
        UserService userServiceProxy = (UserService)enhancer.create();
        /*
        创建代理对象，这一步会做两件事：
        	第一件事：在内存中生成UserService类的子类，其实就是代理类的字节码
        	第二件事：创建代理对象
        */
        //调用代理对象的代理方法
        userServiceProxy.login();
        userServiceProxy.logout();

    }
}
~~~

编写MethodInterceptor接口实现类：

> MethodInterceptor接口中有一个方法intercept()，该方法有4个参数：
>
> 第一个参数：目标对象
>
> 第二个参数：目标方法
>
> 第三个参数：目标方法调用时的实参
>
> 第四个参数：代理方法
>
> ~~~java
> package com.powernode.mall.service;
> 
> import net.sf.cglib.proxy.MethodInterceptor;
> import net.sf.cglib.proxy.MethodProxy;
> 
> import java.lang.reflect.Method;
> 
> /**
>  * @author 动力节点
>  * @version 1.0
>  * @className TimerMethodInterceptor
>  * @since 1.0
>  **/
> public class TimerMethodInterceptor implements MethodInterceptor {
>     @Override
>     public Object intercept(Object target, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
>         // 前增强
>         long begin = System.currentTimeMillis();
>         // 调用目标
>         Object retValue = methodProxy.invokeSuper(target, objects);
>         // 后增强
>         long end = System.currentTimeMillis();
>         System.out.println("耗时" + (end - begin) + "毫秒");
>         // 一定要返回
>         return retValue;
>     }
> }
> 
> ~~~

## 十五、面向切面编程AOP

IoC使软件组件松耦合。AOP让你能够捕捉系统中经常使用的功能，把它转化成组件。

AOP（Aspect Oriented Programming）：面向切面编程，面向方面编程。（AOP是一种编程技术）

AOP是对OOP的补充延伸。

AOP底层使用的就是动态代理来实现的。

Spring的AOP使用的动态代理是：JDK动态代理 + CGLIB动态代理技术。Spring在这两种动态代理中灵活切换，如果是代理接口，会默认使用JDK动态代理，如果要代理某个类，这个类没有实现接口，就会切换使用CGLIB。当然，你也可以强制通过一些配置让Spring只使用CGLIB。

### 15.1 AOP介绍

一般一个系统当中都会有一些系统服务，例如：日志、事务管理、安全等。这些系统服务被称为：**交叉业务**

这些**交叉业务**几乎是通用的，不管你是做银行账户转账，还是删除用户数据。日志、事务管理、安全，这些都是需要做的。

如果在每一个业务处理过程当中，都掺杂这些交叉业务代码进去的话，存在两方面问题：

- 第一：交叉业务代码在多个业务流程中反复出现，显然这个交叉业务代码没有得到复用。并且修改这些交叉业务代码的话，需要修改多处。
- 第二：程序员无法专注核心业务代码的编写，在编写核心业务代码的同时还需要处理这些交叉业务。

使用AOP可以很轻松的解决以上问题。

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665732609757-d8ae52ba-915e-49cf-9ef4-c7bcada0d601.png)

总结：**将与核心业务无关的代码独立的抽取出来，形成一个独立的组件，然后以横向交叉的方式应用到业务流程当中的过程被称为AOP。**

**AOP的优点：**

- **第一：代码复用性增强。**
- **第二：代码易维护。**
- **第三：使开发者更关注业务逻辑。**

### 15.2 AOP的七大术语

1. **连接点 Joinpoint**

>在程序的整个执行流程中，**可以织入**切面的位置。方法的执行前后，异常抛出之后等位置。
>
>连接点描述的是一个位置。

2. **切点 Pointcut**

>在程序执行流程中，**真正织入**切面的方法。（一个切点对应多个连接点）
>
>切点本质上就是方法。

3. **通知 Advice**

> 通知又叫增强，就是具体你要织入的代码。例如：具体的事物代码、日志代码、安全代码、统计时长代码。
> 通知包括：
>
> - 前置通知		（放在方法的前面的代码）
> - 后置通知        （放在方法的后面的代码）
> - 环绕通知          （方法的前面和后面的代码）
> - 异常通知           （在catch中的代码）
> - 最终通知         （在finally中的代码）
>
> 通知是放在连接点上的。

4. **切面 Aspect**

> **切点 + 通知就是切面。** 它是一个逻辑概念。

5. 织入 Weaving

> 把通知应用到目标对象上的过程。

6. 代理对象 Proxy

> 一个目标对象被织入通知后产生的新对象。

7. 目标对象 Target

> 被织入通知的对象。

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1665735638342-44194599-66e2-4c02-a843-8a8b3ba5b0c8.png)

### 15.3 切点表达式

切点表达式用来定义通知（Advice）往哪些方法上切入。

语法格式：

~~~
execution([访问控制权限修饰符] 返回值类型 [全限定类名]方法名(形式参数列表) [异常])
~~~

访问控制权限修饰符：

> 可选项。
>
> 没写，就是4个权限都包括。
>
> 写public就表示只包括公开的方法。

返回值类型：

> 必填项。
>
> “  \*  ”  表示返回值类型任意。

全限定类名 ：

> 可选项。
>
> 两个点  “ .. ”  代表当前包以及子包下的所有类。例如：com..  表示com包以及子包下的所有类
>
> 省略时表示所有的类。

方法名：

> 必填项。
>
> “  *  ”  表示所有方法。例如 “  set*  ” 表示所有的set方法。

形式参数列表：

> 必填项
>
> () 表示没有参数的方法
>
> (..) 参数类型和个数随意的方法
>
> (*) 只有一个参数的方法
>
> (*, String) 第一个参数类型随意，第二个参数是String的。

异常：

> 可选项。
>
> 省略时表示任意异常类型。

练习：

> service包下所有的类中以delete开始的所有方法
>
> ~~~java
> execution(public * com.powernode.mall.service.*.delete*(..))
> ~~~
>
> mall包下所有的类的所有的方法
>
> ~~~java
> execution(* com.powernode.mall..*(..))
> ~~~
>
> 所有类的所有方法
>
> ~~~java
> execution(* *(..))
> ~~~

### 15.4 使用Spring的AOP

Spring对AOP的实现包括以下3种方式：

- **第一种方式：Spring框架结合AspectJ框架实现的AOP，基于注解方式。**
- **第二种方式：Spring框架结合AspectJ框架实现的AOP，基于XML方式。**
- 第三种方式：Spring框架自己实现的AOP，基于XML配置方式。

实际开发中，都是Spring+AspectJ来实现AOP。所以我们重点学习第一种和第二种方式。

什么是AspectJ？（Eclipse组织的一个支持AOP的框架。AspectJ框架是独立于Spring框架之外的一个框架，Spring框架用了AspectJ） 

#### 15.4.1 准备工作

使用Spring+AspectJ的AOP需要引入的依赖如下：

~~~xml
<!--spring context依赖-->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context</artifactId>
  <version>6.0.0</version>
</dependency>
<!--spring aop依赖 在引入context依赖的时候会自动引入-->
<!--spring aspects依赖-->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aspects</artifactId>
  <version>6.0.0</version>
</dependency>
~~~

Spring配置文件中添加context命名空间和aop命名空间

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

</beans>
~~~

#### 15.4.2 基于AspectJ的AOP注解式开发

##### 实现步骤

第一步：定义目标类以及目标方法

> 使用 @Service 注解标注，交给Spring框架去管理
>
> ~~~java
> package com.fengshun.service;
> 
> import org.springframework.stereotype.Service;
> 
> @Service("studentService")
> public class StudentService {
>     public void generate(){
>         System.out.println("订单已生成");
>     }
> }
> ~~~

第二步：定义切面类

> 定义切面类，需要使用 @Aspect 注解标注这个类是切面类，并且使用 @component 注解把这个类交给Spring进行管理。
>
> ```java
> package com.fengshun.service;
> 
> import org.aspectj.lang.annotation.Aspect;
> 
> @Component
> @Aspect
> public class LogAspect {  //切面类
>     // 切面=切点+通知
> }
> ```

第三步：在Spring配置文件中添加组件扫描

> ~~~xml
>     <context:component-scan base-package="com.fengshun.service"/>
> ~~~

第四步：在切面类中定义通知

> 想要定义通知，除了需要定义方法以外，还需要使用注解标记方法为通知。
>
> 在注解中需要传入一个字符串，这个字符串就是切点表达式。
>
> ```java
> @Before("execution(* com.fengshun.service..*(..))")
> public void advice(){
>     System.out.println("这是一个通知");
> }
> ```
>
> 注意：@Before 注解表示前置通知

第五步：在Spring配置文件中添加自动代理

> ~~~xml
> <aop:aspectj-autoproxy/>
> ~~~
>
> aop:aspectj-autoproxy  标签中的 proxy-target-class 属性：
>
> 该属性默认值为false，表示采用JDBC动态代理生成代理类，当没有接口的时候，自动使用 CGLIB 动态代理 生成代理类

第六步：测试代码

> ```java
> @Test
> public void test(){
>     ApplicationContext  applicationContext= new ClassPathXmlApplicationContext("spring.xml");
>     StudentService studentService = applicationContext.getBean("studentService", StudentService.class);
>     studentService.generate();
> }
> ```

##### 通知类型

通知类型包括：

- 前置通知：@Before 目标方法执行之前的通知
- 后置通知：@AfterReturning 目标方法执行之后的通知
- 环绕通知：@Around 目标方法之前添加通知，同时目标方法执行之后添加通知。
- 异常通知：@AfterThrowing 发生异常之后执行的通知
- 最终通知：@After 放在finally语句块中的通知

~~~java
package com.powernode.spring6.service;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

// 切面类
@Component
@Aspect
public class MyAspect {
    //前置通知
    @Before("execution(* com.powernode.spring6.service.OrderService.*(..))")
    public void beforeAdvice(){
        System.out.println("前置通知");
    }
    //后置通知
    @AfterReturning("execution(* com.powernode.spring6.service.OrderService.*(..))")
    public void afterReturningAdvice(){
        System.out.println("后置通知");
    }
    //环绕通知
	@Around("execution(* com.powernode.spring6.service.OrderService.*(..))")
    public void aroundAdvice(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕通知开始");
        // 执行目标方法。
        proceedingJoinPoint.proceed();
        System.out.println("环绕通知结束");
    }
    //异常通知
    @AfterThrowing("execution(* com.powernode.spring6.service.OrderService.*(..))")
    public void afterThrowingAdvice(){
        System.out.println("异常通知");
    }
    //最终通知
    @After("execution(* com.powernode.spring6.service.OrderService.*(..))")
    public void afterAdvice(){
        System.out.println("最终通知");
    }

}
~~~

环绕通知是范围最广的通知，其函数需要 ProceedingJoinPoint 类型参数，通过  ProceedingJoinPoint对象.proceed() 方法 能够执行目标方法。

各个通知的执行顺序：

​		环绕通知开始  >  前置通知  >  方法  >  后置通知  >  环绕通知结束  >  最终通知

出现异常之后，**后置通知**和**环绕通知的结束部分**不会执行： 环绕通知开始  >  前置通知  >  方法  >  异常通知  >  最终通知   

-------

##### 切面的先后顺序

业务流程当中不一定只有一个切面，可能有的切面控制事务，有的记录日志，有的进行安全控制，如果多个切面的话，顺序如何控制：**可以使用@Order注解来标识切面类，为@Order注解的value指定一个整数型的数字，数字越小，优先级越高**。

定义两个切面类：

> 优先级为1
>
> ~~~java
> package com.powernode.spring6.service;
> 
> import org.aspectj.lang.ProceedingJoinPoint;
> import org.aspectj.lang.annotation.*;
> import org.springframework.core.annotation.Order;
> import org.springframework.stereotype.Component;
> 
> @Aspect
> @Component
> @Order(1) //设置优先级
> public class YourAspect {
> 
>     @Around("execution(* com.powernode.spring6.service.OrderService.*(..))")
>     public void aroundAdvice(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
>         System.out.println("YourAspect环绕通知开始");
>         // 执行目标方法。
>         proceedingJoinPoint.proceed();
>         System.out.println("YourAspect环绕通知结束");
>     }
> 
> }
> 
> ~~~
>
> 优先级为2
>
> ~~~java
> package com.powernode.spring6.service;
> 
> import org.aspectj.lang.ProceedingJoinPoint;
> import org.aspectj.lang.annotation.*;
> import org.springframework.core.annotation.Order;
> import org.springframework.stereotype.Component;
> 
> // 切面类
> @Component
> @Aspect
> @Order(2) //设置优先级
> public class MyAspect {
> 
>     @Around("execution(* com.powernode.spring6.service.OrderService.*(..))")
>     public void aroundAdvice(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
>         System.out.println("环绕通知开始");
>         // 执行目标方法。
>         proceedingJoinPoint.proceed();
>         System.out.println("环绕通知结束");
>     
> 
> }
> ~~~

##### 优化使用切点表达式

在之前的程序中，每一个通知注解中都需要切点表达式，这样有以下缺点：

- 第一：切点表达式重复写了多次，没有得到复用。
- 第二：如果要修改切点表达式，需要修改多处，难维护。

使用 @Pointcut 将切点表达式单独的定义出来，在需要的位置引入即可。如下：

~~~java
package com.powernode.spring6.service;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

// 切面类
@Component
@Aspect
@Order(2)
public class MyAspect {
    
    //定义一个空的方法，将切点表达式单独定义 
    @Pointcut("execution(* com.powernode.spring6.service.OrderService.*(..))")
    public void pointcut(){}

    @Around("pointcut()")
    public void aroundAdvice(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕通知开始");
        // 执行目标方法。
        proceedingJoinPoint.proceed();
        System.out.println("环绕通知结束");
    }

    @Before("pointcut()")
    public void beforeAdvice(){
        System.out.println("前置通知");
    }

}

~~~

##### 连接点

除了环绕通知的方法有参数以外，其实其他通知的方法也有参数，这个参数就是 JoinPoint 对象，他代表着连接点。

这个JoinPoint 对象，在Spring容器调用这个方法的时候自动传过来。

这个参数的作用：

> 获取目标方法的签名。
>
> ~~~java
> Signature signature = joinPoint.getsignature();
> ~~~
>
> 通过方法的签名可以获得一个方法的具体信息。（访问修饰符、返回值类型、方法名）
>
> ~~~
> signature.getName();
> ~~~
>
> 

##### 全注解式开发AOP

编写一个类，在这个类上面使用大量注解来代替spring的配置文件，spring配置文件消失了，如下：

```java
package com.fengshun.service;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration  //代替Spring.xml 文件
@ComponentScan({"com.fengshun.service"})    //组件扫描
@EnableAspectJAutoProxy(proxyTargetClass = true) //启用aspectj的自动代理，proxyTargetClass = true 表示只使用CGLIB动态代理
public class SpringConfig {
}
```

测试程序：

```java
public void test1(){
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(SpringConfig.class);
        StudentService studentService = applicationContext.getBean("studentService", StudentService.class);
        studentService.generate();
    }
```

#### 15.4.3 基于XML配置方式的AOP（了解）

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--纳入spring bean管理-->
    <bean id="vipService" class="com.powernode.spring6.service.VipService"/>
    <bean id="timerAspect" class="com.powernode.spring6.service.TimerAspect"/>

    <!--aop配置-->
    <aop:config>
        <!--切点表达式-->
        <aop:pointcut id="p" expression="execution(* com.powernode.spring6.service.VipService.*(..))"/>
        <!--切面-->
        <aop:aspect ref="timerAspect">
            <!--切面=通知 + 切点-->
            <aop:around method="time" pointcut-ref="p"/>
        </aop:aspect>
    </aop:config>
</beans>
~~~

### 15.5  AOP的实际案例：事物处理

银行账户的业务类：

~~~java
package com.powernode.spring6.biz;

import org.springframework.stereotype.Component;

@Component
// 业务类
public class AccountService {
    // 转账业务方法
    public void transfer(){
        System.out.println("正在进行银行账户转账");
    }
    // 取款业务方法
    public void withdraw(){
        System.out.println("正在进行取款操作");
    }
}

~~~

订单业务类：

~~~java
package com.powernode.spring6.biz;

import org.springframework.stereotype.Component;

@Component
// 业务类
public class OrderService {
    // 生成订单
    public void generate(){
        System.out.println("正在生成订单");
    }
    // 取消订单
    public void cancel(){
        System.out.println("正在取消订单");
    }
}
~~~

事务切面类：

~~~java
package com.powernode.spring6.biz;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
// 事务切面类
public class TransactionAspect {
    //环绕通知
    @Around("execution(* com.powernode.spring6.biz..*(..))")
    public void aroundAdvice(ProceedingJoinPoint proceedingJoinPoint){
        try {
            System.out.println("开启事务");
            // 执行目标
            proceedingJoinPoint.proceed();
            System.out.println("提交事务");
        } catch (Throwable e) {
            System.out.println("回滚事务");
        }
    }
}

~~~

spring 配置类：

~~~java
package com.fengshun.service;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration  //代替Spring.xml 文件
@ComponentScan({"com.powernode.spring6.biz"})    //组件扫描
@EnableAspectJAutoProxy(proxyTargetClass = true) //启用aspectj的自动代理，proxyTargetClass = true 表示只使用CGLIB动态代理
public class SpringConfig {
}

~~~

### 15.6 AOP的实际案例：安全日志

需求是这样的：项目开发结束了，已经上线了。运行正常。客户提出了新的需求：凡事在系统中进行修改操作的，删除操作的，新增操作的，都要把这个人记录下来。因为这几个操作是属于危险行为。例如有业务类和业务方法：

用户业务类

~~~java
package com.powernode.spring6.biz;

import org.springframework.stereotype.Component;

@Component
//用户业务
public class UserService {
    public void getUser(){
        System.out.println("获取用户信息");
    }
    public void saveUser(){
        System.out.println("保存用户");
    }
    public void deleteUser(){
        System.out.println("删除用户");
    }
    public void modifyUser(){
        System.out.println("修改用户");
    }
}
~~~

商品业务类

~~~java
package com.powernode.spring6.biz;

import org.springframework.stereotype.Component;

// 商品业务类
@Component
public class ProductService {
    public void getProduct(){
        System.out.println("获取商品信息");
    }
    public void saveProduct(){
        System.out.println("保存商品");
    }
    public void deleteProduct(){
        System.out.println("删除商品");
    }
    public void modifyProduct(){
        System.out.println("修改商品");
    }
}


~~~

负责安全的切面类

~~~java
package com.powernode.spring6.biz;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

@Component
@Aspect
public class SecurityAspect {

    @Pointcut("execution(* com.powernode.spring6.biz..save*(..))")
    public void savePointcut(){}

    @Pointcut("execution(* com.powernode.spring6.biz..delete*(..))")
    public void deletePointcut(){}

    @Pointcut("execution(* com.powernode.spring6.biz..modify*(..))")
    public void modifyPointcut(){}

    @Before("savePointcut() || deletePointcut() || modifyPointcut()")
    public void beforeAdivce(JoinPoint joinpoint){
        System.out.println("XXX操作员正在操作"+joinpoint.getSignature().getName()+"方法");
    }
}

~~~

测试程序

~~~java
@Test
public void testSecurity(){
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(Spring6Configuration.class);
    UserService userService = applicationContext.getBean("userService", UserService.class);
    ProductService productService = applicationContext.getBean("productService", ProductService.class);
    userService.getUser();
    userService.saveUser();
    userService.deleteUser();
    userService.modifyUser();
    productService.getProduct();
    productService.saveProduct();
    productService.deleteProduct();
    productService.modifyProduct();
}
~~~

执行结果：

<img src="C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\1665901327786-9bfab382-61a3-4d1e-abe5-728b242eb3a2.png" alt="5F9597E7-7930-4384-95C2-CF64C9DDA9F3.png" style="zoom:67%;" />

## 十六、Spring对事务的支持

### 16.1 事务概述

什么是事务
> - 在一个业务流程当中，通常需要多条DML（insert delete update）语句共同联合才能完成，这多条DML语句必须同时成功，或者同时失败，这样才能保证数据的安全。
> - 多条DML要么同时成功，要么同时失败，这叫做事务。
> -  事务：Transaction（tx）

事务的四个处理过程：

> - 第一步：开启事务 (start transaction)
> - 第二步：执行核心业务代码
> - 第三步：提交事务（如果核心业务处理过程中没有出现异常）(commit transaction)
> - 第四步：回滚事务（如果核心业务处理过程中出现异常）(rollback transaction)

事务的四个特性：

> - A 原子性：事务是最小的工作单元，不可再分。
> - C 一致性：事务要求要么同时成功，要么同时失败。事务前和事务后的总量不变。
> - I 隔离性：事务和事务之间因为有隔离性，才可以保证互不干扰。
> - D 持久性：持久性是事务结束的标志。

### 16.2 引入事务场景

定义POJO类

```java
package com.fengshun.transaction.pojo;

public class Account {
    private Long id;
    private String actno;
    private Double balance;

    public Account() {
    }

    public Account(Long id, String actno, Double balance) {
        this.id = id;
        this.actno = actno;
        this.balance = balance;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public Double getBalance() {
        return balance;
    }

    public void setBalance(Double balance) {
        this.balance = balance;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", actno='" + actno + '\'' +
                ", balance=" + balance +
                '}';
    }
}
```

定义Mapper接口的实现类：

```java
package com.fengshun.transaction.mapper.impl;

import com.fengshun.transaction.mapper.AccountMapper;
import com.fengshun.transaction.pojo.Account;
import jakarta.annotation.Resource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@Repository("accountMapperImpl")
public class AccountMapperImpl implements AccountMapper {
    @Resource(name="jdbcTemplate")
    private JdbcTemplate jdbcTemplate;

    @Override
    public Account selectByActno(String actno) {

        String sql = "select id,actno,balance from t_act where actno=?";
        Account account = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(Account.class), actno);
        return account;
    }

    @Override
    public int updateAct(Account account) {
        String sql = "update t_act set balance=? where actno=?";
        int update = jdbcTemplate.update(sql, account.getBalance(), account.getActno());
        return update;
    }
}
```

定义service接口的实现类：

```java
package com.fengshun.transaction.service.impl;

import com.fengshun.transaction.mapper.AccountMapper;
import com.fengshun.transaction.pojo.Account;
import com.fengshun.transaction.service.AccountService;
import jakarta.annotation.Resource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service("accountServiceImpl")
public class AccountServiceImpl implements AccountService {
    @Resource(name = "accountMapperImpl")
    private AccountMapper accountMapper;

    @Override
    public void transfer(String fromActno, String toActno, Double money) {
        // 查询账户余额是否充足
        Account fromAct = accountMapper.selectByActno(fromActno);
        if (fromAct.getBalance() < money) {
            throw new RuntimeException("账户余额不足");
        }
        // 余额充足，开始转账
        Account toAct = accountMapper.selectByActno(toActno);
        fromAct.setBalance(fromAct.getBalance() - money);
        toAct.setBalance(toAct.getBalance() + money);
        int x = accountMapper.updateAct(fromAct);
        x+= accountMapper.updateAct(toAct);
        if (x!=2) {
            throw new RuntimeException("转账失败，请联系银行");
        }
    }
}
```

Spring配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    
<context:component-scan base-package="com.fengshun.transaction"/>
    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
        <property name="username" value="root"/>
        <property name="password" value="051727"/>
     </bean>
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="druidDataSource"/>
    </bean>
</beans>
```

测试程序：

```java
@Test
public void test(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
    AccountServiceImpl accountServiceImpl = applicationContext.getBean("accountServiceImpl", AccountServiceImpl.class);
    accountServiceImpl.transfer("act001","act002",1000.00);
}
```

### 16.3 Spring 对事物的支持

#### Spring实现事物的两种方式：

> 编程式事务
>
> - - 通过编写代码的方式来实现事务的管理。
>
> 声明式事务
>
> - - 基于注解方式
>
> - - 基于XML配置方式

#### Spring事务管理API

Spring对事务的管理底层实现方式是基于AOP实现的。采用AOP的方式进行了封装。所以Spring专门针对事务开发了一套API，API的核心接口如下：

![image.png](C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\1666504216275-1b6a9ac4-6958-4cdf-9323-7a79a08d059d.png)

PlatformTransactionManager接口：spring事务管理器的核心接口。在**Spring6**中它有两个实现：

- DataSourceTransactionManager：支持JdbcTemplate、MyBatis、Hibernate等事务管理。
- JtaTransactionManager：支持分布式事务管理。

如果要在Spring6中使用JdbcTemplate，就要使用DataSourceTransactionManager来管理事务。（Spring内置写好了，可以直接用。）

#### 声明式事物之注解实现方式

第一步：在spring配置文件中配置事务管理器。

想要管理事物，需要 conn.setAutocommit(false),connection 对象需要数据源提供。

```xml
     <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
        <property name="username" value="root"/>
        <property name="password" value="051727"/>
     </bean>    
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<!--数据源-->
        <property name="dataSource" ref="druidDataSource"/>
    </bean>
```

第二步：在spring配置文件中引入tx命名空间。

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
~~~

第三步：在spring配置文件中配置“事务注解驱动器”，开始注解的方式控制事务。

​	开启事务注解驱动器，开启事务注解，告诉spring框架采用注解的方式去控制事物

```xml
<tx:annotation-driven transaction-manager="transactionManager"/>
```

第四步：使用注解

在service类上或方法上添加**@Transactional**注解

在类上添加该注解，该类中所有的方法都有事务。在某个方法上添加该注解，表示只有这个方法使用事务。

```java
package com.fengshun.transaction.service.impl;

import com.fengshun.transaction.mapper.AccountMapper;
import com.fengshun.transaction.pojo.Account;
import com.fengshun.transaction.service.AccountService;
import jakarta.annotation.Resource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service("accountServiceImpl")
@Transactional
public class AccountServiceImpl implements AccountService {
    @Resource(name = "accountMapperImpl")
    private AccountMapper accountMapper;

    @Override
    public void transfer(String fromActno, String toActno, Double money) {
        // 查询账户余额是否充足
        Account fromAct = accountMapper.selectByActno(fromActno);
        if (fromAct.getBalance() < money) {
            throw new RuntimeException("账户余额不足");
        }
        // 余额充足，开始转账
        Account toAct = accountMapper.selectByActno(toActno);
        fromAct.setBalance(fromAct.getBalance() - money);
        toAct.setBalance(toAct.getBalance() + money);
        int x = accountMapper.updateAct(fromAct);
        x+= accountMapper.updateAct(toAct);
        if (x!=2) {
            throw new RuntimeException("转账失败，请联系银行");
        }
    }
}
```

### 16.4 事务属性

@Transactional 注解源代码如下：

<img src="C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\1666506552984-8a4f9d42-73ba-4ded-853d-564d27340db5.png" alt="image.png" style="zoom:67%;" />

事务中的重点属性：

- 事务传播行为 Propagation
- 事务隔离级别 Isolation
- 事务超时时间   timeout
- 只读事务 readOnly
- 设置出现哪些异常回滚事务  rollbackFor
- 设置出现哪些异常不回滚事务  noRollbackFor

#### 事务传播行为（Propagation）

什么是事务的传播行为？

在service类中有a()方法和b()方法，a()方法上有事务，b()方法上也有事务，当a()方法执行过程中调用了b()方法，事务是如何传递的？合并到一个事务里？还是开启一个新的事务？这就是事务传播行为。

事务传播行为在spring框架中被定义为枚举类型：

<img src="C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\1666505960049-06173489-15fc-4d16-94f3-1a9025f85d8c.png" alt="image.png" style="zoom:67%;" />

一共有七种传播行为：

- REQUIRED：支持当前事务，如果不存在就新建一个(默认)**【没有就新建，有就加入】**
- SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行**【有就加入，没有就不管了】**
- MANDATORY：必须运行在一个事务中，如果当前没有事务正在发生，将抛出一个异常**【有就加入，没有就抛异常】**
- REQUIRES_NEW：开启一个新的事务，如果一个事务已经存在，则将这个存在的事务挂起**【不管有没有，直接开启一个新事务，开启的新事务和之前的事务不存在嵌套关系，之前事务被挂起】**
- NOT_SUPPORTED：以非事务方式运行，如果有事务存在，挂起当前事务**【不支持事务，存在就挂起】**
- NEVER：以非事务方式运行，如果有事务存在，抛出异常**【不支持事务，存在就抛异常】**
- NESTED：如果当前正有一个事务在进行中，则该方法应当运行在一个嵌套式事务中。被嵌套的事务可以独立于外层事务进行提交或回滚。如果外层事务不存在，行为就像REQUIRED一样。**【有事务的话，就在这个事务里再嵌套一个完全独立的事务，嵌套的事务可以独立的提交和回滚。没有事务就和REQUIRED一样。**】**

使用：

1号Service

~~~java
@Transactional(propagation = Propagation.REQUIRED)
public void save(Account act) {

    // 这里调用dao的insert方法。
    accountDao.insert(act); // 保存act-003账户

    // 创建账户对象
    Account act2 = new Account("act-004", 1000.0);
    try {
        accountService.save(act2); // 保存act-004账户
    } catch (Exception e) {

    }
    // 继续往后进行我当前1号事务自己的事儿。
}
~~~

2号Service

~~~java
@Override
//@Transactional(propagation = Propagation.REQUIRED)
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void save(Account act) {
    accountDao.insert(act);
    // 模拟异常
    String s = null;
    s.toString();

    // 事儿没有处理完，这个大括号当中的后续也许还有其他的DML语句。
}
~~~

在1号Service中的save方法，先是调用了dao层中的insert方法，其次调用了2号Service 中的save方法。

当设置1号service中的 save方法的事物传播行为为：REQUIRED 。设置2号Service的事物传播行为为：REQUIRES_NEW。这意味着1号Service是一个事物，2号Service是一个事物，两个事物不是嵌套关系。

> 当2号Service中的save方法，发生异常，并且1号Service中捕获到异常，那么2号事务会回滚，由于1号捕获到了异常，并且处理了异常，所有1号事务不会回滚。
>
> 但是当1号Service中并没有对异常进行处理，那么由2号Servic发生的异常将上抛给1号Service，由于1号Service并没有处理异常，所以导致两个事物都会回滚。

#### 事务隔离级别（isolation）

数据库中读取数据存在的三大问题：（三大读问题）

> - 脏读：读取到没有提交到数据库的数据，叫做脏读。
> - 不可重复读：在同一个事务当中，第一次和第二次读取的数据不一样。
> - 幻读：读到的数据是假的。

事务隔离级别包括四个级别：

> - 读未提交：READ_UNCOMMITTED
>
> - ​	这种隔离级别，存在脏读问题，所谓的脏读(dirty read)表示能够读取到其它事务未提交的数据。
>
> - 读提交：READ_COMMITTED
>
> - ​	解决了脏读问题，其它事务提交之后才能读到，但存在不可重复读问题。
>
> - 可重复读：REPEATABLE_READ
>
> - ​	解决了不可重复读，可以达到可重复读效果，只要当前事务不结束，读取到的数据一直都是一样的。但存在幻读问题。
>
> - 序列化：SERIALIZABLE
>
> - ​	解决了幻读问题，事务排队执行。不支持并发。

| 隔离级别 | 脏读 | 不可重复读 | 幻读 |
| :------: | :--: | :--------: | :--: |
| 读未提交 |  有  |     有     |  有  |
|  读提交  |  无  |     有     |  有  |
| 课重复读 |  无  |     无     |  有  |
|  序列化  |  无  |     无     |  无  |

Isolation 枚举源代码：

![image.png](C:\Users\fengshun\Desktop\自学\spring\图片\1666508609641-2c838566-7334-4cf1-b452-0fed9aaebf3d.png)

使用：

~~~java
@Transactional(isolation = Isolation.READ_COMMITTED)
~~~



#### 事务超时（timeout）

使用：

~~~java
//设置超时时间为10s
@Transactional(timeout = 10)
~~~

事务的超时时间指的是哪一段时间？

**在当前事务当中，最后一条DML语句执行之前的时间。如果最后一条DML语句后面很有很多业务逻辑，这些业务代码执行的时间不被计入超时时间。**



#### 只读事务（readOnly）

使用：

~~~java
@Transactional(readOnly = true)
~~~

将当前事务设置为只读事务，在该事务执行过程中只允许select语句执行，delete insert update均不可执行。

作用：**启动spring的优化策略。提高select语句执行效率。**

如果该事务中确实没有增删改操作，建议设置为只读事务。

------

#### 设置出现哪些异常回滚事务（noRollbackFor）

使用：

~~~java
@Transactional(noRollbackFor = NullPointerException.class)
~~~

------

#### 设置出现哪些异常不回滚事务（noRollbackFor）

使用：

~~~java
@Transactional(noRollbackFor = NullPointerException.class)
~~~

------

### 16.5 事务的全注解式开发

之前在使用注解时，需要在spring 配置文件中进行配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                            http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--组件扫描-->
    <context:component-scan base-package="com.fengshun.transaction"/>
    <!--配置数据源-->
    <bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
        <property name="username" value="root"/>
        <property name="password" value="051727"/>
    </bean>
    <!--配置JDBCTemplate-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="druidDataSource"/>
    </bean>
    <!--配置事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--数据源-->
        <property name="dataSource" ref="druidDataSource"/>
    </bean>
    <!--开启事务注解驱动器，开启事务注解。告诉spring框架，采用注解的方式去控制事物-->
    <tx:annotation-driven transaction-manager="transactionManager"/>
</beans>
```

使用全注解式：

```java
package com.fengshun.transaction;


import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.transaction.TransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;

@Configuration      // 代替spring.xml 文件，在这个类中完成配置
@ComponentScan("com.fengshun.transaction")      //组件扫描
@EnableTransactionManagement        //开启事务注解
public class Spring6Cofig {

    @Bean(name = "dataSource")
    public DataSource getDataSource(){
        DruidDataSource druidDataSource = new DruidDataSource();
        druidDataSource.setDriverClassName("com.nysql.cj.jdbc.Driver");
        druidDataSource.setUrl("jdbc:mysql://localhost:3306/mybatis");
        druidDataSource.setUsername("root");
        druidDataSource.setPassword("051727");
        return druidDataSource;
    }
    @Bean(name = "jdbcTemplate")
    public JdbcTemplate getJdbcTemplate(DataSource dataSource){
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }
    @Bean(name = "transactionManager")
    public DataSourceTransactionManager getDatasourceTransactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }
}
```

@Bean注解相当于xml标签中的bean标签

name 属性就是bean 标签的id属性

每个方法的返回值都会被spring纳入容器管理

方法需要的参数，spring容器会自动传递参数

------

### 16..6 声明式事务之XML实现方式

见文档



##  十七、 Spring6 整合 Junit5

### 17.1 Spring对JUnit4的支持

在之前的测试程序中，需要使用getBean方法去获取对象，再执行对象的方法。每次测试方法都需要写，这很麻烦：

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
// ApplicationContext applicationContext = new AnnotationConfigApplicationContext(Spring6Cofig.class);
AccountServiceImpl accountServiceImpl = applicationContext.getBean("accountServiceImpl", AccountServiceImpl.class);
```

Spring6 对JUnit4 进行了改动：

添加依赖：

~~~xml
<dependencies>
        <!--spring context依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
        <!--spring对junit的支持相关依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
        <!--junit4依赖-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
~~~

声明Bean：

~~~java
package com.powernode.spring6.bean;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

/**
 * @author 动力节点
 * @version 1.0
 * @className User
 * @since 1.0
 **/
@Component
public class User {

    @Value("张三")
    private String name;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public User() {
    }

    public User(String name) {
        this.name = name;
    }
}

~~~

spring配置文件中暴露包

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.powernode.spring6.bean"/>
</beans>
~~~

测试类：

~~~java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;


@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:spring.xml")
public class SpringJUnit4Test {

    @Autowired
    private User user;

    @Test
    public void testUser(){
        System.out.println(user.getName());
    }
}

~~~

Spring提供的方便主要是这几个注解：

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:spring.xml")

在单元测试类上使用这两个注解之后，在单元测试类中的属性上可以使用@Autowired。比较方便。



### 17.2  Spring对JUnit5的支持

引入JUnit5的依赖，Spring对JUnit支持的依赖还是：spring-test，如下：

~~~xml

    <!--仓库-->
    <repositories>
        <!--spring里程碑版本的仓库-->
        <repository>
            <id>repository.spring.milestone</id>
            <name>Spring Milestone Repository</name>
            <url>https://repo.spring.io/milestone</url>
        </repository>
    </repositories>

    <dependencies>
        <!--spring context依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
        <!--spring对junit的支持相关依赖-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>6.0.0-M2</version>
        </dependency>
        <!--junit5依赖-->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.9.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
~~~

单元测试类：

~~~java
package com.powernode.spring6.test;

import com.powernode.spring6.bean.User;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;


@ExtendWith(SpringExtension.class)
@ContextConfiguration("classpath:spring.xml")
public class SpringJUnit5Test {

    @Autowired
    private User user;

    @Test
    public void testUser(){
        System.out.println(user.getName());
    }
}
~~~

在JUnit5当中，可以使用Spring提供的以下两个注解，标注到单元测试类上，这样在类当中就可以使用@Autowired注解了。

@ExtendWith(SpringExtension.class)

@ContextConfiguration("classpath:spring.xml")



## 十八、Spring6集成MyBatis

### 18.1 实现步骤

- 第一步：准备数据库表

- - 使用t_act表（账户表）

- 第二步：IDEA中创建一个模块，并引入依赖

- - spring-context
  - spring-jdbc
  - spring-test
  - mysql驱动
  - mybatis
  - mybatis-spring：**mybatis提供的与spring框架集成的依赖**
  - 德鲁伊连接池
  - junit

- 第三步：基于三层架构实现，所以提前创建好所有的包

- - com.powernode.bank.mapper
  - com.powernode.bank.service
  - com.powernode.bank.service.impl
  - com.powernode.bank.pojo

- 第四步：编写pojo

- - Account，属性私有化，提供公开的setter getter和toString。

- 第五步：编写mapper接口

- - AccountMapper接口，定义方法

- 第六步：编写mapper配置文件

- - 在配置文件中配置命名空间，以及每一个方法对应的sql。

- 第七步：编写service接口和service接口实现类

- - AccountService
  - AccountServiceImpl

- 第八步：编写jdbc.properties配置文件

- - 数据库连接池相关信息

- 第九步：编写mybatis-config.xml配置文件

- - 该文件可以没有，大部分的配置可以转移到spring配置文件中。
  - 如果遇到mybatis相关的系统级配置，还是需要这个文件。

- 第十步：编写spring.xml配置文件

- - 组件扫描
  - 引入外部的属性文件
  - 数据源
  - SqlSessionFactoryBean配置

- - - 注入mybatis核心配置文件路径
    - 指定别名包
    - 注入数据源

- - Mapper扫描配置器

- - - 指定扫描的包

- - 事务管理器DataSourceTransactionManager

- - - 注入数据源

- - 启用事务注解

- - - 注入事务管理器

- 第十一步：编写测试程序，并添加事务，进行测试

### 18.2 具体实现

第二步：引入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.fengshun</groupId>
    <artifactId>spring6-009-mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>
    <dependencies>
        <!--spring 上下文-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.0.0</version>
        </dependency>
        <!--spring-jdbc-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>6.0.0</version>
        </dependency>
        <!--spring-test-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>6.0.0</version>
        </dependency>
        <!--mysql 驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.30</version>
        </dependency>
        <!--MyBatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.10</version>
        </dependency>
        <!--mybatis-spring：mybatis提供的与spring框架集成的依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>3.0.2</version>
        </dependency>
        <!--德鲁伊连接池-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.16</version>
        </dependency>
        <!--junit-->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.10.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

第二步：创建好所有的包

第三步：编写pojo 类

```java
package com.fengshun.bank.pojo;

public class Account {
    private Long id;
    private String actno;
    private Double balance;

    public Account() {
    }

    public Account(Long id, String actno, Double balance) {
        this.id = id;
        this.actno = actno;
        this.balance = balance;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public Double getBalance() {
        return balance;
    }

    public void setBalance(Double balance) {
        this.balance = balance;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", actno='" + actno + '\'' +
                ", balance=" + balance +
                '}';
    }
}
```

第四步：编写Mapper 接口

```java
package com.fengshun.bank.mapper;

import com.fengshun.bank.pojo.Account;

import java.util.List;

public interface AccountMapper {
    int insert(Account account);
    // 更加账号删除
    int deleteByActno(String actno);
    int update(Account account);
    Account selectByActno(String actno);
    List<Account> selectAll();
}
```

第五步：编写mapper配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.fengshun.bank.mapper.AccountMapper">
    <insert id="insert">
        insert into t_act values(null,#{actno},#{balance})
    </insert>
    <delete id="deleteByActno">
        delete from  t_act where actno=#{actno}
    </delete>
    <update id="update" parameterType="Account">
        update t_act  set balance=#{balance} where actno=#{actno}
    </update>
    <select id="selectByActno" resultType="Account">
        select id ,actno,balance from t_act where actno=#{actno}
    </select>
    <select id="selectAll" resultType="Account">
        select id,actno,balance from t_act
    </select>
</mapper>
```

第六步：编写service接口和service接口实现类

service接口

```java
package com.fengshun.bank.service;

import com.fengshun.bank.pojo.Account;

import java.util.List;

public interface AccountService {
    // 增加
    int save(Account  account);
    // 删除
    int deleteByActno(String actno);
    // 修改
    int modify(Account account);
    // 根据actno 查询
    Account getByActno(String actno);
    // 查询所有
    List<Account> getAll();
    // 转账
    void transfer(String fromActno,String toActno ,Double money);
}
```

service接口实现类

```java
package com.fengshun.bank.service.impl;

import com.fengshun.bank.mapper.AccountMapper;
import com.fengshun.bank.pojo.Account;
import com.fengshun.bank.service.AccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service("accountService")
@Transactional
public class AccountServiceImpl implements AccountService {
    @Autowired
    private AccountMapper accountMapper;

    @Override
    public int save(Account account) {
        return accountMapper.insert(account);
    }

    @Override
    public int deleteByActno(String actno) {
        return accountMapper.deleteByActno(actno);
    }

    @Override
    public int modify(Account account) {
        return accountMapper.update(account);
    }

    @Override
    public Account getByActno(String actno) {
        return accountMapper.selectByActno(actno);
    }

    @Override
    public List<Account> getAll() {
        return accountMapper.selectAll();
    }

    @Override
    public void transfer(String fromActno, String toActno, Double money) {
        Account fromAccount = accountMapper.selectByActno(fromActno);
        if (fromAccount.getBalance()<money) {
            throw new RuntimeException("余额不足");
        }
        Account toAccount = accountMapper.selectByActno(toActno);
        fromAccount.setBalance(fromAccount.getBalance()-money);
        toAccount.setBalance(toAccount.getBalance()+money);
        int i = accountMapper.update(fromAccount);
        i+=accountMapper.update(toAccount);
        if (i!=2){
            throw new RuntimeException("转账失败");
        }
    }
}
```

第七步：编写jdbc.properties配置文件

```
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis
jdbc.username=root
jdbc.password=051727
```

第八步：编写mybatis-config.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--帮助我们打印mybatis的日志信息-->
<settings>
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
</configuration>
```

第九步：编写spring.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--组件扫描-->
    <context:component-scan base-package="com.fengshun.bank"/>

    <!--引入外部的属性文件-->
    <context:property-placeholder location="jdbc.properties"/>

    <!--数据源-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!--SqlSessionFactoryBean配置-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
        <!-- 注入mybatis核心配置文件路径-->
        <property name="configLocation" value="mybatis-config.xml"/>
        <!-- 指定别名包-->
        <property name="typeAliasesPackage" value="com.fengshun.bank.pojo"/>
    </bean>

    <!--Mapper扫描配置器,主要扫描mapper接口，生成代理类-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--指定扫描的包-->
        <property name="basePackage" value="com.fengshun.bank.mapper"/>
    </bean>

    <!--事务管理器DataSourceTransactionManager-->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--启用事务注解-->
    <tx:annotation-driven transaction-manager="txManager"/>

</beans>
```

第十步：编写测试程序，并添加事务，进行测试

```java
package com.fengshun.spring6.test;

import com.fengshun.bank.pojo.Account;
import com.fengshun.bank.service.AccountService;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import java.util.List;

@ExtendWith(SpringExtension.class)
@ContextConfiguration("classpath:spring.xml")
public class SmTest {
    @Autowired
    private AccountService accountService;
    @Test
    public void testSm(){
        List<Account> all = accountService.getAll();
        all.forEach(account -> System.out.println(account));
    }

}
```

### 18.3 Spring配置文件的import

spring配置文件有多个，并且可以在spring的核心配置文件中使用import进行引入，我们可以将组件扫描单独定义到一个配置文件中，如下：

common.xml

~~~xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--组件扫描-->
    <context:component-scan base-package="com.powernode.bank"/>

</beans>
~~~

然后在核心配置文件中引入：

~~~xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--引入其他的spring配置文件-->
    <import resource="common.xml"/>

</beans>
~~~



## 十九、Spring中的八大模式

### 19.1 简单工厂模式

BeanFactory的getBean()方法，通过唯一标识来获取Bean对象。是典型的简单工厂模式（静态工厂模式）；

### 19.2 工厂方法模式

FactoryBean是典型的工厂方法模式。在配置文件中通过factory-method属性来指定工厂方法，该方法是一个实例方法。

### 19.3 单例模式

Spring用的是双重判断加锁的单例模式。请看下面代码，我们之前讲解Bean的循环依赖的时候见过：

![img](C:\Users\fengshun\Desktop\自学\主流框架\spring\图片\1666663352271-4ba8d737-1e32-4f0e-b01a-aa305ad3abea.png)

### 19.4 代理模式

Spring的AOP就是使用了动态代理实现的。

### 19.5 装饰器模式

JavaSE中的IO流是非常典型的装饰器模式。

Spring 中配置 DataSource 的时候，这些dataSource可能是各种不同类型的，比如不同的数据库：Oracle、SQL Server、MySQL等，也可能是不同的数据源：比如apache 提供的org.apache.commons.dbcp.BasicDataSource、spring提供的org.springframework.jndi.JndiObjectFactoryBean等。

这时，能否在尽可能少修改原有类代码下的情况下，做到动态切换不同的数据源？此时就可以用到装饰者模式。

Spring根据每次请求的不同，将dataSource属性设置成不同的数据源，以到达切换数据源的目的。

**Spring中类名中带有：Decorator和Wrapper单词的类，都是装饰器模式。**

### 19.6 观察者模式

定义对象间的一对多的关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并自动更新。Spring中观察者模式一般用在listener的实现。

Spring中的事件编程模型就是观察者模式的实现。在Spring中定义了一个ApplicationListener接口，用来监听Application的事件，Application其实就是ApplicationContext，ApplicationContext内置了几个事件，其中比较容易理解的是：ContextRefreshedEvent、ContextStartedEvent、ContextStoppedEvent、ContextClosedEvent

### 19.7 策略模式

策略模式是行为性模式，调用不同的方法，适应行为的变化 ，强调父类的调用子类的特性 。

getHandler是HandlerMapping接口中的唯一方法，用于根据请求找到匹配的处理器。

比如我们自己写了AccountDao接口，然后这个接口下有不同的实现类：AccountDaoForMySQL，AccountDaoForOracle。对于service来说不需要关心底层具体的实现，只需要面向AccountDao接口调用，底层可以灵活切换实现，这就是策略模式。

### 19.8 模板方法模式

Spring中的JdbcTemplate类就是一个模板类。它就是一个模板方法设计模式的体现。在模板类的模板方法execute中编写核心算法，具体的实现步骤在子类中完成。

