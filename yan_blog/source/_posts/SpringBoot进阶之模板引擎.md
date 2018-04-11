---
title: SpringBoot进阶之模板引擎
date: 2018-04-11 11:35:09
tags:
 - SpringBoot
 - Thymeleaf
categories: 
 - 分布式
copyright: true
---

在动态HTML实现上Spring Boot依然可以完美胜任，并且提供了多种模板引擎的默认配置支持，所以在推荐的模板引擎下，我们可以很快的上手开发动态网站。

#### 模板引擎种类


Spring Boot提供了默认配置的模板引擎主要有以下几种：

<!-- more -->


 - Thymeleaf 
 - FreeMarker 
 - Velocity 
 - Groovy 
 - Mustache

当你使用上述模板引擎中的任何一个，它们默认的模板配置路径为：src/main/resources/templates。

#### Thymeleaf

Thymeleaf是一个XML/XHTML/HTML5模板引擎，可用于Web与非Web环境中的应用开发。Thymeleaf提供了一个用于整合Spring MVC的可选模块，在应用开发中，你可以使用Thymeleaf来完全代替JSP或其他模板引擎，如Velocity、FreeMarker等。

在Spring Boot中使用Thymeleaf，只需要引入下面依赖：
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
>**案例-遍历所有学生信息**

效果图：

![遍历所有学生信息](http://img.blog.csdn.net/20170904113034792?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**工程结构**

root package结构：com.example.myproject 应用主类Application.java置于rootpackage下，通常我们会在应用主类中做一些框架配置扫描等配置，我们放在root package下可以帮助程序减少手工配置来加载到我们希望被Spring加载的内容

 - 实体（Entity）与数据访问层（Repository）置于com.example.myproject.domain包下
 - 逻辑层（Service）置于com.example.myproject.service包下
 - Web层（web）置于com.example.myproject.web包下

![工程结构](http://img.blog.csdn.net/20170904113401513?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

实体类 Student .java
```
package com.javazhan.domain;

public class Student {
    private int id ;
    private String name ;
    private int age ;
    private String address ;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getAddress() {
        return address;
    }
    public void setAddress(String address) {
        this.address = address;
    }   
}
```
web层 StudentController .java
```
package com.javazhan.controller;

import java.util.ArrayList;
import java.util.List;

import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;

import com.javazhan.domain.Student;

@Controller
public class StudentController {
    
    @RequestMapping("/getStudentList")
    public String getStudentList(ModelMap map) {
        List<Student> list = new ArrayList<Student>() ;
        for(int i=0; i<=5; i++) {
            Student st = new Student() ;
            st.setId(i+1) ;
            st.setName("章三"+(i+1)) ;
            st.setAge(20+i) ;
            st.setAddress("北京故宫门牌号20"+i) ;
            list.add(st) ;
        }
        map.addAttribute("list", list);
        return "index" ;  
    }
}

```

启动类 RunApplication.java
```
package com.javazhan;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class RunApplication {
    
    public static void main(String[] args) {
        SpringApplication.run(RunApplication.class, args) ;
    }
}

```

页面 index.html
```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"  lang="zh-CN">
<head>
    <meta charset="utf-8"></meta>
    <meta http-equiv="X-UA-Compatible" content="IE=edge"></meta>
    <meta name="viewport" content="width=device-width, initial-scale=1"></meta>
    <title>学生信息</title>
    <link rel="stylesheet" type="text/css" href="/css/bootstrap.min.css"></link>
    <link rel="stylesheet" type="text/css" href="/css/style.css"></link>
    <link rel="stylesheet" type="text/css" href="/css/font-awesome.min.css"></link>
</head>
<body>
<!-- 内容 -->
<div class="container">
    <br></br>
    <div class="panel panel-warning">
        <div class="panel-heading"><span class="text-size"><i class="icon-link"></i> 学生所有信息</span></div>
        <div class="panel-body">
            <table class="table table-bordered">
               <thead>
                 <tr>
                    <th>ID</th>
                    <th>姓名</th>
                    <th>年龄</th>
                    <th>地址</th>
                 </tr>
               </thead>
               <tbody>
                 <tr th:each="list : ${list}">
                    <td th:text="${list.id}"></td>
                    <td th:text="${list.name}"></td>
                    <td th:text="${list.age}"></td>
                    <td th:text="${list.address}"></td>
                  </tr>
               </tbody>
             </table>
        </div>
    </div>
</div>
<!-- 内容 -->
<!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
<script type="text/javascript" src="/js/jquery-1.11.2.min.js"></script>
<script type="text/javascript" src="/js/bootstrap.min.js"></script>
</body>
</html>

```

更多使用技巧请查看[Thymeleaf官网文档](http://www.thymeleaf.org/)

若想修改默认参数配置参考如下，如缓存，编码，修改默认的模板路径等。

```
# Enable template caching.
spring.thymeleaf.cache=true 
# Check that the templates location exists.
spring.thymeleaf.check-template-location=true 
# Content-Type value.
spring.thymeleaf.content-type=text/html 
# Enable MVC Thymeleaf view resolution.
spring.thymeleaf.enabled=true 
# Template encoding.
spring.thymeleaf.encoding=UTF-8 
# Comma-separated list of view names that should be excluded from resolution.
spring.thymeleaf.excluded-view-names= 
# Template mode to be applied to templates. See also StandardTemplateModeHandlers.
spring.thymeleaf.mode=HTML5 
# Prefix that gets prepended to view names when building a URL.
spring.thymeleaf.prefix=classpath:/templates/ 
# Suffix that gets appended to view names when building a URL.
spring.thymeleaf.suffix=.html  spring.thymeleaf.template-resolver-order= # Order of the template resolver in the chain. spring.thymeleaf.view-names= # Comma-separated list of view names that can be resolved.
```

#### **源码下载**

[SpringBoot进阶之模板引擎源码](https://github.com/yandongquan/SpringBootInstance/tree/master/SpringBootTemplate)
