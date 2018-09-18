---
title: SpringMVC 返回 json 字符串中文乱码
date: 2018-04-19 09:12:37
tags:
 - SpringMVC
categories: 
 - 后台
copyright: true
---
#### 原因

>最近在写一些小的Demo案例，但是被AJAX的 json 返回乱码折磨了好久。最后通过研究StringHttpMessageConverter源代码发现，开发者很坑的使用了"ISO-8859-1"作为默认编码。经过代码测试，下面给出四种方法解决SpringMVC 返回 json 字符串中文乱码。（本文spring版本4.3.11.RELEASE）

<!-- more -->

```
public class StringHttpMessageConverter extends AbstractHttpMessageConverter<String> {
    public static final Charset DEFAULT_CHARSET = Charset.forName("ISO-8859-1");
    private volatile List<Charset> availableCharsets;
    private boolean writeAcceptCharset;
    // 后面省略
```
![乱码](https://img-blog.csdn.net/20180403144507128?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlbnRlcnlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 方法一：自己编写一个工具类

学过servlet我们可以知道，可以通过HttpServletResponse设置返回编码。

工具类
```
public class ResponseTool {

    public static void write(HttpServletResponse response,Object o)throws Exception{
        response.setContentType("text/html;charset=utf-8");
        PrintWriter out=response.getWriter();
        out.println(o.toString());
        out.flush();
        out.close();
    }
}
```
测试方法
```
@RequestMapping(value = "/toolToJson")
public void toolToJson(HttpServletResponse response) throws Exception {
    Map map = new HashMap();
    map.put("code", "200");
    map.put("msg", "这是一句话，测试返回json是否乱码。");
    ResponseTool.write(response,new Gson().toJson(map));
}
```
![工具类](https://img-blog.csdn.net/20180403144547210?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlbnRlcnlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


#### 方法二：使用produces属性

在RequestMapping使用（produces = "text/html; charset=utf-8"）produces 作用根据请求头中的Accept进行匹配，如请求头“Accept:text/html”时即可匹配。

如果类型是：application/json ，设置为：produces = "application/json; charset=utf-8"  
测试方法
```
@RequestMapping(value = "/producesToJson", method = RequestMethod.GET, produces="text/html;charset=UTF-8")
@ResponseBody
public String producesToJson() {
    Map map = new HashMap();
    map.put("code", "200");
    map.put("msg", "这是一句话，测试返回json是否乱码。");
    return new Gson().toJson(map);
}
```
![使用produces属性](https://img-blog.csdn.net/20180403145012839?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlbnRlcnlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


#### 方法三：通过配置spring-mvc.xml


在spring-mvc.xml添加配置
```
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <property name="supportedMediaTypes">
                <list>
                    <value>text/html;charset=UTF-8</value>
                    <value>application/json;charset=UTF-8</value>
                    <value>text/plain;charset=UTF-8</value>
                    <value>application/xml;charset=UTF-8</value>
                </list>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```
测试方法

```
@RequestMapping(value = "/json")
@ResponseBody
public String json() {
    Map map = new HashMap();
    map.put("code", "200");
    map.put("msg", "这是一句话，测试返回json是否乱码。");
    return new Gson().toJson(map);
}
```
![重写](https://img-blog.csdn.net/2018040314595757?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlbnRlcnlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 方法四：完成自己的AbstractHttpMessageConverter

分析源码可以看出StringHttpMessageConverter继承与AbstractHttpMessageConverter<String>，分析该抽象类得，相关的操作字符编码的方法可以重写，于是我们可以自定义一个类继承StringHttpMessageConverter，然后重写相关的方法。这里我们只需要把ISO-8859-1改成UTF-8，并修改构造方法。


UTF8StringHttpMessageConverter 类
```
package com.javazhan.controller;

import org.springframework.http.HttpInputMessage;
import org.springframework.http.HttpOutputMessage;
import org.springframework.http.MediaType;
import org.springframework.http.converter.AbstractHttpMessageConverter;
import org.springframework.http.converter.HttpMessageNotReadableException;
import org.springframework.http.converter.HttpMessageNotWritableException;
import org.springframework.util.StreamUtils;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.nio.charset.Charset;
import java.util.ArrayList;
import java.util.List;

/**
 * @Author: yandq
 * @Description:
 * @Date: Create in 11:50 2018/4/3
 * @Modified By:
 */
public class UTF8StringHttpMessageConverter extends AbstractHttpMessageConverter<String> {
    public static final Charset DEFAULT_CHARSET = Charset.forName("UTF-8");

    private volatile List<Charset> availableCharsets;
    private boolean writeAcceptCharset;

    public UTF8StringHttpMessageConverter() {
        this(DEFAULT_CHARSET);
    }

    public UTF8StringHttpMessageConverter(Charset defaultCharset) {
        super(defaultCharset, new MediaType[]{MediaType.TEXT_PLAIN, MediaType.ALL});
        this.writeAcceptCharset = true;
    }

    public void setWriteAcceptCharset(boolean writeAcceptCharset) {
        this.writeAcceptCharset = writeAcceptCharset;
    }

    @Override
    public boolean supports(Class<?> clazz) {
        return String.class == clazz;
    }

    @Override
    protected String readInternal(Class<? extends String> clazz, HttpInputMessage inputMessage) throws IOException {
        Charset charset = this.getContentTypeCharset(inputMessage.getHeaders().getContentType());
        return StreamUtils.copyToString(inputMessage.getBody(), charset);
    }

    @Override
    protected Long getContentLength(String str, MediaType contentType) {
        Charset charset = this.getContentTypeCharset(contentType);

        try {
            return (long)str.getBytes(charset.name()).length;
        } catch (UnsupportedEncodingException var5) {
            throw new IllegalStateException(var5);
        }
    }

    @Override
    protected void writeInternal(String str, HttpOutputMessage outputMessage) throws IOException {
        if (this.writeAcceptCharset) {
            outputMessage.getHeaders().setAcceptCharset(this.getAcceptedCharsets());
        }

        Charset charset = this.getContentTypeCharset(outputMessage.getHeaders().getContentType());
        StreamUtils.copy(str, charset, outputMessage.getBody());
    }

    protected List<Charset> getAcceptedCharsets() {
        if (this.availableCharsets == null) {
            this.availableCharsets = new ArrayList(Charset.availableCharsets().values());
        }

        return this.availableCharsets;
    }

    private Charset getContentTypeCharset(MediaType contentType) {
        return contentType != null && contentType.getCharset() != null ? contentType.getCharset() : this.getDefaultCharset();
    }
}

```
定义好上面的类后，只需要将该类注册到Spring的annotaion 处理序列中即可，当加上@ResponseBody时，Spring将会调用上面自定义类中复写的方法，从而返回UTF-8的编码：

```
<mvc:annotation-driven>
      <mvc:message-converters register-defaults="true">
            <bean class="com.javazhan.controller.UTF8StringHttpMessageConverter"/>
      </mvc:message-converters>
</mvc:annotation-driven>
```
测试方法

```
@RequestMapping(value = "/json")
@ResponseBody
public String json() {
    Map map = new HashMap();
    map.put("code", "200");
    map.put("msg", "这是一句话，测试返回json是否乱码。");
    return new Gson().toJson(map);
}
```
![重写](https://img-blog.csdn.net/2018040314595757?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlbnRlcnlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
