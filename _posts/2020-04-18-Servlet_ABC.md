---
layout: post
title: Servlet配置和尝试使用
date: 2020-04-18 
tag: JavaWeb
---

## Servlet

### 概述

> server applet: 运行在服务器端的小程序

![image-20200417080316701](/images/posts/Servlet.assets/image-20200417080316701.png)

1. Servlet就是一个接口,定义了Java类被浏览器访问(Tomcat识别)到的规则
2. 将来自定义的类需要实现Servlet接口,复写方法

### 快速入门

1. 创建JavaEE项目

2. 定义一个类,实现Servlet接口

3. 实现接口中的抽象方法

4. 配置Servlet

   ```xml
   <!-- 配置servlet -->
   <servlet>
       <!-- 配置servlet名称 -->
       <servlet-name>demo1</servlet-name>
       <!-- 实现servlet类 -->
       <servlet-class>cn.itcast.web.servlet.ServletDemo1</servlet-class>
   </servlet>
   
   <servlet-mapping>
       <!-- 名称 -->
       <servlet-name>demo1</servlet-name>
       <!-- /名称  表示路径-->
       <url-pattern>/demo1</url-pattern>
   </servlet-mapping>
   ```

   ---

### 执行原理

   ![image-20200418094723229](/images/posts/Servlet.assets/image-20200418094723229.png)

1. 当服务器接收到哭护短浏览器的请求后,会解析URL路径,获取访问的Servler的资源路径
2. 查找web,xml文件,是否有对应的<url-pattern>标签内容
3. 如果有,再查询,<servlet-class>的全类名
4. tomcat会将字节码文件加载进内存,并创建其对象
5. 调用方法

### Servlet的生命周期

1. 被创建: 执行init方法,只执行一次

   - 可以修改web.xml,指定什么时候为创建时机

     ```java
     在servlet标签下配置
     1. 第一次被访问时,创建(<load-on-startup>的值设为负数)
     2. 服务器启动时,创建(<load-on-startup>的值设为0或者正整数)
     ```

   - init方法只执行一次,Servlet是单例的
     1. 多个用户访问时,可能存在安全问题
     2. 解决方法,尽量不要在Servlet定义成员变量.即使定义,也不要修改

2. 提供服务: 执行service方法,执行多次

   - 每次访问ervlet时,都会被调用

3. 被销毁: 执行destroy方法,只执行一次

   - 只有当服务器正常关闭时,才能执行destroy方法

   

   #### 生命周期的是三个方法

   ```java
   /**
   * 初始化方法 在Servlet被创建时执行,且只执行一次,最先执行...
   */
   @Override
   public void init(ServletConfig servletConfig) throws ServletException {
       System.out.println("init方法...");
   }
   /**
   * 获取ServletConfig对象   ServletConfig: Servlet的配置对象
   */
   @Override
   public ServletConfig getServletConfig() {
       return null;
   }
   /**
   * 提供服务方法
   * 每一次Servlet被访问时执行,执行多次
   */
   @Override
   public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
       System.out.println("service方法...");
   }
   /**
   * 获取Servlet的一些信息,版本,作者等...
   */
   @Override
   public String getServletInfo() {
       return null;
   }
   ```

   ### servlet3.0

   - 优势: 支持注解配置,可以不需要配置web.xml

   - 步骤

     1. 创建JavaEE项目,选择Servlet的版本3.0以上,可以不创建web.xml

     2. 定义一个类,实现Servlet接口

     3. 在类上使用@WebServlet注解,进行配置

        ```java
        // @WebServlet(urlPatterns = "/demo2")
        // @WebServlet(value = "/demo2")
        @WebServlet("/demo2(资源路径)") // 区别于与项目名命名的虚拟目录
        ```

        





