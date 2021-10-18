---
title: 从0开始的springboot-quickstart.md
date: 2021-10-18 22:02:40
tags: [springboot]
---
## springboot01-入门应用

> 快速构建应用

### 1. pom.xml

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.5</version>
</parent>
```

starter-parent默认配置了超多的模块,截止目前,最新版本为2.5.5;

### 2. 应用入口

```java
@SpringBootApplication
public class App {
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}
```

### 3. 核心注解

#### 3.1 @ResponseBody

将*@controller*的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据；使用此注解就不会走视图处理器，而是将数据写入到输入流中；

- 作用在方法上，表示该方法的返回值直接写入HTTP response body中，在使用 *@RequestMapping*后，返回值通常解析为跳转路径，但是加上 @ResponseBody 后返回结果不会被解析为跳转路径，而是直接写入 HTTP response body 中；
- 作用在形参列表上，用于将前台发送过来固定格式的数据【xml格式 或者 json等】封装为对应的javabean对象，封装时使用到的一个对象是系统默认配置的HttpMessageConverter进行解析，然后封装到形参上。

一般来说，对应*get，post，put*请求，根据request header Content-Type的值来判断:    

- application/x-www-form-urlencoded， 可选（即非必须，因为这种情况的数据@RequestParam, @ModelAttribute也可以处理，当然@RequestBody也能处理）； 
- multipart/form-data, 不能处理（即使用@RequestBody不能处理这种格式的数据）； 
- 其他格式， 必须（其他格式包括application/json, application/xml等。这些格式的数据，必须使用@RequestBody来处理）；

<!--more-->
#### 3.2 @Controller

控制器Controller 负责处理由DispatcherServlet 分发的请求，它把用户请求的数据经过业务处理层处理之后封装成一个Model ，然后再把该Model 返回给对应的View 进行展示。

#### 3.3 @RestController

@RestController=@ResponseBody+@Controller

### 4. 简化部署

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

maven->lifecycle->clean+package = jar文件
