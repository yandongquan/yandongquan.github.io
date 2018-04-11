---
title: SpringBoot进阶之访问数据库
date: 2018-04-11 11:39:59
tags:
 - SpringBoot
 - JdbcTemplate
categories: 
 - 分布式
copyright: true
---
本文介绍在Spring Boot基础下配置数据源和通过JdbcTemplate编写数据访问的示例。

简单介绍一下
@Controller：修饰class，用来创建处理http请求的对象
@RestController：Spring4之后加入的注解，原来在@Controller中返回json需要@ResponseBody来配合，如果直接用@RestController替代@Controller就不需要再配置@ResponseBody，默认返回json格式。
@RequestMapping：配置url映射

#### **数据源配置**


<!-- more -->

首先，为了连接数据库需要引入jdbc支持，在pom.xml中引入如下配置：
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

以MySQL数据库为例，先引入MySQL连接的依赖包，在pom.xml中加入：

```
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.21</version>
</dependency>
```

#### **配置数据源信息**

在application.yml配置数据源信息
```
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver
```

#### **使用JdbcTemplate操作数据库**

编写实体类Student.java

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

编写StudentService.java

```
package com.javazhan.service;

import java.util.List;

import com.javazhan.domain.Student;

public interface StudentService {
    
    /**
     * 新增一个学生
     * @param student
     */
    void create(Student student) ;
    
    /**
     * 根据id删除一个学生
     * @param id
     */
    void deleteById(int id) ;
    
    /**
     * 查出所有学生
     * @return
     */
    List<Student> getAllStudent() ;
    
    /**
     * 根据id查出学生
     * @return
     */
    Student getStudentById(int id) ;
    
    /**
     * 根据Id更新一个学生
     */
    void updateStudentById(Student student) ;
}

```

编写实现类StudentServiceImpl.java

```
package com.javazhan.service.impl;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.ResultSetExtractor;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Service;

import com.javazhan.domain.Student;
import com.javazhan.service.StudentService;

@Service
public class StudentServiceImpl implements StudentService {
    
    @Autowired
    private JdbcTemplate jdbcTemplate  ;

    @Override
    public void create(Student student) {
        // TODO Auto-generated method stub
        jdbcTemplate.update("insert into student(name, age, address) value(?,?,?)", student.getName(), student.getAge(), student.getAddress()) ;
    }

    @Override
    public void deleteById(int id) {
        // TODO Auto-generated method stub
        jdbcTemplate.update("delete from student where id = ?", id) ;
    }

    @Override
    public List<Student> getAllStudent() {
        // TODO Auto-generated method stub
        return jdbcTemplate.query("select * from student", new RowMapper()
        {
            @Override
            public Object mapRow(ResultSet rs, int rowNum) throws SQLException
            {
                Student student = new Student();
                student.setId(rs.getInt("id"));
                student.setName(rs.getString("name"));
                student.setAge(rs.getInt("age"));
                student.setAddress(rs.getString("address"));
                return student;
            }
        }) ;
    }
    
    @Override
    public Student getStudentById(int id) {
        // TODO Auto-generated method stub
        return (Student) jdbcTemplate.query("select * from student where id=?", new ResultSetExtractor() {  
            @Override  
            public Student extractData(ResultSet rs) throws SQLException {  
                while (rs.next()) {
                    Student student = new Student();
                    student.setId(rs.getInt("id"));
                    student.setName(rs.getString("name"));
                    student.setAge(rs.getInt("age"));
                    student.setAddress(rs.getString("address"));
                    return student ;
                }
                return null ;
            }  
        }, id);  
    }

    @Override
    public void updateStudentById(Student student) {
        // TODO Auto-generated method stub
        jdbcTemplate.update("update student set name=?, age=?, address=? where id =?", student.getName(), student.getAge(), student.getAddress(),student.getId()) ;
    }
    
}

```

我们尝试使用Spring MVC来实现一组对Student对象操作的RESTful API，配合注释详细说明在Spring MVC中如何映射HTTP请求、如何传参、如何编写单元测试。
| 请求类型 | URL    | 功能说明 |
| ------------- |:-------------| -----|
| GET | /student | 查询所有学生 |
| POST | /student | 新增一个学生 |
| GET | /student/id | 根据id查询一个学生 |
| PUT | /student/id | 根据id更新一个学生 |
| DELETE | /student/id | 根据id删除一个学生 |

编写StudentRestController .java
```
package com.javazhan.web.rest;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import com.javazhan.domain.Student;
import com.javazhan.service.StudentService;

@RestController
@RequestMapping(value="student")
public class StudentRestController {
    
    @Autowired
    private StudentService stuService ;
    /**
     * 查出所有学生
     * @return
     */
    @RequestMapping(value="/", method=RequestMethod.GET) 
    public List<Student> getAllStudent() {
        return stuService.getAllStudent() ;
    }
    
    /**
     * 根据id查出学生
     * @return
     */
    @RequestMapping(value="/{id}", method=RequestMethod.GET)
    public Student getStudentById(@PathVariable Integer id) {
        return stuService.getStudentById(id) ;
    }
    
    /**
     * 根据id删除学生
     * @return
     */
    @RequestMapping(value="/{id}", method=RequestMethod.DELETE)
    public String deleteById(@PathVariable Integer id) {
        stuService.deleteById(id) ;
        return "success" ;
    }
    
    /**
     * 新增一个学生
     * @return
     */
    @RequestMapping(value="/", method=RequestMethod.POST)
    public String create(@ModelAttribute Student student) {
        stuService.create(student) ;
        return "success" ;
    }
    
    /**
     * 根据Id更新一个学生
     * @return
     */
    @RequestMapping(value="/", method=RequestMethod.PUT)
    public String updateStudentById(@ModelAttribute Student student) {
        stuService.updateStudentById(student) ;
        return "success" ;
    }
}

```

编写测试RestTest.java

```
package com.java.test;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.delete;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.put;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.MvcResult;
import org.springframework.test.web.servlet.RequestBuilder;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

import com.javazhan.RunApplication;

@RunWith(SpringRunner.class)
@SpringBootTest(classes=RunApplication.class)
public class RestTest {
    @Autowired
    private WebApplicationContext context;
    
    // mock api 模拟http请求
    private MockMvc mvc; 
    
    @Before
    public void setUp() throws Exception {
        //集成Web环境测试（此种方式并不会集成真正的web环境，而是通过相应的Mock API进行模拟测试，无须启动服务器）
        mvc = MockMvcBuilders.webAppContextSetup(context).build();
    }
    @Test
    public void testUserController() throws Exception {
        RequestBuilder request = null ;
        MvcResult mvcResult = null ;
        int status = 500 ;
        // 新增学生
        request = post("/student/").param("name", "李四")
                .param("age", "20")
                .param("address", "哈尔滨") ;
        mvcResult = mvc.perform(request).andReturn() ;  
        status = mvcResult.getResponse().getStatus() ;
        if(status==200) {
            String content = mvcResult.getResponse().getContentAsString() ;  
            System.out.println("新增学生："+content) ;
        }
        // 查出所有学生
        request = get("/student/") ;
        mvcResult = mvc.perform(request).andReturn() ;  
        status = mvcResult.getResponse().getStatus() ;
        if(status==200) {
            String content = mvcResult.getResponse().getContentAsString() ;  
            System.out.println("查出所有学生："+content);
        }
        // 根据Id查询学生
        request = get("/student/1") ;
        mvcResult = mvc.perform(request).andReturn() ;  
        status = mvcResult.getResponse().getStatus() ;
        if(status==200) {
            String content = mvcResult.getResponse().getContentAsString() ;  
            System.out.println("根据Id查询学生："+content) ;
        }
        
        // 根据Id更新一个学生
        request = put("/student/").param("id", "5")
                .param("name", "李四5")
                .param("age", "25")
                .param("address", "哈尔滨5") ;
        mvcResult = mvc.perform(request).andReturn() ;  
        status = mvcResult.getResponse().getStatus() ;
        if(status==200) {
            String content = mvcResult.getResponse().getContentAsString() ;  
            System.out.println("根据Id更新一个学生："+content) ;
        }
        
        // 根据id删除一个学生
        request = delete("/student/6") ;
        mvcResult = mvc.perform(request).andReturn() ;  
        status = mvcResult.getResponse().getStatus() ;
        if(status==200) {
            String content = mvcResult.getResponse().getContentAsString() ;  
            System.out.println("根据id删除一个学生："+content) ;
        }
        

    }
}

```
编写RunApplication.java
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

![测试结果](http://img.blog.csdn.net/20170904181825976?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### **源码下载**

[SpringBoot进阶之访问数据库（含源码）](https://github.com/yandongquan/SpringBootInstance/tree/master/SpringBootSQL)
