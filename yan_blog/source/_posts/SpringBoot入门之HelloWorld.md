---
title: SpringBoot入门之HelloWorld
date: 2018-04-11 11:26:55
tags:
 - SpringBoot
categories: 
 - 分布式
copyright: true
---


#### **SpringBoot是什么？**

Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。

#### **SpringBoot优点？**

<!--more-->

 1. 快速构建项目 对主流开发框架的无配置集成 
 2. 项目可独立运行，无须外部依赖Servlet容器（Spring Boot默认自带了一个Tomcat）
 3. 提供运行时的应用监控 
 4. 极大地提高了开发、部署效率 
 5. 提供一系列大型企业级项目的功能性特性

#### **那就开始我们的HelloWorld**

只需三步就完成我们的HelloWorld

第一步，我们新建一个Maven项目。给项目添加架包依赖，修改pom.xml

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.javazhan</groupId>
  <artifactId>sb01</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.2.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
  </parent>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <java.version>1.8</java.version>
  </properties>

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

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
  </dependencies>
</project>

```
第二步，新建一个类HelloWorldController .java

```
package com.javazhan.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloWorldController {
    
    @RequestMapping("/helloworld")
    public String helloworld() {
        return "HelloWorld!" ;
    }
}

```

新建一个启动类RunApplication.java

```
package com.javazhan;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class RunApplication{
    public static void main( String[] args ){
        SpringApplication.run(RunApplication.class, args) ;
    }
}
```

第三步，运行RunApplication.java，在浏览器输入地址http://localhost:8080/helloworld查看结果

![这里写图片描述](http://img.blog.csdn.net/20170831155736340?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### **知识点**

一般Spring Boot的Web应用都有一个xxxApplication类，并使用@SpringBootApplication注解标记，作为该web应用的加载入口。此外，还需要在main方法中(可以是任意一个类)使用SpringApplication.run(xxxApplication.class, args)来启动该web应用


#### **源码下载**

[SpringBoot入门之HelloWorld源码](https://github.com/yandongquan/SpringBootInstance/tree/master/SpringBootHelloWorld)
