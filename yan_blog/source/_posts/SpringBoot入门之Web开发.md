---
title: SpringBoot入门之Web开发
date: 2018-04-11 11:31:42
tags:
 - SpringBoot
 - Web
categories: 
 - 分布式
copyright: true
---

#### **静态资源目录**

Spring Boot默认提供静态资源目录位置需置于classpath下，目录名需符合如下规则：
```
/static
/public
/resources
/META-INF/resources
```

<!-- more -->

![public love.gif](http://img.blog.csdn.net/20170904100613965?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![静态资源图片gif](http://img.blog.csdn.net/20170904100502316?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### **配置文件**

Spring Boot项目使用一个全局的配置文件application.properties或者是application.yml，在resources目录下或者类路径下的/config下，一般我们放到resources下。

修改tomcat端口配置

```
server.port=8888
```

修改Spring MVC拦截路径规则
默认Spring MVC拦截路径规则是/，如果要修改成*.html的话，可以在全局配置文件中进行如下设置：

```
server.servlet-path=*.html
```

视图解析器配置
一样的，Spring Boot也可以通过全局配置文件对视图解析器进行配置：

```
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

日志输出
Spring Boot对各种日志框架都做了支持，我们可以通过配置来修改默认的日志的配置：

```
#设置日志级别
logging.level.org.springframework=DEBUG
```

application.yml

properties与yml文件在形式上有所差别，yml文件的书写方式比较简洁，类似eclipse中package的flag呈现方式（而properties文件则像Hierarchical方式）。如上面properties文件中的属性配置使用yml文件来写：

```
server:
  port: 8888
  context-path: /

spring:
    mvc:
      view:
        prefix: /WEB-INF/views/
        suffix: .jsp

logging:
  level: 
    org:
      springframework: debug
```

![配置文件](http://img.blog.csdn.net/20170904101404072?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![效果图](http://img.blog.csdn.net/20170904101553043?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

<font color='red'>注：推荐使用yml文件在书写，需要注意一个地方：冒号与值中间是存在空格的！</font>




Spring Boot的全局配置很强大，同时它可以配置的属性也很多，以上只列出几个常用的属性配置，如需查看完整的全局属性配置，请到[spring-boot官方配置文档](https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html)查看。


#### **源码下载**

[SpringBoot入门之Web开发源码](https://github.com/yandongquan/SpringBootInstance/tree/master/SpringBootWeb)
