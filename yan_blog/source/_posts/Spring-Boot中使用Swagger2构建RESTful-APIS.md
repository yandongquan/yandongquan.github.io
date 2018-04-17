---
title: Spring Boot中使用Swagger2构建RESTful APIS
date: 2018-04-16 15:06:53
tags:
 - SpringBoot
categories: 
 - 分布式
copyright: true
---
#### **Swagger2简介**

本次教程是Spring Boot中使用Swagger2构建RESTful APIS
Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。（如图）

<!-- more -->
![Spring Boot中使用Swagger2构建RESTful APIS效果图](http://img.blog.csdn.net/20171113125648734?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

Swagger除了查看接口功能外，还提供了调试测试功能。（如图）
新增博客

![新增](http://img.blog.csdn.net/20171113135922229?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![新增](http://img.blog.csdn.net/20171113135932356?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

查看所有博客

![查看所有](http://img.blog.csdn.net/20171113135951247?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

修改博客

![修改](http://img.blog.csdn.net/20171113140003773?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


查看单个博客

![查看单个](http://img.blog.csdn.net/20171113140219853?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![查看单个](http://img.blog.csdn.net/20171113140019135?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

删除博客

![输入要删除的id](http://img.blog.csdn.net/20171113140552417?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![删除成功](http://img.blog.csdn.net/20171113140600519?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![查看是否删除](http://img.blog.csdn.net/20171113140613843?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### **SpringBoot整合Swagger2**

**配置pom.xml,引入Swagger2架包**

```
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.7.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.7.0</version>
</dependency>
```

**在RunApplication.java同级下创建Swagger2.java**

```
package com.javazhan;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.*;
import springfox.documentation.service.*;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

/**
 * Created by yando on 2017/11/10.
 */

@Configuration
@EnableSwagger2
public class Swagger2 {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.any())
                .build()
                .apiInfo(apiInfo());
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Spring Boot中使用Swagger2构建RESTful APIS")
                .description("HTTP对外开放接口")
                .version("1.0.0")
                .termsOfServiceUrl("http://blog.csdn.net/wenteryan")
                .license("Spring Boot 入门+实战（提供源码哟）")
                .licenseUrl("http://blog.csdn.net/column/details/15021.html")
                .build();
    }
}

```

**创建实体类 Blog.java（略）** 

**创建ABlogController.java**

```
package com.javazhan.controller.admin;

import com.javazhan.domain.Blog;
import io.swagger.annotations.ApiImplicitParam;
import io.swagger.annotations.ApiImplicitParams;
import io.swagger.annotations.ApiOperation;
import org.springframework.web.bind.annotation.*;

import java.util.*;

/**
 * Created by yando on 2017/11/13.
 */
@RestController
@RequestMapping(value = "/admin/blog")
public class ABlogController {

    static Map<Long, Blog> blogs = Collections.synchronizedMap(new HashMap<Long, Blog>()) ;

    @ApiOperation(value = "后台管理查询所有博客")
    @RequestMapping(value = "", method = RequestMethod.GET)
    public List<Blog> getBlogList() {
        List<Blog> list = new ArrayList<Blog>(blogs.values()) ;
        return list ;
    }

    @ApiOperation(value = "新增博客", notes = "根据博客对象")
    @ApiImplicitParam(name = "blog", value = "博客信息实体blog", required = true, dataType = "Blog")
    @RequestMapping(value = "/add", method = RequestMethod.POST)
    public Map addBlog(@RequestBody Blog blog) {
        blogs.put(blog.getId(), blog) ;
        Map map = new HashMap() ;
        map.put("message", "新增成功") ;
        map.put("code", "0000") ;
        return map ;
    }

    @ApiOperation(value = "获取博客信息", notes = "根据ID获取博客对象")
    @ApiImplicitParam(name = "id", value = "博客ID", required = true, paramType="path", dataType = "Long")
    @RequestMapping(value = "/{id}", method = RequestMethod.GET)
    public Blog getBlogById(@PathVariable Long id) {
        Blog blog = new Blog() ;
        blog = blogs.get(id) ;
        return blog ;
    }

    @ApiOperation(value = "修改博客信息", notes = "根据传过来的博客对象修改博客信息")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "id", value = "博客ID", required = true, paramType="path", dataType = "Long"),
            @ApiImplicitParam(name = "blog", value = "博客信息实体blog", required = true, dataType = "Blog")
    })
    @RequestMapping(value = "/{id}", method = RequestMethod.PUT)
    public Map updateBlog(@PathVariable Long id, @RequestBody Blog blog) {
        Blog b = blogs.get(blog.getId()) ;
        b.setId(blog.getId()) ;
        b.setName(blog.getName()) ;
        blogs.put(b.getId(), b) ;
        Map map = new HashMap() ;
        map.put("message", "修改成功") ;
        map.put("code", "0000") ;
        return map ;
    }

    @ApiOperation(value = "删除博客信息", notes = "根据ID删除博客对象")
    @ApiImplicitParam(name = "id", value = "博客ID", required = true, paramType="path", dataType = "Long")
    @RequestMapping(value = "/{id}", method = RequestMethod.DELETE)
    public Map deleteBlog(@PathVariable Long id) {
        blogs.remove(id) ;
        Map map = new HashMap() ;
        map.put("message", "删除成功") ;
        map.put("code", "0000") ;
        return map ;
    }
}

```

运行RunApplication.java
访问地址：http://localhost:8080/swagger-ui.html

![Spring Boot中使用Swagger2构建RESTful APIS效果图](http://img.blog.csdn.net/20171113125648734?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

更多教程请参考[官方文档](http://springfox.github.io/springfox/docs/current/#introduction)

#### **源码下载**

[Spring Boot中使用Swagger2构建RESTful APIS（含源码）](https://github.com/yandongquan/SpringBootInstance/tree/master/SpringBootSwagger)
