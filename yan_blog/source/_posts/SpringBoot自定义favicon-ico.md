---
title: SpringBoot自定义favicon.ico
date: 2018-04-12 10:05:55
tags:
 - SpringBoot
categories: 
 - 分布式
copyright: true
---

### **默认的Favicon**
Spring Boot提供了一个默认的Favicon，每次访问应用的时候都能看到。

![默认的Favicon](http://img.blog.csdn.net/20180126114007899?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### **关闭Favicon**
我们可以在application.properties中设置关闭Favicon，默认为开启。

```
spring.mvc.favicon.enable=false 
```
<!-- more -->

或在application.yml中设置关闭Favicon

```
spring:
  mvc:
    favicon:
      enabled: false
```

### **设置自己的Favicon**
若需要设置自己的Favicon，则只需将自己的favicon.ico文件放置在类路径根目录、类路径META-INF/resources/下、类路径resources/下、类路径static/下或类路径public/下。

这里将favicon.ico放置在src/main/resources/static下。

![这里写图片描述](http://img.blog.csdn.net/20180126114340435?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### **源码分析**
application.properties
```
spring.mvc.favicon.enabled=false
```
IndexController .java
```
@Controller
public class IndexController {

    @RequestMapping(value = "/index")
    public String index(Model model) {
        model.addAttribute("name","SpringBootFavicon");
        return "index";
    }
}
```

IndexRestController .java
```
@RestController
public class IndexRestController {

    @RequestMapping(value = "/indexRest")
    public String index() {
        return "indexRest";
    }
}
```
RunApplication .java
```
@SpringBootApplication
public class RunApplication {

    public static void main(String[] args) {
        SpringApplication.run(RunApplication.class, args) ;
    }
}
```
运行RunApplication,java

### **效果图**

访问Rest请求

![访问Rest请求](http://img.blog.csdn.net/20180126114415775?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

访问页面

![访问页面](http://img.blog.csdn.net/20180126114256857?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

访问错误请求
![访问错误请求](http://img.blog.csdn.net/20180126114400072?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### **源码下载**
[SpringBoot自定义favicon.ico(含源码)](https://github.com/yandongquan/SpringBootInstance/tree/master/SpringBootFavicon)

[SpringBoot 入门+实战系列源码)](https://github.com/yandongquan/SpringBootInstance)
