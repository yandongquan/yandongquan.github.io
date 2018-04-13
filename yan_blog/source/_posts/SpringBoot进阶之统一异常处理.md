---
title: SpringBoot进阶之统一异常处理
date: 2018-04-13 17:55:37
tags:
 - SpringBoot
categories: 
 - 分布式
copyright: true
---

#### **浅谈异常处理**

在J2EE项目的开发中，不管是对底层的数据库操作过程，还是业务层的处理过程，还是控制层的处理过程，都不可避免会遇到各种可预知的、不可预知的异常需要处理。每个过程都单独处理异常，系统的代码耦合度高，工作量大且不好统一，维护的工作量也很大。 所以我们会进行统一异常处理，进而去避免这些问题。

#### **默认异常处理**

Spring Boot提供了一个默认的映射：/error，当处理中抛出异常之后，会转到该请求中处理，并且该请求有一个全局的错误页面用来展示异常内容。

<!-- more -->


启动该应用，访问一个不存在的URL。

![错误页面](http://img.blog.csdn.net/20170905163307912?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### **统一异常处理，场景1 返回HTML页面**

编写全局异常处理类 GlobalExceptionHandler.java

```
// 通过使用@ControllerAdvice定义统一的异常处理类，而不是在每个Controller中逐个定义。
@ControllerAdvice
public class GlobalExceptionHandler {

    public static final String DEFAULT_ERROR_VIEW = "error";
    // @ExceptionHandler用来定义函数针对的异常类型，最后将Exception对象和请求URL映射到error.html中
    @ExceptionHandler(value = Exception.class)
    public ModelAndView defaultErrorHandler(HttpServletRequest request, Exception e) throws Exception {
        ModelAndView mav = new ModelAndView() ;
        mav.addObject("errorname", "统一异常处理页面") ;
        mav.addObject("exception", e) ;
        mav.addObject("url", request.getRequestURL()) ;
        mav.setViewName(DEFAULT_ERROR_VIEW) ;
        return mav ;
    }
}
```

编写异常类 ExceptionController.java

```
@Controller
public class ExceptionController {
    
    @RequestMapping("/nException")
    public String nException() throws Exception {
        throw new Exception("这里有个错误异常") ;
    }
}
```

编写error.html页面

```
<div class="container">
    <br></br>
    <div class="alert alert-warning alert-dismissible" role="alert">
      <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
      <h2 th:text="${errorname }"></h2>
      <p th:text="${exception.message }"></p>
      <p th:text="${url }"></p>
    </div>
</div>
```
启动应用，访问：http://localhost:8080/nException

![统一异常处理页面](http://img.blog.csdn.net/20170905163333045?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### **统一异常处理，场景2 返回JSON页面**

编写ErrorInfo.java实体类

```
package com.javazhan.domain;

public class ErrorInfo<T> {
    public static final Integer OK = 0000 ;
    public static final Integer ERROR = 9999 ;

    private Integer code ;
    private String message ;
    private String url ;
    private T data ;
    public Integer getCode() {
        return code;
    }
    public void setCode(Integer code) {
        this.code = code;
    }
    public String getMessage() {
        return message;
    }
    public void setMessage(String message) {
        this.message = message;
    }
    public String getUrl() {
        return url;
    }
    public void setUrl(String url) {
        this.url = url;
    }
    public T getData() {
        return data;
    }
    public void setData(T data) {
        this.data = data;
    }  
}

```

在全局异常处理类 GlobalExceptionHandler.java编写jsonErrorHandler方法

```
@ExceptionHandler(value = MyException.class)
@ResponseBody
public ErrorInfo<String> jsonErrorHandler(HttpServletRequest request, MyException e) throws Exception {
    ErrorInfo<String> r = new ErrorInfo<>() ;
    r.setMessage(e.getMessage()) ;
    r.setCode(ErrorInfo.ERROR) ;
    r.setData("Some Data") ;
    r.setUrl(request.getRequestURL().toString()) ;
    return r ;
}
```

在ExceptionController.java新增jsonException方法
```
@RequestMapping("/jsonException")
public String jsonException() throws MyException {
    throw new MyException("这里有个错误异常");
}
```
启动应用，访问：http://localhost:8080/jsonException

![返回JSON页面](http://img.blog.csdn.net/20170905164335250?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### **源码下载**

[SpringBoot进阶之统一异常处理（含源码）](https://github.com/yandongquan/SpringBootInstance/tree/master/SpringBootException)

