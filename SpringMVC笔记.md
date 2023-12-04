# 一、SpringMVC 概述

## 1.1 SpringMVC 简介

 SpringMVC 也叫Spring web mvc。是Spring 框架的一部分，是在Spring3.0 后发布的。它是专门做web开发的，可以理解是Servlet的一个升级。

 定义：它是基于MVC开发模式订单框架，用来优化控制器，它是Spring家族的一员，它具备IoC和AOP。SpringMVC能够创建对象，放入到容器中（SpringMVC容器），SpringMVC容器中放的是控制器对象。

我们要做的是 使用@Controller创建控制器对象，把对象放到SpringMVC容器中，把创建的对象作为控制器使用这个控制器对象能够接受用户的请求，显示处理结果，就当做是一个Servlet使用。

使用@Controller 注解创建的是一个普通类的对象，不是servlet，SpringMVC赋予了控制器对象的一些额外的功能。

SpringMVC 是如何处理用户请求的？

> web开发底层是servlet，SpringMVC中有一个对象是Servlet：DispatherServlet
>
> DispatherServlet：负责接受用户的所有请求，用户把请求给了DispatherServlet，之后DispatherServlet把请求转发给我们的Controller对象，最后是Controller对象处理请求。

 SSM框架：
 >S：Spring  用于整合SpringMVC 和 MyBatis 框架
 >S:SpringMVC  用于优化Controller层
 >M：MyBatis  用于优化Dao层

## 1.2 SpringMVC的优点

 1. 轻量级，基于MVC的框架

   ​	基于 MVC 架构，功能分工明确。解耦合。

 2. 易于上手，容易理解，功能强大

   ​	就可以开发一个注解的 SpringMVC 项目，SpringMVC 也是轻量级的，jar 很小。不依赖的特定的接口和类。

 3. 完全基于注解开发

 4. 作为Spring的一般分，能够使用Spring的IoC 和 AOP

 5. SpingMVC强化注解的使用


## 1.3 SpringMVC优化的方向

![image-20230902224321885](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230902224321885.png)

## 1.4 SpringMVC 执行流程

![image-20230902224554095](C:\Users\fengshun\Desktop\自学\spring\图片\image-20230902224554095.png)

执行流程说明：

1) 向服务器发送HTTP请求，请求被前端控制器 DispatcherServlet 捕获。

2) DispatcherServlet 根据<servlet-name>中的配置对请求的URL进行解析，得到请求资源标识符（URI）。然后根据该URI，调用 HandlerMapping 获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以 HandlerExecutionChain 对象的形式返回。

3) DispatcherServlet 根据获得的Handler，选择一个合适的 HandlerAdapter。

4) 提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作：

> HttpMessageConveter：将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息。
>
> 数据转换：对请求消息进行数据转换。如String转换成Integer、Double等。
>
> 数据格式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等。
>
> 数据验证：验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中。

5) Handler(Controller)执行完成后，向 DispatcherServlet 返回一个 ModelAndView 对象。

6) 根据返回的ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到Spring容器中的ViewResolver)返回给DispatcherServlet。

7) ViewResolver 结合Model和View，来渲染视图。

8) 视图负责将渲染结果返回给客户端

## 1.5 第一个SpringMVC程序

第一步：新建项目

>  	使用Maven，选择app模板
>

第二步：添加目录

>  	添加缺失的test、java、resource，并修改目录属性
>

> <img src="C:\Users\fengshun\Desktop\自学\spring\图片\image-20230902230931908.png" alt="image-20230902230931908" style="zoom:67%;" />

第三步：添加依赖

> ​	添加spring-webmvc 依赖的时候，Maven 会自动引入spring依赖
>
> ```xml
> <dependencies>
>     <!--添加依赖-->
>     <dependency>
>         <groupId>org.springframework</groupId>
>         <artifactId>spring-webmvc</artifactId>
>         <version>6.0.0</version>
>     </dependency>
>     <dependency>
>         <groupId>javax.servlet</groupId>
>         <artifactId>javax.servlet-api</artifactId>
>         <version>4.0.1</version>
>     </dependency>
>     <dependency>
>         <groupId>junit</groupId>
>         <artifactId>junit</artifactId>
>         <version>4.11</version>
>         <scope>test</scope>
>     </dependency>
> </dependencies>
> <!--加载资源文件-->
> <build>
>     <resources>
>         <resource>
>             <directory>src/main/java</directory>
>             <includes>
>                 <include>**/*.xml</include>
>                 <include>**/*.properties</include>
>             </includes>
>         </resource>
>         <resource>
>             <directory>src/main/resources</directory>
>             <includes>
>                 <include>**/*.xml</include>
>                 <include>**/*.properties</include>
>             </includes>
>         </resource>
>     </resources>
> </build>
> ```
>



第四步：在web.xml 中注册SpringMVC框架

> 因为web的请求都是由Servlet来进行处理的，而SpringMVC的核心处理器就是一个DispatcherServlet，它负责接收客户端的请求，并根据请求的路径分派给对应的action（控制器）进行处理，处理结束后依然由核心处理器DispatcherServlet进行响应返回。
>
> > DispatcherServlet 叫做中央调度器，是一个Servlet，它的父类是继承HttpServlet
> >
> > DispatcherServlet也叫做前端控制器（front Controller）
> >
> > DispatcherServlet 负责接受用户提交的请求，调用其他的控制器对象，并把请求的处理结果显示给用户
> >
> > DispatcherServlet  是SpringMVC 框架自带的
>
> 需要在Tomcat服务器启动后，创建DispactherServlet 对象的实例
>
> ```
> 为什么要创建DispatcherServlet对象的实例？
>    因为DispatcherServlet在它的创建过程中，会同时创建springmvc容器对象，
>    读取springmvc的配置文件，把这个配置文件中的对象都创建好，当用户发起请求时，就可以直接使用对象了
> ```
>
> ```xml
> <?xml version="1.0" encoding="UTF-8"?>
> <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
>          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>          xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
>          version="5.0">
> 
>     <!--声明，注册Springmvc的核心对象DispacherServlet-->
>     <!-- 需要在Tomcat服务器启动后，创建DispactherServlet 对象的实例-->
>     <servlet>
>         <servlet-name>springmvc</servlet-name>
>         <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
>         <!--自定义springmvc读取配置文件的位置-->
>         <init-param>
>             <!--springmvc的配置文件的位置的属性-->
>             <param-name>contextConfigLocation</param-name>
>             <!--指定自定义文件的位置-->
>             <param-value>classpath:springmvc.xml</param-value>
>         </init-param>
> 
>         <!--在tomcat启动后，创建Servlet对象-->
>         <!--在Tomcat启动时进行加载-->
>         <load-on-startup>1</load-on-startup>
>         <!--load-on-startup:表示Tomcat启动后创建对象的顺序，它的值是整数，数值越小，Tomcat创建对象的时间越早。它的值是大于等于0的。-->
>     </servlet>
>     
>     <servlet-mapping>
>         <servlet-name>springmvc</servlet-name>
>         <!--使用框架的时候，url-pattern 可以使用两种值：
> 				1.使用扩展名方式，语法*.xxx ，xxx是自定义的扩展名。常用的方式： *.do  *action  *.mvc 这写都是自						定义扩展名
> 						表示：http://localhost:8080/springmvc/some.do 以 .do 结尾的 都交给springmvc处理
> 				2.使用斜杠"/"
> -->
>         <url-pattern>*.action</url-pattern>
>     </servlet-mapping>
> </web-app>
> ```
>
> 使用Servlet 标签注册Springmvc的核心对象DispacherServlet。
>
> 想要让DispacherServlet 对象在tomcat服务器启动时就被创建，那么需要 使用 <load-on-startup>1</load-on-startup> 指定优先级
>
> 在启动tomcat服务器后，DispacherServlet 就会被初始化：
>
> ~~~
>  Servlet的初始化会执行init() 方法。DispatcherServlet在init()中{
>        //创建容器，读取配置文件
>        WebApplicationContext ctx = new ClassPathXmlApplicationContext("spring.xml");
>        //把容器对象放到ServletContext中
>        getServletContext().setAttribute(key,ctx);
>    }
> ~~~
>
> ​	这个init() 方法会寻找SpringMVC 的核心配置文件，如果没有指定SpringMVC 核心配置文件的位置以及名字，那么会自动在web//WEB-INF 中寻找配置文件，而这个配置文件的名字就是 <servlet-name> 标签的值+“-servlet.xml"。如果没有找到这个文件，那么就出错，出错如下：![image-20230903140226089](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230903140226089.png)
>
> 所以这时候，需要我们为servlet 指定SpringMVC 的核心配置文件的路劲。
>
> ~~~xml
>         <init-param>
>             <!--springmvc的配置文件的位置的属性-->
>             <param-name>contextConfigLocation</param-name>
>             <!--指定自定义文件的位置-->
>             <param-value>classpath:springmvc.xml</param-value>
>         </init-param>
> 
> ~~~
>
> ​	<param-value>classpath:springmvc.xml</param-value>表示从类路径下加载SpringMVC的配置文件，并且文件名为springmvc.xml。
>
> 

第五步：创建一个发起请求的页面 index.jsp

```jsp

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>mian</title>
</head>
<body>
<p>第一个springmvc项目</p>
<p><a href="some.do">发起some.do 的请求</a></p>

</body>
</html>
```

第六步：创建控制器类

> 1. 在类的上面加上@Controller 注解，创建对象，并放到SpringMVC容器中
> 2. 在类中的方法上面加入@RequestMapping 注解
>
> ```java
> package com.fengshun.service;
> 
> import org.springframework.stereotype.Controller;
> import org.springframework.web.bind.annotation.RequestMapping;
> import org.springframework.web.servlet.ModelAndView;
> 
> @Controller
> public class MyController {
>     /*    处理用户提交的1请求，在springmvc中是使用方法来处理的
>         方法是自定义的，可以有多种返回值，多种参数，方法名自定义*/
> 
>     // 使用dosome方法处理some.do 请求
>     @RequestMapping(value = "/some.do")
>     /*
>     数组形式
>     @RequestMapping(value = {"/some.do","other.do"})
>     */
>     public ModelAndView doSome() {
>        /* ModelAndView 表示本次请求的处理结果
>                 model：数据，请求处理完成后，要显示给用户的数据
>                 view：视图，例如jsp等*/
> 
>         // 处理some.do的请求，相当于servlet调用处理完成了
>         ModelAndView mv = new ModelAndView();
>         // 添加数据
> 
>         mv.addObject("msg", "欢迎使用springmvc");
>         mv.addObject("fun", "欢迎使用springmvc");
>         /*框架在请求的最后会把数据放到request作用域中
>         request.setAttribute("msg","欢迎使用springmvc");
>         request.setAttribute("fun","欢迎使用springmvc");
>         */
>         // 指定视图 ，指定视图的完整路劲
>         // 框架对视图执行的forword操作，reques.getRequestDispather("/show.jsp").forword(....)
>         mv.setViewName("/show.jsp");
>         // 返回mv
>         return mv;
>     }
> }
> ```
>
> 在springmvc中，是通过使用方法来处理用户提交的请求，方法是自定义的，方法名的参数、返回值类型、方法名都是自定义的。访问修饰符为public。
>
> 使用@RequestMapping 注解 标注方法来处理某个请求，它可以使用在类或者某个方法上。
>
> > 使用@RequestMapping 修饰的方法叫做处理器方法或者控制器方法，这个方法可以用于处理请求，类似于servlet中的doGet或doPost
>
> @RequestMapping 注解有多个属性：
>
> > - 第一个属性：value  ：它是一个字符串，表示请求的url地址（例如 /some.do）。value的值可以是数组，并且它的值必须是唯一的，不能重复。在使用时推荐以 " / "  开头
>
> 



第七步：创建一个作为结果的jsp，显示请求的处理结果

```jsp

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>显示请求结果的jsp</title>
</head>
<body>
<h3>show.jsp</h3><br>
获取数据
<p><u>msg数据：${msg}</u></p>
<p><u>fun数据：${fun}</u></p>
</body>
</html>
```

第八步：创建SpringMVC的配置文件

> 声明组件扫描器，指定@Controller 注解所在的包名
>
> 声明视图解析器。它是帮助处理视图的。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--添加包扫描-->
    <context:component-scan base-package="com.fengshun.controller"/>
    <!--添加视图解析器-->
<!--
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        &lt;!&ndash;配置前缀&ndash;&gt;
        <property name="prefix" value="/admin/"/>
        &lt;!&ndash;配置后缀&ndash;&gt;
        <property name="suffix" value=".jsp"/>
    </bean>-->
</beans>
```

第九步：测试

![image-20230903181320039](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230903181320039.png)

![image-20230903181331540](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230903181331540.png)

## 1.6 第一个程序的细节

以上的程序，SpringMVC 请求处理流程：

1. 发起some.do 请求

2. tomcat 收到请求后通过 web.xml 中的 url-patter 知道 *.do 的请求给 DispatcherServlet 进行处理。DispatcherServlet 根据springmvc.xml 配置就知道some.do的请求 交给dosome()方法进行处理。
3. DispatcherServlet 把some.do 转发给MyController.dosome() 方法
4. 框架执行 dosome() 方法，把得到的ModelAndView进行处理，转发到show.jsp

DispatcherServlet  中央调度器的作用：

> 1. 负责创建springmvc容器对象，读取xml配置文件，创建文件中的Controller对象
> 2. 负责接受用户的请求，分派给自定义的Controller对象

**拓展**：

> 在webapp 目录下的 jsp 文件，用户可以通过url 直接访问，没有任何限制，但是页面没有经过请求，所以只会显示固定内容。
>
> 怎么做才能让用户不能通过url直接访问，而只能访问index页面？
>
> ​	将jsp文件存放在 WEB-INF 目录下，这样用户通过url就不能直接访问到页面。也可以在WEB-INF 目录下再新建目录
>
> 修改地址
>
> ![image-20230903194617476](C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230903194617476.png)
>
> 修改代码
>
> ![image-20230903194835440](C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230903194835440.png)
>
> 通过url 无法直接访问：
>
> <img src="C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230903194818836.png" alt="image-20230903194818836" style="zoom:67%;" />
>
> 通过index页面访问
>
> <img src="C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230903194915897.png" alt="image-20230903194915897" style="zoom:67%;" />

在WEB-INF 目录下，可以有多级目录，这会导致我们编写的请求路径比较长，例如： mv.setViewName("/WEB-INF/view/show.jsp");

可以通过视图解析器，简化这一步骤：

> 在springmvc的核心配置文件中声明视图解析器 InternalResourceViewResolver 
>
> ```xml
>     <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
>         <!--配置前缀-->
>         <property name="prefix" value="/WEB-INF/view/"/>
>         <!--配置后缀-->
>         <property name="suffix" value=".jsp"/>
>     </bean>
> </beans>
> ```
>
> InternalResourceViewResolver  有两个属性：
>
> 	1. 前缀 prefix  ： 前缀必须以 " /"  开始，以" / " 结束。它代表的是视图文件的路径
> 	1. 后缀 suffix ： 后缀必须是 ".xxx" 代表的是视图文件的扩展名
>
> 当配置视图解析器后，可以使用逻辑名称（文件名）来指定视图:
>
> 框架会使用试图解析器的前缀 + 逻辑名称 + 后缀 组成完全路径 
>
>  ~~~JAVA
>  mv.setViewName("show");
>  ~~~

# 二、SpringMVC注解式开发

## 2.1@RequestMapping 定义请求规则

### 2.1.1 指定模块名称

使用 @RequestMapping 注解中的value属性指定的值叫模块名称。模块名称可以有多个。

<img src="C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230904101743318.png" alt="image-20230904101743318" style="zoom:67%;" />

```html
<a href="user/some.do">点击转发</a><br>
<a href="user/action.do">点击转发</a>
```

可以将 @RequestMapping 注解 放到类上：

<img src="C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230904102020069.png" alt="image-20230904102020069" style="zoom:67%;" />

注意： value 的值尝尝以 "/" 开头

### 2.1.2 指定请求方式

@RequestMapping 中有个属性 method 它表示请求的方式，它的值是RequestMethod类枚举值。

例如 表示get请求时，是 RequestMethod.GET ; 表示post 请求时，是 RequestMethod.POST

![image-20230904103625664](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230904103625664.png)

​	如果页面发送的是post 请求，但是方法中定义的是GET 请求， 那么会出现 405 错误。同理，前端发送的是psot 请求，后端的方法设置的是get请求，同样也会出现错误。

 @RequestMapping 中的method 属性可以不设置，程序会更加前端发送的请求方式进行改变：

![image-20230904104204421](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230904104204421.png)

![image-20230904104308975](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230904104308975.png)

以上两种请求方式都是可以的。

## 2..2 处理器方法的参数

处理器方法可以包含以下四类参数，这些参数会在系统调用时有系统自动赋值，即程序员在方法中直接使用即可。

> 1. HttpServletRequest
> 2. HttpServletResponse
> 3. HttpSession
> 4. 请求中所携带的请求参数

1. 使用前三种参数：

   ```java
   @RequestMapping(value = "/other.do")
   public ModelAndView test3(HttpServletRequest request, HttpServletResponse response, HttpSession session)  {
       ModelAndView modelAndView = new ModelAndView();
   
       modelAndView.addObject("message","获取请求参数："+request.getParameter("name"));
   
       modelAndView.setViewName("show");
       return modelAndView;
   }
   ```

下面主要讲，请求中所携带的请求参数：

### 2.2.1 单个数据注入

#### 单个数据注入

**处理器方法的形参名和请求中参数名必须一致**，同名的请求参数会赋值给同名的形参

发送请求：

```jsp
<h1>逐个接受请求参数</h1>
<form action="user/first.do">
    用户名：<input type="text" name="name" placeholder="请输入用户名">
    年龄：<input type="text" name="age" placeholder="请输入年龄">
    <input type="submit" value="提交">
</form>
```

接受参数：

```java
// 逐个接受请求参数
@RequestMapping(value = "/first.do")
public ModelAndView test4(String name,int age)  {
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("message","逐个获取请求参数： name=="+name+", age=="+age);
    modelAndView.setViewName("show");
    return modelAndView;
}
```

框架接收请求参数原理：

> 1. 使用request 对象接受请求参数：
>
>    String name = request.getParamenter("name");
>
>    String age = request.getParamenter("age");
>
> 2. SpringMVC 框架通过 DispatherServlet 调用 test4 方法。调用方法时，按名称对应，把接受的参数赋值给形参。
>
>    调用： test(String name , Integer.valueOf(age) )
>
>    框架会提供类型转换的功能，能把String 类型转换为 ： int ， long , float , double 等类型。

注意：如果形参本来就是String 类型，那么在发送请求时，请求参数为空字符串时不会出错。但是如果形参是其他类型，请求参数依旧是空字符串，那么浏览器会显示 400 错误，在控制台中，虽然不会报错，但是SpringMVC 框架会把错误写进日志信息中：

如下是错误信息：

~~~
警告 [http-nio-8081-exec-93] org.springframework.web.servlet.handler.AbstractHandlerExceptionResolver.logException Resolved [org.springframework.web.method.annotation.MethodArgumentTypeMismatchException: Failed to convert value of type 'java.lang.String' to required type 'int'; For input string: ""]

~~~

但是在实际开发中，经常会出现提交的数据是空字符串，为了避免这一类型事件的发生，可以设置形参的类型为包装类。

如果是空字符串，包装类会将空字符串转换为null

![image-20230904112705713](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230904112705713.png)

运行结果：

![image-20230904112722189](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230904112722189.png)

------

#### 解决乱码问题

1. 在传统的方式中，可以使用servlet设置编码方式：

   ```java
   public void  doget(HttpServletRequest request){
       request.setCharacterEncoding("utf-8");
   }
   ```

   但是这种方式，需要方法中有 HttpServletRequest 参数，并且每个方法中都需要设置。

2. 在过滤器中设置编码方式：

   过滤器可以自定义，也可以使用框架中提供的过滤器：CharacterEncodingFilter

   源码分析：

   ![image-20230904114131765](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230904114131765.png)

   底层源代码：<img src="C:\Users\fengshun\AppData\Roaming\Typora\typora-user-images\image-20230904120507449.png" alt="image-20230904120507449" style="zoom:67%;" />、

CharacterEncodingFilter 中的三个值：

> 1. encoding ： 表示的是项目中需要使用的字符集编码。
> 2. forceRequestEncoding ： 布尔类型，默认值为false ，当为true 时 表示强制请求对象（HttpServletRequest) 使用 encoding 编码的值
> 3. forceResponseEncoding ： 布尔类型，默认值为false ，当为true 时 表示强制应答对象（HttpServletResponse) 使用 encoding 编码的值

在web.xml 文件中配置过滤器：

```xml
<!--注册声明过滤器，解决中文乱码问题-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <!--设置项目中字符集编码-->
    <init-param>
        <param-name>encoding=</param-name>
        <param-value>utf-8</param-value>
    </init-param>
    <!--强制请求对象（HttpServletRequest) 使用 encoding 编码的值-->
    <init-param>
        <param-name>forceRequestEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
    <!--强制应答对象（HttpServletResponse) 使用 encoding 编码的值-->
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <!--/* 表示强制所有请求先通过过滤处理-->
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

#### 矫正请求参数名 @RequestParam

所谓矫正请求参数名，指的是请求URL 所携带的参数名称和处理方法中指定的参数名不相同时，需要在处理办法参数前，添加一个注解 **@RequestParam(" 请求参数名" )**。

这个注解是用来解决请求中参数名和形参名不一样的问题。

@RequestParam 的参数：

>  value 属性：指的是请求中参数名称。
>
> required 属性：是一个boolean 类型的，默认值为true，表示请求中必须包含此参数

![image-20230904142028720](C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230904142028720.png)

### 2.2.2 对象接收参数

对象接受参数，其实就是使用自定义的 java 对象。

处理器方法形参是java对象，这个对象的属性名和请求中参数名是一样的。形参中可以有多个java对象，它们互不干扰。

框架会创建形参的java对象，给属性赋值。例如：请求中的参数是name，那么框架会调用 setName() 方法给属性赋值，这种操作类型去set注入。

定义java对象：

```java
package pojo;

public class User {
    private String name;
    private Integer age;
    //必须要无参构造方法
    public User() {
    }
    //有参构造方法
    //set 和 get方法 必须的
    //tostring方法
}
```

测试代码：

```java
// 使用自定义实体类作为形参
@RequestMapping(value = "/testpojo.do")
public ModelAndView test5(User user)  {
    ModelAndView modelAndView = new ModelAndView();

    modelAndView.addObject("message","获取请求参数："+user);

    modelAndView.setViewName("show");
    return modelAndView;
}
```

2.2.3 动态占位符

这种方式仅限于超链接或者地址栏。这种方式在请求路径中需要“ /参数/参数 ”

需要使用 @PathVariable 注解进行解析。

发送请求

```html
<a href="${pageContext.request.contextPath}/user/testa/张三/18.do">点击提交</a>
```

处理器方法：

```java
// 动态占位符
@RequestMapping(value = "/testa/{name}/{age}.do")
public ModelAndView testa(@PathVariable String name, @PathVariable Integer age)
{
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("message","逐个获取请求参数： name=="+name+", age=="+age);
    modelAndView.setViewName("show");
    return modelAndView;
}
```

执行结果：

<img src="C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230904150618633.png" alt="image-20230904150618633" style="zoom:67%;" />

## 2.3 处理器方法的返回值

### 2.3.1 返回 ModelAndView

如果处理器方法处理完成后，既需要传递参数，又需要跳转页面，那么可以返回 ModelAndView 对象。

model 代表的是模块，view 即视图。如果只需要跳转页面或者只需要传递参数，那么会浪费另一方的资源，此时就不再建议使用 ModelAndView 

```java
@RequestMapping(value = "/testpojo.do")
public ModelAndView test5(User user)  {
    ModelAndView modelAndView = new ModelAndView();

    modelAndView.addObject("message","获取请求参数："+user);

    modelAndView.setViewName("show");
    return modelAndView;
}
```



### 2.3.2 返回String

​	返回值是字符串，代表的是 ModelAndView 中的 View 视图，它可以指定逻辑视图名，通过视图解析器解析可以将其转换为物理视图地址。同时也可以是完整的视图地址。

```java
@RequestMapping("/testString.do")
public String testString(){
    System.out.println("成功跳转到/view/show.jsp");
    return "show";
}
```

### 2.3.3 返回void

​	对于处理器方法返回 void 的应用场景，应用在AJAX 响应处理。若处理器对请求处理后，无需跳转到其它任何资源，此时可以让处理器方法返回 void。我们SSM整合案例中的分页使用的就是无返回值。

发送ajax 请求

```javascript

    $("#but").on("click",function (){
        alert("dshjjlk")
       $.ajax({
            uri:"${pageContext.request.contextPath}user/testVoid.do",
            data:{
                name:"张三",
                age:18
            },
            type:"post",
            dataType:"json",
            success:function (resp){
                alert(resp)
            },
            error:function (){
                console.log("...")
            }
        })

    })

```

处理方法：

```java
// 当方法返回值为void
@RequestMapping("/testVoid.do")
public void testVoid(User user,HttpServletResponse response) throws Exception {
    //将字符串转换为json格式
    ObjectMapper objectMapper = new ObjectMapper();
    String s = objectMapper.writeValueAsString(user);
    //响应
    PrintWriter writer = response.getWriter();
    writer.println(s);
    writer.flush();
    writer.close();

}
```

### 2.3.4返回对象Object

​	处理器方法也可以返回 Object 对象。这个 Object 可以是 Integer，自定义对象，Map，List 等。但返回的对象不是作为逻辑视图出现的，而是作为直接在页面显示的数据出现的。返回对象，需要使用@ResponseBody 注解，将转换后的 JSON 数据放入到响应体中。Ajax请求多用于Object返回值类型。由于转换器底层使用了Jackson 转换方式将对象转换为JSON 数据，所以需要添加Jackson的相关依赖。

SpringMVC 框架默认是使用 jackson

使用ajax请求返回一个JSON结构的用户

> 1. 添加依赖：
>
>    ~~~xml
>    <dependency>
>          <groupId>com.fasterxml.jackson.core</groupId>
>          <artifactId>jackson-databind</artifactId>
>          <version>2.9.8</version>
>        </dependency>
>    ~~~
>
> 
>
> 2. 添加jQuery的函数库,在webapp目录下,新建js目录,拷贝jquery-3.3.1.js到目录下
>
> 3. 在页面添加jQuery的函数库的引用
>
>    ~~~html
>     <script src="js/jquery-3.3.1.js"></script>
>    ~~~
>
> 4. 发送ajax请求
>
>    ~~~javascript
>     $("#but").on("click",function (){
>            alert("dshjjlk")
>           $.ajax({
>                uri:"${pageContext.request.contextPath}user/testObject.do",
>                data:{
>                    name:"张三",
>                    age:18
>                },
>                type:"post",
>                dataType:"json",
>                success:function (resp){
>                    alert(resp)
>                },
>                error:function (){
>                    console.log("...")
>                }
>            })
>    ~~~
>
> 5. 在springmvc.xml文件中添加注解驱动
>
>    ~~~xml
>    <mvc:annotation-driven></mvc:annotation-driven>
>    ~~~
>
>    添加上注解驱动后，类似于：
>
>    ```
>    ObjectMapper objectMapper = new ObjectMapper();
>    String json = objectMapper.writeValueAsString(user);
>    ```
>
> 6. 编写处理器方法：
>
>     在处理器方法上添加 @ResponseBody 注解
>
>    @Response注解类似于：
>
>    ```java
>    response.setContentType("application/json;charset=utf-8");
>    PrintWriter writer = response.getWriter();
>    writer.println(s);
>    ```
>    
>    ```java
>    // 当方法返回值为User
>    @RequestMapping("/testObject.do")
>    @ResponseBody
>    public User testObject(String name,Integer age) throws Exception {
>        User user = new User(name, age);
>        System.out.println(user);
>        return user;
>    }
>    ```
>    
>    除了可以返回实体类对象以外，还能返回list集合
>    
>    ```java
>    // 当方法返回值为LIst
>    @RequestMapping("/returnListJson.do")
>    @ResponseBody
>    public List<User> testList(User users) throws Exception {
>        List<User> list = new ArrayList<>();
>        list.add(users);
>        return list;
>    }
>    ```



------

SpringMVC 处理器方法返回Object，可以转换为json输出到浏览器，响应ajax的内部原理：

> 1. < mvc:annotation-driven >   注解驱动：
>
>    - ​	注解驱动实现的功能是：通过HttpMessageConverter 接口以及其7个实现类完成java对象到 json，xml，text，二进制等数据格式的转换。
>
>    - HttpMessageConverter 接口：消息转换器。这个接口定义了java对象转为json、xml等数据格式的方法。这个方法有很多实现类，这些实现类完成 java对象到json、java对象到xml、java对象到二进制数据的转换
>
>    - HttpMessageConverter 接口 源码：
>
>      ```java
>      //下面两个方法就是控制器类把结果输出给浏览器时使用的：
>      boolean canWrite(Class<?> clazz, @Nullable MediaType mediaType);
>      void write(T t, @Nullable MediaType contentType, HttpOutputMessage outputMessage);
>      /*
>      	1.canWrite 作用：检查处理器方法的返回值，能不能转换为 mediaType 表示的数据格式。
>      		 mediaType 表示的数据格式，例如json、xml等
>      	2.write 作用：把处理器方法的返回值对象，调用jackson中的ObjectMapper 转换为mediaType字符串。
>      */
>      ```
>
> 2.  @ResponseBody 注解
>
>    这个注解是放在处理器方法上面的，通过HttpServletResponse 输出数据，响应ajax请求的。

返回对象框架的处理流程：

> 1. 框架会把返回的 User 类型,调用框架中的ArrayList<HttpmessageConverter> 中每个类的 CANWrite() 方法，检查哪个HttpMessageConverter 接口的实现类能处理 User 类型的数据 （MappingJackson2HttpMessageConverter 实现类） 
> 2. 框架会调用实现类的write() ，例如：MappingJackson2HttpMessageConverter 的 write() 方法 把User 对象转为json(实际就是调用Jackson 的 ObjectMappier 实现转为 json) 
> 3. 框架会调用 @ReponseBody 把 2 的结果数据输出到浏览器，ajax请求处理完成。

### 2.3.5 解决中文乱码问题

在相应到浏览器中的如果是 text 格式的数据，可能会出现中文乱码问题

在 @RequestMapping 注解中，使用 produces 属性，用来指定新的contextType

```java
@RequestMapping(value = "/returnListJson.do",produces = "text/plain;charset=utf-8")
```

## 2.4 解读 url-pattern 

### 2.4.1 配置详情

tomcat 本身能处理静态资源的访问，像html 、 图片、js文件 等都是静态资源。

tomcat 的web.xml 文件中有一个 servlet 名称是 defult，在服务器启动时创建

~~~xml
<servlet>
	<servlet-name>default</servlet-name>
	<servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
	<init-param>
		<param-name>debug</param-name>
		<param-value>0</param-value>
	</init-param>
	<init-param>
		<param-name>listings</param-name>
		<param-value>false</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>defult</servlet-name> 
    <url-pattern>/</url-pattern>   <!--表示静态资源和未映射的请求都交给这个 defult 处理-->
</servlet-mapping>
~~~

defult 这个servlet的作用：

> 1. 处理静态资源。
> 2. 处理未映射到其他Servlet的请求。

当项目中的DispatcherServlet 的 url-pattern 使用了 "/" ，那么它会代替 Tomcat 中的 defult。

```xml
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

如果单纯的只是这样设置，这会导致所有的静态资源都给 DispatcherServlet 处理， 默认情况下 DispatcherServlet 没有处理静态资源的能力。没有控制器对象能处理静态资源的访问。所以浏览器访问静态资源都会 404 错误。

### 2.4.2 静态资源访问

**（1）使用 < mvc:defult-servlet-handler/>** 

​		在springMVC 配置文件中加入了 < mvc:defult-servlet-handler/>  标签之后，框架会创建控制器对象 DefultServletRequestHandler （类似于我们自己创建的控制器类）。DefultServletRequestHandler  这个对象可以把接收到的请求转发给 tomcat 的 defult 这个servlet。

注意： < mvc:defult-servlet-handler/>  和  @RequestMapping 注解有冲突，需要加入注解驱动才能请求非静态资源（< mvc:annotation-driven/>）

**（2）使用< mvc:resources/>**

​		< mvc:resources/> 的作用：在SpringMVC 的配置文件中加入< mvc:resources/> 后，框架会创建 ResourceHttpRequestHandler 这个处理器对象。让这个对象处理静态资源的访问，不依赖tomcat 服务器。

它有两个属性：

> mapping ： 访问静态资源的uri 地址，使用通配符 " ** "
>
> location ： 静态资源在你的项目中的目录位置

```xml
<mvc:resources mapping="/images/**" location="/images/"/>
<mvc:resources mapping="/html/**" location="/html/"/>
<!--
其中 /images/** 表示：images/p1.jpg images/user/logn.jpg  等images 文件下的所有文件。  
	/images/ 中的第一个目录代表的是根目录
如果文件较多，可以使用多个标签.
-->
<!--想要只配置一哥标签可以将所有文件放在一个文件夹中
<mvc:resources mapping="/static/**" location="/static/"/>
-->
```

注意： < mvc:resources/>  和  @RequestMapping 注解有冲突，需要加入注解驱动才能请求非静态资源（< mvc:annotation-driven/>）

## 2.5 请求路径中是否以斜杠开头

​	地址分类：

> 1. 绝对地址：带有协议名称的是绝对地址，例如 http://localhost:8081/SpringMVc_002_requestMapping/ 
>
> 2. 相对地址：没有协议开头的，例如 user/some.do  /user/action.do
>
>    相对地址不能独立使用，必须有一个参考地址，通过参考地址+相对地址本身才能指定资源。

参考地址：

> 1. 在相对地址中，不以  " / "  开头
>
>    ​	在页面向 user/index.do 发起请求之后，访问地址 = 当前地址 + 链接地址。例如，当前地址为： http://localhost:8081/SpringMVc_002_requestMapping/   连接地址为 ：user/index/do   访问地址为： http://localhost:8081/SpringMVc_002_requestMapping/user/index.do
>
> 2. 在相对地址中，以 " / "  开头
>
>    ​	访问地址 = 域名 + 链接地址。 例如 域名为：http://localost:8081   链接地址为 : /user/some/do   访问地址为：http://localost:8081/user/some/do

如果在 http://localost:8081/user/some/do 页面中，再次请求 user/some.do 资源。那么访问地址就会变成：http://localost:8081/user/some/user/some.do  ，怎么样才能解决这样的问题：

> 1. 使用 " / " 开头的相对地址
>
> 2. 使用 base 标签
>
>    ​	base 标签位于 header 标签中。
>
>    ```html
>    <header>
>        <base ref="http://localhost:8080/user"
>    </header>
>    <body>
>        <a ref="user/some.do"></a>    
>    </body>
>    ```
>
>    原理：base标签中的值加上请求的路径。
>
>     

## 2.5 SpringMVC的四种跳转方式

​	传统的 Servlet 的跳转方式有两种：一种是请求转发，一种是重定向。请求转发是通过服务器进行的，重定向是不经过服务器的。

​	在 SpringMVC 中的四种跳转方式，其实就是传统的两种转发方式的衍生。

> 1. 重定向到页面
> 2. 重定向到 some.do 或其他资源
> 3. 请求转发到页面
> 4. 请求转发到资源

​	在 SpringMVC 中，默认使用的是请求转发，指定跳转方式为**重定向**可以使用 " forword: " ,  **请求转发** 使用： " redirct: "  

​	" forword: "  和 " redirct: "   能自动屏蔽掉视图解析器中的前缀后后缀的拼接。

请求：

```html
<a href="${pageContext.request.contextPath}/one.do">请求转发到页面</a>
<a href="${pageContext.request.contextPath}/two.do">请求转发到资源</a>
<a href="${pageContext.request.contextPath}/threen.do">重定向到页面</a>
<a href="${pageContext.request.contextPath}/four.do">重定向到资源</a>
```

处理器类：

```java
package controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class TestForwardOrRedirect {
    // 请求转发到页面
    @RequestMapping("one.do")
    public String testOne() {
        System.out.println("这是请求转发到页面");
        return "test";
    }
    // 请求转发到资源
    @RequestMapping("two.do")
    public String testTwo() {
        System.out.println("这是请求转发到资源");
        return "redirect:user/some.do";
    }
    // 重定向到页面
    @RequestMapping("threen.do")
    public String testThree() {
        System.out.println("这是重定向到页面");
        return "test";
    }
    // 重定向到资源
    @RequestMapping("four.do")
    public String testFour() {
        System.out.println("这是重定向到资源");
        return "forward:user/some.do";
    }
}
```

## 2.7 日期处理

日期类型不能自动注入到方法的参数中。需要单独做转换处理。使用@DateTimeFormat注解，需要在springmvc.xml文件中添加<mvc:annotation-driven/ >标签。

### 2.7.1 注入日期

####  @DateTimeFormat 注解

 	@DateTimeFormat 注解，可以使用在方法的参数上，也可以使用在 set方法上。

​	 发起请求：

```html
<form action="user/testdate.do">
    <input type="date" name="myDate"/>
    <input type="submit" value="提交"/>
</form>
```

​	处理请求：

```java
@RequestMapping(value = "/testdate.do")
public String mydate(@DateTimeFormat(pattern = "yyyy-MM-dd") Date myDate){
    DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
    System.out.println(dateFormat.format(myDate));
    return "show";
}
```

####  @InitBinder注解

​	在一个类的诸多方法中，都需要使用 @DateTimeFromat 注解 ，这样写出来的代码并不简介，可以使用 @ InitBinder 注解，这样在类中出现的所有日期都能被转换。并且使用了该注解，不需要在springmvc的核心配置文件中配置注解驱动< mvc:annotation-driver/ >

​	想要使用 @InitBinder 注解，单独定义一个方法。

```java
@InitBinder
public void initBinder(WebDataBinder dataBinder){
    DateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
    dataBinder.registerCustomEditor(Date.class,new CustomDateEditor(simpleDateFormat,true));
}
```

### 2.7.2 显示日期

####  使用 jstl 在jsp页面中处理日期

在 jsp 中，如何 Date 格式 转换

第一步：添加 jstl 依赖

~~~xml
<dependency>
	<groupId>jstl</groupId>
	<artifactId>jstl</artifactId>
	<version>1.2</version>
</dependency>
~~~

第二步：导入国际化标签

~~~jsp
<!--导入jstl 核心标签库-->
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!--导入jstl 格式化标签库-->
<%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
~~~

第三步：使用标签显示日期

~~~jsp
<div id="stulistgood">
	<c:forEach items="${list}" var="stu">
		<p>${stu.name}-------${stu.age}-------<fmt:formatDate value="${stu.date}" pattern="yyyy-MM-dd"></fmt:formatDate></p>
	</c:forEach>
</div>
~~~

除了使用这种方式以外，还可以在实体类中，修改。

#### JSON 中的日期显示

在类的成员变量的get 方法上进行处理

~~~java
@JsonFormat(pattern="yyyy-MM-dd HH:mm:ss")
public Date getDate() {
	return date;
}
~~~

## 2.8 < mvc:annotation-driven/ > 标签的使用

< mvc:annotation-driven/>会自动注册两个bean，分别为 DefaultAnnotationHandlerMapping  和  AnnotationMethodHandlerAdapter。是springmvc 为 @controller 分发请求所必须的。除了注册了这两个 bean，还提供了很多支持。

1）支持使用ConversionService 实例对表单参数进行类型转换；

 2）支持使用 @NumberFormat 、@DateTimeFormat；

 3）注解完成数据类型的格式化；

 4）支持使用 @RequestBody 和 @ResponseBody 注解；

 5）静态资源的分流也使用这个标签;

# 三、SpringMVC 拦截器

## 3.1 拦截器介绍

​	SpringMVC 中的 Interceptor 拦截器，它的主要作用是拦截指定的用户请求，并进行相应的预处理与后处理。其拦截的时间点在“处理器映射器根据用户提交的请求映射出了所要执行的处理器类，并且也找到了要执行该处理器类的处理器适配器，在处理器适配器执行处理器之前”。当然，在处理器映射器映射出所要执行的处理器类时，已经将拦截器与处理器组合为了一个处理器执行链，并返回给了中央调度器。

### 3.1.1  拦截器的应用场景

> - ​	日志记录：记录请求信息的日志
> - ​	权限检查，如登录检查
> - ​	性能检测：检测方法的执行时间

### 3.1.2 拦截器的执行原理

![image-20230911155545943](C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230911155545943.png)

### 3.1.3 拦截器的执行时机

> 1) preHandle()	:	在请求被处理之前进行操作
> 2) postHandle()	:	在请求被处理之后,但结果还没有渲染前进行操作,可以改变响应结果
> 3) afterCompletion   :    所有的请求响应结束后执行善后工作,清理对象,关闭资源

### 3.1.4  拦截器实现了两种方式 

> 1. ​	继承HandlerInterceptorAdapter的父类
> 2. ​	实现HandlerInterceptor接口,实现的接口,推荐使用实现接口	的方式

------

## 3.2 HandlerInterceptor 接口分析

源码;

​			<img src="C:\Users\fengshun\AppData\Roaming\Typora\typora-user-images\image-20230911164759165.png" alt="image-20230911164759165" style="zoom:67%;" />

###  3.2.1 preHandle

​	该方法在处理器方法执行之前执行。其返回值为 boolean，若为 true，则紧接着会执行处理器方法，且会将 afterCompletion()方法放入到一个专门的方法栈中等待执行。

### 3.2.2 postHandle

​	该方法在处理器方法执行之后执行。处理器方法若最终未被执行，则该方法不会执行。由于该方法是在处理器方法执行完后执行，且该方法参数中包含 ModelAndView，所以该方法可以修改处理器方法的处理结果数据，且可以修改跳转方向。	

### 3.2.3 afterCompletion

​	当preHandle()方法返回 true 时，会将该方法放到专门的方法栈中，等到对请求进行响应的所有工作完成之后才执行该方法。即该方法是在中央调度器渲染（数据填充）了响应页面之后执行的，此时对 ModelAndView 再操作也对响应无济于事。afterCompletion 最后执行的方法，清除资源，例如在 Controller 方法中加入数据等。

### 3.2.4 三个方法的参数

HttpServletRequest request,     ： 请求

 HttpServletResponse response,  ： 相应

 Object handler    : 被拦截的控制器对象，



------

## 3.3 拦截器的实现步骤

1. 修改web.xml文件中请求路径

![image-20230911162309958](C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230911162309958.png)

2. 将所有的页面放入WEB-INF目录下

3. 开发登录的处理器类

   ![image-20230911164455965](C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230911164455965.png)

4. 开发拦截器

   ![image-20230911164541907](C:\Users\fengshun\Desktop\自学\主流框架\SpringMVC\图片\image-20230911164541907.png)

5. 配置springmvc.xml 文件

   ~~~xml
   <!--注册拦截器-->
   <mvc:interceptors>
       <mvc:interceptor>
           <!--配置拦截的路径(哪些请求被拦截)-->
           <mvc:mapping path="/**"/>
           <!--设置放行的请求-->
           <mvc:exclude-mapping path="/login"></mvc:exclude-mapping>
           <mvc:exclude-mapping path="/showLogin"></mvc:exclude-mapping>
           <!--设置进行功能处理的拦截器类-->
           <bean class="com.bjpowernode.interceptor.LoginInterceptor"></bean>
       </mvc:interceptor>
   </mvc:interceptors>
   ~~~

   ------

# 四、SSM整合

## 4.1 SSM整合后台功能

1. 新建Maven项目，添加依赖
   
      > ```xml
      > <?xml version="1.0" encoding="UTF-8"?>
      > 
      > <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      >   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      >   <modelVersion>4.0.0</modelVersion>
      > 
      >   <groupId>org.example</groupId>
      >   <artifactId>SSM1</artifactId>
      >   <version>1.0-SNAPSHOT</version>
      >   <packaging>war</packaging>
      > 
      > 
      > 
      >   <!--集中定义版本编号-->
      >   <properties>
      >     <!--单元测试-->
      >     <junit.version>4.13.2</junit.version>
      >     <!--spring相关依赖-->
      >     <spring.version>6.0.9</spring.version>
      >     <!--MyBatis相关依赖-->
      >     <mybatis.version>3.5.13</mybatis.version>
      >     <!--MyBatis与spring整合依赖-->
      >     <mybatis.spring.version>3.0.2</mybatis.spring.version>
      >     <!--MyBatis支持的分页插件的依赖-->
      >     <mybatis.paginator.version>1.2.17</mybatis.paginator.version>
      >     <!--mysql依赖-->
      >     <mysql.version>8.0.33</mysql.version>
      >     <!--日志-->
      >     <slf4j.version>1.6.4</slf4j.version>
      >     <!--德鲁伊-->
      >     <druid.version>1.2.16</druid.version>
      >     <!--分页插件的依赖-->
      >     <pagehelper.version>5.3.2</pagehelper.version>
      >     <!--jstl的依赖(jsp的标准标签库)-->
      >     <jstl.version>1.2</jstl.version>
      >     <!--Servlet的依赖-->
      >     <servlet-api.version>3.0.1</servlet-api.version>
      >     <!--jsp的依赖-->
      >     <jsp-api.version>2.0</jsp-api.version>
      >     <!--jackson的依赖,SpringMVC框架默认进行JSON转换的依赖工具-->
      >     <jackson.version>2.15.1</jackson.version>
      >   </properties>
      > 
      >   <dependencies>
      >     <!-- spring -->
      >     <dependency>
      >       <groupId>org.springframework</groupId>
      >       <artifactId>spring-context</artifactId>
      >       <version>${spring.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>org.springframework</groupId>
      >       <artifactId>spring-beans</artifactId>
      >       <version>${spring.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>org.springframework</groupId>
      >       <artifactId>spring-webmvc</artifactId>
      >       <version>${spring.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>org.springframework</groupId>
      >       <artifactId>spring-jdbc</artifactId>
      >       <version>${spring.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>org.springframework</groupId>
      >       <artifactId>spring-aspects</artifactId>
      >       <version>${spring.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>org.springframework</groupId>
      >       <artifactId>spring-jms</artifactId>
      >       <version>${spring.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>org.springframework</groupId>
      >       <artifactId>spring-context-support</artifactId>
      >       <version>${spring.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>org.springframework</groupId>
      >       <artifactId>spring-test</artifactId>
      >       <version>6.0.6</version>
      >     </dependency>
      >     <!-- Mybatis -->
      >     <dependency>
      >       <groupId>org.mybatis</groupId>
      >       <artifactId>mybatis</artifactId>
      >       <version>${mybatis.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>org.mybatis</groupId>
      >       <artifactId>mybatis-spring</artifactId>
      >       <version>${mybatis.spring.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>com.github.miemiedev</groupId>
      >       <artifactId>mybatis-paginator</artifactId>
      >       <version>${mybatis.paginator.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>com.github.pagehelper</groupId>
      >       <artifactId>pagehelper</artifactId>
      >       <version>${pagehelper.version}</version>
      >     </dependency>
      >     <!-- MySql -->
      >     <dependency>
      >       <groupId>mysql</groupId>
      >       <artifactId>mysql-connector-java</artifactId>
      >       <version>${mysql.version}</version>
      >     </dependency>
      >     <!-- 连接池 -->
      >     <dependency>
      >       <groupId>com.alibaba</groupId>
      >       <artifactId>druid</artifactId>
      >       <version>${druid.version}</version>
      >     </dependency>
      > 
      >     <!-- junit -->
      >     <dependency>
      >       <groupId>org.junit.jupiter</groupId>
      >       <artifactId>junit-jupiter</artifactId>
      >       <version>5.9.0</version>
      >       <scope>test</scope>
      >     </dependency>
      > 
      > 
      >     <!-- JSP相关 -->
      >     <dependency>
      >       <groupId>jstl</groupId>
      >       <artifactId>jstl</artifactId>
      >       <version>${jstl.version}</version>
      >     </dependency>
      >     <!--<dependency>-->
      >     <!--  <groupId>javax.servlet</groupId>-->
      >     <!--  <artifactId>javax.servlet-api</artifactId>-->
      >     <!--  <version>4.0.1</version>-->
      >     <!--  <scope>provided</scope>-->
      >     <!--</dependency>-->
      >     <dependency>
      >       <groupId>jakarta.servlet</groupId>
      >       <artifactId>jakarta.servlet-api</artifactId>
      >       <version>6.0.0</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>javax.servlet</groupId>
      >       <artifactId>jsp-api</artifactId>
      >       <scope>provided</scope>
      >       <version>${jsp-api.version}</version>
      >     </dependency>
      >     <!-- Jackson Json处理工具包 -->
      >     <dependency>
      >       <groupId>com.fasterxml.jackson.core</groupId>
      >       <artifactId>jackson-databind</artifactId>
      >       <version>${jackson.version}</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>org.json</groupId>
      >       <artifactId>json</artifactId>
      >       <version>20230227</version>
      >     </dependency>
      > 
      >     <!--    文件异步上传使用的依赖-->
      >     <dependency>
      >       <groupId>commons-io</groupId>
      >       <artifactId>commons-io</artifactId>
      >       <version>2.11.0</version>
      >     </dependency>
      >     <dependency>
      >       <groupId>commons-fileupload</groupId>
      >       <artifactId>commons-fileupload</artifactId>
      >       <version>1.5</version>
      >     </dependency>
      >   </dependencies>
      > 
      >   <!-- 插件配置 -->
      >   <build>
      >     <plugins>
      >       <plugin>
      >         <groupId>org.apache.maven.plugins</groupId>
      >         <artifactId>maven-compiler-plugin</artifactId>
      >         <configuration>
      >           <source>17</source>
      >           <target>17</target>
      >           <encoding>UTF-8</encoding>
      >         </configuration>
      >       </plugin>
      >     </plugins>
      >     <!--识别所有的配置文件-->
      >     <resources>
      >       <resource>
      >         <directory>src/main/java</directory>
      >         <includes>
      >           <include>**/*.properties</include>
      >           <include>**/*.xml</include>
      >         </includes>
      >         <filtering>false</filtering>
      >       </resource>
      >       <resource>
      >         <directory>src/main/resources</directory>
      >         <includes>
      >           <include>**/*.properties</include>
      >           <include>**/*.xml</include>
      >         </includes>
      >         <filtering>false</filtering>
      >       </resource>
      >     </resources>
      >   </build>
      > 
      > </project>
      > ```
   
   2. 创建相关的包
   
      > <img src="C:\Users\fengshun\Desktop\自学\SpringMVC\图片\image-20230912232229662.png" alt="image-20230912232229662" style="zoom:67%;" />
   
   3. 创建 jdbc.properties 配置文件
   
      > ```properties
      > jdbc.driver=com.mysql.cj.jdbc.Driver
      > jdbc.url=jdbc:mysql://localhost:3306/mybatis
      > jdbc.username=root
      > jdbc.password=051727
      > ```
   
   4. 编写 springmvc 配置文件
   
      > ```xml
      > <?xml version="1.0" encoding="UTF-8"?>
      > <beans xmlns="http://www.springframework.org/schema/beans"
      >        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      >        xmlns:context="http://www.springframework.org/schema/context"
      >        xmlns:mvc="http://www.springframework.org/schema/mvc"
      >        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
      >     <!--添加组件扫描器-->
      >     <context:component-scan base-package="com.fengshun.controller"/>
      > 
      >     <!--添加注解驱动-->
      >     <!--
      >     1.处理ajax请求，返回json
      >     2.处理静态资源
      >     -->
      >     <mvc:annotation-driven></mvc:annotation-driven>
      > 
      > </beans>
      > ```
   
   5. 编写 mybatis 配置文件
   
      > ```xml
      > <?xml version="1.0" encoding="UTF-8" ?>
      > <!DOCTYPE configuration
      >         PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
      >         "https://mybatis.org/dtd/mybatis-3-config.dtd">
      > <configuration>
      >     <properties resource=""></properties>
      >     <settings>
      >         <!--开启小驼峰命名-->
      >         <setting name="mapUnderscoreToCamelCase" value="true"/>
      >         <setting name="logImpl" value="STDOUT_LOGGING"/>
      >     </settings>
      > </configuration>
      > ```
   
   6. 编写 spring 配置文件
   
      > ```xml
      > <?xml version="1.0" encoding="UTF-8"?>
      > <beans xmlns="http://www.springframework.org/schema/beans"
      >        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      >        xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
      >        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
      > 
      >     <!--组件扫描-->
      >     <context:component-scan base-package="com.fengshun.service"/>
      > 
      > 
      >     <!--引入外部的属性文件-->
      >     <context:property-placeholder location="classpath:jdbc.properties"/>
      > 
      >     <!--配置数据源-->
      >     <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
      >         <property name="driverClassName" value="${jdbc.driver}"/>
      >         <property name="url" value="${jdbc.url}"/>
      >         <property name="username" value="${jdbc.username}"/>
      >         <property name="password" value="${jdbc.password}"/>
      >     </bean>
      > 
      >     <!--SqlSeesionFactoryBean-->
      >     <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
      >         <!--配置数据源-->
      >         <property name="dataSource" ref="dataSource"/>
      >         <!--关联MyBatis核心配置文件-->
      >         <property name="configLocation" value="classpath:mybatis-config.xml"/>
      >         <!--指定别名包，类似于在MyBatis配置文件中使用 typeAliases 标签-->
      >         <property name="typeAliasesPackage" value="com.fengshun.pojo"/>
      >     </bean>
      > 
      >     <!--Mapper扫描配置器,主要扫描mapper接口，生成代理类  -->
      >     <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
      >         <!--指定扫描的包，这样mapper中的接口都会被创建出来-->
      >         <property name="basePackage" value="com.fengshun.mapper"/>
      > 
      >         <!--<property name="sqlSessionFactoryBeanName" value="sqlSessionFactoryBean"/>-->
      >     </bean>
      > 
      >     <!--事务管理器DataSourceTransactionManager-->
      >     <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      >         <!--注入数据源-->
      >         <property name="dataSource" ref="dataSource"/>
      >     </bean>
      > 
      >     <!--启用事务注解-->
      >     <tx:annotation-driven transaction-manager="txManager"/>
      > </beans>
      > ```
   
   7. 编写 web.xml 文件
   
      > ```xml
      > <?xml version="1.0" encoding="UTF-8"?>
      > <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
      >          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      >          xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
      >          version="5.0">
      >     <!--添加中文编码过滤器-->
      >     <filter>
      >         <filter-name>characterEncodingFilter</filter-name>
      >         <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
      >         <init-param>
      >             <param-name>encoding</param-name>
      >             <param-value>utf-8</param-value>
      >         </init-param>
      >         <init-param>
      >             <param-name>forceRequestEncoding</param-name>
      >             <param-value>true</param-value>
      >         </init-param>
      >         <init-param>
      >             <param-name>forceResponseEncoding</param-name>
      >             <param-value>true</param-value>
      >         </init-param>
      >     </filter>
      >     <filter-mapping>
      >         <filter-name>characterEncodingFilter</filter-name>
      >         <url-pattern>/*</url-pattern>
      >     </filter-mapping>
      > 
      >     <!--注册SpringMVC框架，中央调度器-->
      >     <servlet>
      >         <servlet-name>springmvc</servlet-name>
      >         <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      >         <init-param>
      >             <param-name>contextConfigLocation</param-name>
      >             <param-value>classpath:springmvc.xml</param-value>
      >         </init-param>
      >         <load-on-startup>1</load-on-startup>
      >     </servlet>
      >     <servlet-mapping>
      >         <servlet-name>springmvc</servlet-name>
      >         <url-pattern>/</url-pattern>
      >     </servlet-mapping>
      > 
      >     <!--注册监听器，注册spring框架,目的是启动spring容器-->
      >     <listener>
      >         <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
      >     </listener>
      >     <context-param>
      >         <param-name>contextConfigLocation</param-name>
      >         <param-value>classpath:spring.xml</param-value>
      >     </context-param>
      > </web-app>
      > ```
   
   8. 创建实体类
   
      > ```java
      > package com.fengshun.pojo;
      > 
      > public class User {
      >     private String userId;
      >     private String cardType;
      >     private String cardNo;
      >     private String userName;
      >     private String userSex;
      >     private String userAge;
      >     private String userRole;
      > 
      >     public User(String userId, String cardType,
      >                 String cardNo, String userName,
      >                 String userSex, String userAge, String userRole) {
      >         this.userId = userId;
      >         this.cardType = cardType;
      >         this.cardNo = cardNo;
      >         this.userName = userName;
      >         this.userSex = userSex;
      >         this.userAge = userAge;
      >         this.userRole = userRole;
      >     }
      > 
      >     public User() {
      >     }
      > 
      >   // getter and setter
      >     //toString
      > }
      > ```
   
   9. 创建 mapper 接口
   
      > ```java
      > package com.fengshun.mapper;
      > 
      > import com.fengshun.pojo.User;
      > import org.apache.ibatis.annotations.Mapper;
      > import org.apache.ibatis.annotations.Param;
      > 
      > import java.util.List;
      > 
      > 
      > public interface UserMapper {
      >     // 根据条件分页获取分页数据
      >     List<User> selectUserPage(
      >             @Param("userName")
      >             String userName,
      >             @Param("userSex")
      >             String userSex,
      >             @Param("startRow")
      >             int startRow);
      >     // 获取总行数
      >     int getRowCount(@Param("userName") String userName, @Param("userSex") String userSex);
      >     // 根据用户ID删除用户
      >     int deleteUserById(String userId);
      >     // 增加用户
      >     int createUser(User user);
      >     // 更新用户
      >     int updateUserById(User user);
      > }
      > ```
   
   10. 编写 sqlmapper 文件
   
       > ```xml
       > <?xml version="1.0" encoding="UTF-8" ?>
       > <!DOCTYPE mapper
       >         PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
       >         "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
       > <mapper namespace="com.fengshun.mapper.UserMapper">
       >     <sql id="allColumn">
       >         user_id,card_type,card_no,user_name,user_sex,user_age,user_role
       >     </sql>
       >     <!--    // 根据条件分页获取分页数据
       >         List<User> selectUserPage(
       >         @Param("userName")
       >         String userName,
       >         @Param("userSex")
       >         String userSex,
       >         @Param("startRow")
       >         String startRow);-->
       >     <select id="selectUserPage" resultType="user">
       >         select
       >         <include refid="allColumn"></include>
       >         from user
       >         <where>
       >             <if test="userName != null and userName != ''">
       >                 and user_name like concat('%',#{userName},'%')
       >             </if>
       >             <if test="userSex != null and userSex != '' ">
       >                 and user_sex = #{userSex}
       >             </if>
       >         </where>
       >         limit #{startRow} , 5
       >     </select>
       >     <!--    // 获取总行数
       >         int getRowCount(@Param("userName") String userName, @Param("userSex") String userSex);-->
       >     <select id="getRowCount">
       >         select count(*) from user
       >         <where>
       >             <if test="userName != null and userName != ''">
       >                 and user_name like concat('%',#{userName},'%')
       >             </if>
       >             <if test="userSex != null and userSex != '' ">
       >                 and user_sex = #{userSex}
       >             </if>
       >         </where>
       >     </select>
       >     <!--// 根据用户ID删除用户
       >     int deleteUserById(String userId);-->
       >     <delete id="deleteUserById">
       >         delete from user where user_id=#{userId}
       >     </delete>
       > 
       >     <!--// 增加用户
       >     int createUser(User user);
       > 
       >         private String userId;
       >     private String cardType;
       >     private String cardNo;
       >     private String userName;
       >     private String userSex;
       >     private String userAge;
       >     private String userRole;-->
       >     <insert id="createUser" parameterType="user">
       >         insert into user values(#{userId},#{cardType},#{cardNo},#{userName},#{userSex},#{userAge},#{userRole})
       >     </insert>
       > 
       >     <!--  // 更新用户
       >       int updateUserById(User user);-->
       >     <update id="updateUserById" parameterType="user">
       >         update user set card_type=#{cardType},card_no=#{cardNo},user_name=#{userName},user_sex=#{userSex},user_age=#{userAge},user_role=#{userRole}
       >         where user_id=#{userId}
       >     </update>
       > </mapper>
       > ```
   
   11. 开发 Service 层接口以及实现类
   
       > service 接口：
       >
       > ```java
       > package com.fengshun.service;
       > 
       > import com.fengshun.pojo.User;
       > 
       > import java.util.List;
       > 
       > public interface UserService {
       >     // 根据条件分页获取分页数据
       >     List<User> selectUserPage(
       >             String userName,
       >             String userSex,
       >             int startRow);
       >     // 获取总行数
       >     int getRowCount(String userName,String userSex);
       >     // 根据用户ID删除用户
       >     int deleteUserById(String userId);
       >     // 增加用户
       >     int createUser(User user);
       >     // 更新用户
       >     int updateUserById(User user);
       > }
       > ```
       >
       > 实现类：
       >
       > ```java
       > package com.fengshun.service.impl;
       > 
       > import com.fengshun.mapper.UserMapper;
       > import com.fengshun.pojo.User;
       > import com.fengshun.service.UserService;
       > import org.springframework.beans.factory.annotation.Autowired;
       > import org.springframework.stereotype.Service;
       > import org.springframework.transaction.annotation.Transactional;
       > 
       > import java.util.List;
       > 
       > @Service(value = "userServiceImpl")
       > @Transactional
       > public class UserServiceImpl implements UserService {
       > 
       >     @Autowired()
       >     private UserMapper userMapper;
       > 
       >     @Override
       >     public List<User> selectUserPage(String userName, String userSex, int startRow) {
       >         return userMapper.selectUserPage(userName,userSex,startRow);
       >     }
       > 
       >     @Override
       >     public int getRowCount(String userName, String userSex) {
       >         return userMapper.getRowCount(userName,userSex);
       >     }
       > 
       >     @Override
       >     public int deleteUserById(String userId) {
       >         return userMapper.deleteUserById(userId);
       >     }
       > 
       >     @Override
       >     public int createUser(User user) {
       >         return userMapper.createUser(user);
       >     }
       > 
       >     @Override
       >     public int updateUserById(User user) {
       >         return userMapper.updateUserById(user);
       >     }
       > }
       > ```
   
   12. 开发 Controller 层
   
       > ```java
       > package com.fengshun.controller;
       > 
       > import com.fengshun.pojo.User;
       > import com.fengshun.service.UserService;
       > import org.springframework.beans.factory.annotation.Autowired;
       > import org.springframework.beans.factory.annotation.Qualifier;
       > import org.springframework.stereotype.Controller;
       > import org.springframework.web.bind.annotation.RequestMapping;
       > import org.springframework.web.bind.annotation.ResponseBody;
       > 
       > import java.util.List;
       > import java.util.UUID;
       > 
       > @Controller("userController")
       > @RequestMapping(value = "/user")
       > public class UserController {
       >     private static final int PAGE_SIZE = 5;
       >     @Autowired
       >     @Qualifier("userServiceImpl")
       >     private UserService userService;
       > 
       >     // 分页查询全部
       >     @RequestMapping("/selectUserPage")
       >     @ResponseBody
       >     public List<User> selectUserPage(String userName, String userSex, Integer page) {
       >         // 计算起始页码
       >         int startRow = 0;
       >         if (page != null) {
       >             startRow = (page - 1) * PAGE_SIZE;
       >         }
       > 
       >         return userService.selectUserPage(userName, userSex, startRow);
       >     }
       > 
       >     // 获取总行数
       >     @RequestMapping("/getRowCount")
       >     @ResponseBody
       >     public int getRowCount(String userName, String userSex) {
       > 
       >         return userService.getRowCount(userName, userSex);
       >     }
       >     // 根据用户ID删除用户
       >     @RequestMapping("/deleteUserById")
       >     @ResponseBody
       >     public int deleteUserById(String userId){
       >         return userService.deleteUserById(userId);
       >     }
       >     // 增加用户
       >     @RequestMapping("/createUser")
       >     @ResponseBody
       >     public int createUser(User user){
       >         user.setUserId(UUID.randomUUID().toString());
       >         return userService.createUser(user);
       >     }
       >     // 更新用户
       >     @RequestMapping("/updateUserById")
       >     @ResponseBody
       >     public int updateUserById(User user){
       >         return userService.updateUserById(user);
       >     }
       > }
       > ```
   

后端开发中的拓展：

> 1. @ResponseBody 注解是用来响应ajax请求的，框架会将返回的值转换为 json，在每个方法中都使用该注解，不能让代码得到有效的简洁，可以使用 **@RestController 注解**，这个注解作用在类上，表示这个类上的所有方法都是 ajax 请求
> 2. 前后端分离项目中，由于vue框架和tomcat服务的的端口号冲突，这意味着ajax必须是跨域请求，针对ajax跨域请求，需要在Controller层的类上添加 **@CrossOrigin 注解**，这个注解表示在服务器端支持跨域访问。

## 4.2 SSM整合前端功能

使用Element UI 框架创建前端页面。

Element UI官网地址:

https://element.eleme.cn/#/zh-CN/component/installation

Element UI是Vue使用的前端的框架,通过官网可以自行学习.
