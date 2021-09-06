---
title: maven基础知识一览
date: 2021-09-06 09:45:53
tags: [多线程,E9]
---

> maven作为java项目构建的常用工具,必须掌握

### 一.从开发模式和软件构建方式说起

#### 1.1 瀑布式还是敏捷式

项目实践中,开发模式中较为常见的有 **瀑布式开发模式** 和 **敏捷开发模式**, 简单介绍如下:

- 瀑布式

有*严格的开发阶段和完备的开发文档*, 追求稳定和阶段化, 适合于需求相对稳定, 周期较长的大型的项目;

- 敏捷开发

*快速迭代,拥抱变化*. 首先把客户最关注的软件原型先做出来，交付或者上线，在实际场景中去修改弥补需求中的不足，快速修改，再次发布版本;

#### 1.2 项目构建工具

从早期的ant构建到maven再到gradle, 大致的进程是去复杂化, 简洁化. ant成为历史, 原因在于xml脚本的编写过于复杂, 但对构建过程中的控制十分细化; maven: 项目对象模型,填补了ant的缺点, 同时支持从网络获取jar文件,另外maven更专注于依赖管理; gradle结合了以上两个的优点, dsl格式使得脚本的编写更加简洁.
<!--more-->
### 二.maven的四大特性

#### 2.1 依赖管理

在maven的理念中,唯一标识一个依赖,只需要 *由groupid, artifactid, version组成的坐标*;

- groupid

定义当前maven依赖隶属于的实际项目-公司名称

- artifactid

定义实际项目中的一个maven模块-项目名称

- version

依赖所处版本

#### 2.2 多模块构建

项目通过多个maven工程的模块构建, 并通过 parent pom 聚合这些模块的构建方式, 在parent中,使用`<modules>`来定义子模块

#### 2.3 一致的项目结构

简单来说,使用maven可以在多种ide中使用

#### 2.4 一致的构建模型和插件机制

同上, maven插件机制也是适用ide的

### 三.maven的安装配置

ps: 另外的博客去实践, 先附上b站的参考链接:

`https://www.bilibili.com/video/BV1Fz4y167p5?p=3&spm_id_from=pageDriver`

日后填坑!

#### 四.maven命令

#### 4.1 通用命令参数

命令行操作mvn指令是搬砖必备的技能, maven的指令格式`mvn [plugin-name]:[goal-name]`; 常用指令 `mvn -version`: 展示版本信息; `mvn clean`: 清理项目生产的临时文件,一般是target目录; `mvn compile`: 编译源代码,一般编译模块下的src/main/java目录; `mvn package`: 项目打包工具,会在模块下的target目录生产jar或war文件; `mvn test`:测试命令,或执行src/test/java下的junit的测试用例; `mvn install`: 将打包的jar/war文件复制到你的本地仓库,供其他模块使用; `mvn deploy`: 将打包的文件发布到远程参考,提供其他人员下载依赖;`mvn eclipse:eclipse`:将项目转化成eclipse项目; `mvn dependency:tree`:打印出整个依赖树; `mvn archetype:generate`:创建maven的普通java项目; `mvn tomcat7:run`:在tomcat中运行web应用; `mvn jetty:run`:调用jetty插件的在jetty servlet容器中启动web

#### 4.2 命令参数(详细)

##### 4.2.1 -D 传入属性参数

`mvn package -Dmaven.test.skip=true`, 以-D开头,将maven.test.skip的值设置为true,表示maven打包是跳过单元测试;

##### 4.2.2 -P 使用指定的PROFILE配置

一般在maven项目的pom文件中,会指定`profiles`的多个profile,用于开发,测试,预发和正式;具体的找个例子去实践. 贴出案例指令: `mvn package -Pdev -Dmaven.test.skip=true`, 打包profile为dev, 并跳过测试的项目;

### 五.maven仓库的基本概念

对应maven而言,仓库只分为:**本地仓库和远程仓库**, **本地仓库**不用说, 安装maven的时候就指定了, 它也是项目构建最主要的仓库对象, **远程仓库**又分为 *中央仓库,私服, 其他公共库*, 中央仓库是一个超级pom,一般不去使用; 私服是一个特殊的远程仓库,它一般是架设在局域网中的, 公司中比较常见, 私服的pom.xml配置参考: `https://cloud.tencent.com/developer/article/1492820`; **其他公共库**, 如aliyun maven,比较常用

### 六.maven打包

`mvn clean compile package -Ptest -Dmaven.test.skip=true`,更详细的介绍日后补充

