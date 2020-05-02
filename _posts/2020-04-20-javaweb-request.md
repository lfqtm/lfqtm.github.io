---
layout: post
title: 请求与响应简单了解
date: 2020-04-20 
tag: JavaWeb
---

## 请求与响应(Request&Response)

![image-20200418200649988](/images/posts/Request.assets/image-20200418200649988.png)

**request对象和response对象的原理**

1. request和response对象是由服务器创建的,我们只能调用
2. request对象是来获取请求消息,response对象是来设置响应消息

### 请求Request

#### 1. request对象继承体系结构

SerletRequst  --接口
    | 继承
HttpServletRequest --接口
	| 实现
org.apache.catalina.connector.RequestFacade --类(tomcat源码实现)

#### 2. request功能

##### 1. 获取请求消息数据

> 数据格式: 请求行,请求头,请求空行,请求体

1. 获取请求行数据

   `GET /day14/demo1?name=zhangsan HTTP/1.1`

   方法:  可以用来区分用户的浏览器做浏览器适配

   ```java
   // 1.获取请求方式: GET
   String getMethod();
   // 2.获取虚拟目录: /day14  
   String getContextPath(); // 常用
   // 3.获取Servlet路径: /demo1
   String getServletPath();
   // 4.获取get方式请求参数: name=zhangsan
   String getQueryString();
   // 5.获取请求的URI以及URL: /day14/demo1  http://localhost/day14/demo1
   /**
   URL: 同统一资源定位符  中华人民共和国
   URI: 统一资源标识符  共和国
   */
   String getRequestURI(); // 常用
   StringBuffer getRequestURL();  
   // 6.获取协议及版本: HTTP/1.1
   String getProtocol();
   // 7.获取客户机的IP地址
   String getRemoteAddr();
   ```

   

2. 获取请求头数据

   - `String getHeader(String name); ` 通过请求头名称获取请求头的值 (**主要使用这个方法**)
   - `Enumeration<String> getHeaderNames();`  获取所有的请求头名称

3. 获取请求体数据

   - 请求方式必须是POST,才有请求体数据

   - 操作步骤

     1. 获取对象流

        `BufferedReader getReader()` : 获取字符输入流,只可以操作字符数据

        `ServletInputStream getInputStream()`: 获取直接输入流,可以操作所有类型数据

     2. 再从流中拿数据

   

##### 2. 其他功能(重要,为了更方便使用request对象的方法) ​ ​ ​ ​ :alarm_clock:

1. **获取请求参数(通用,get和post都可用)**
   
   1. `String getParameter(String name)`: 根据参数名称获取参数值
   2. `String[] getParameterValues(String name)`: 根据参数名称获取 参数名称和参数值的数组;多用于复选框(`hobby=xx&hobby=xx2`)
   3. `Enumeration<String> getParameterNames()`: 获取所有请求的参数名称
   4. `Map<String,String[]> getParameterMap()` : 获取所有参数的map集合
   
   **补充: 中文乱码问题**
   
   - get方式: tomcat8已结将get方式乱码问题解决了
   - post方式: 会乱码 (解决方案: 在获取参数前,设置request的编码`request.setCharacterEncoding("utf-8")`)
   
   
   
2. **请求转发**

   1. 步骤: 
      - 通过request对象获取请求转发器对象: `request.getRequestDispatcher(String path)`
      - 使用RequestDispatcher对象来进行转发: `forward(ServletRequest request, ServletResponse response)`
   2. 特点
      - 浏览器地址栏路径不发生变化
      - 只能转发到当前服务器内部资源(无法跳转到其他服务器url)
      - 转发始终只有一次请求

3. **共享数据(请求转发需要传递数据)**

   - 域对象: 一个有作用范围的对象,可以在范围内共享数据
   - request域: 代表一次请求的范围,一般用于请求转发的多个资源中共享数据
     1. `void setAttribute(String name,Object obj)`:存储数据
     2. `Object getAttribute(String name)`: 通过键获取值
     3. `void removeAttribute(String name)`: 通过键移除键值对

4. **获取ServletContext**

   方法: `getServletContext()`

### 响应response

#### 1. 响应数据格式

- 响应行
  1. 格式: `协议/版本 响应状态码 状态码描述`
  2. 什么是状态码: 服务器告诉客户端本次请求和响应的一个状态(*都是三位数字*)
     - 1xx : 服务器接收客户顿消息,但并没有接收完成,等待一段时间后,发送1xx
     - 2xx : 请求和响应成功
     - 3xx : 重定向 (`302` - 重定向页面; `304` - 访问缓存)
     - 4xx : 客户端错误  (`404` - 请求路径错误; `405` - 请求方式没有对应的doXXX方法)
     - 5xx : 服务器端异常 `500` - 服务端代码错误

- 响应头

  1. 格式 : 头名称 : 值
  2. 常见响应头
     - `Content-Type` : 本次响应的数据格式和编码格式
     - `Content-disposition` : 服务器告诉客户端以什么格式打开响应体数据 

- 响应空行

  间隔响应头和响应体

- 响应体

  响应数据,图片和网页等

#### 2. response对象

1. 设置响应行
   - `setStatus(int sc)`
2. 设置响应头
   - `setHeader(String name, String value)`
3. 设置响应体(**步骤**)
   1. 获取输入流
      - 字符输出流: `PrintWriter getWriter()`
      - 直接输出流 : `ServletOutputStream getOutPutStream()`
   2. 使用输出流,输出到客户端浏览器

#### 3. response案例

1. 重定向 (资源跳转)

   ```java
   // 1.设置状态码
   response.setStatus(302); // 重定向状态码
   // 2.设置响应头
   response.setHeader("location", "/day14/responseDemo2");
   
   // 3.简单的重定向方式
   response.sendRedirect("/day15/responseDemo2");
   ```

   *与转发的比较* 

   - 转发(forward)
     1. 转发地址栏路径不变
     2. 转发只能访问当前服务器下的资源
     3. 转发只是一次请求,可以使用request对象来共享数据
   - 重定向(redirect)
     1. 地址栏发生变化
     2. 重定向可以访问其他站点资源
     3. 重定向是两次求情,不能使用request对象来共享数据

   *路径写法*

   - 路径分类

     1. 相对路径: 通过相对位置关系确定位置(以点开头 ./ 或者 ../)
     2. 绝对路径: `/day14/responseDemo2`或者url地址

   - 判断规则 : 判断定义的路径是给谁用的?判断请求从哪来

     1. 给客户端浏览器使用 : 需要添加虚拟路径(典型的重定向)

     2. 给服务器使用 :不需要加虚拟路径(典型的转发)

     3. **动态获取虚拟目录**

        ```java
        // 动态获取虚拟目录
        String contextPath = response.getContextPath();
        // 重定向
        response.sendRedirect(contextPath + "/responseDemo2");
        ```

        

     

2. 服务器输出字符数据到浏览器

3. 服务器输出字节数据到浏览器

   1. 获取字节输出流

      ```java
      // 0.设置编码
      response.setContentType("text/html;charset=utf-8");
      // 1.获取输出流(字节)
      ServletOutputStream sos = response.getOutputStream();
      // 2.输出数据
      sos.write("你好".getBytes("utf-8"));
      ```

   2. 输出数据 `write()` : 不需要刷新,输出后自动释放

4. 服务器输出字符数据到浏览器

   1. 获取字符输出流

      ```java
      // 最推荐的解决中文乱码问题
      response.setContentType("text/html;charset=utf-8");
      // 1.获取字符输出流
      PrintWriter pw = response.getWriter();
      // 2.输出数据
      pw.write("hello response"); // 写入response,可以解析html标签
      ```

   2. 输出数据 `write()` : 不需要刷新,输出后自动释放

5. 验证码验证案例

   ```java
   int width = 100;
   int heigth = 50;
   // 1.创建一个对象,图片
   BufferedImage image = new BufferedImage(width,heigth,BufferedImage.TYPE_INT_RGB);
   
   // 2.美化图片
   // 2.1 填充背景色
   Graphics g = image.getGraphics(); // 画笔对象
   g.setColor(Color.pink);
   g.fillRect(0,0, width, heigth);
   
   // 2.2 边框
   g.setColor(Color.blue);
   g.drawRect(0, 0, -1, -1);
   
   // 2.3 写验证码
   String string = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
   // 创建随机角标
   Random ran = new Random();
   for (int i = 0; i < 4; i++) {
       int index = ran.nextInt(string.length());
       // 获取随机角标对应的随机字符
       char ch = string.charAt(index);
       // 2.3写验证码
       g.drawString(ch + "", width / 5 * i, heigth / 2);
   }
   
   // 2.3 画干扰线
   g.setColor(Color.green);
   // 随机生成坐标点
   for (int i = 0; i < 7; i++) {
       int x1 = ran.nextInt(width);
       int x2 = ran.nextInt(width);
       int y1 = ran.nextInt(heigth);
       int y2 = ran.nextInt(heigth);
       g.drawLine(x1, y1, x2, y2);
   }
   g.drawLine(1, 1, 30, 30);
   // 3.将图片输出到页面中
   ImageIO.write(image, "jpg", response.getOutputStream());
   }
   ```

   ```html
   <script>
       window.onload = function () {
           // 1.获取图片对象
           var img = document.getElementById("checkCode");
           // 2.绑定单击事件
           img.onclick = function () {
               // 加时间戳,欺骗服务器
               var date = new Date().getTime();
               img.src = "/day14/checkCodeServlet" + date;
           }
       }
   
   </script>
   </head>
   <body>
       <img id="checkCode" src="/day14/checkCodeServlet"/>
       <a id="change" href="看不清换一张?"></a>
   </body>
   ```

   

