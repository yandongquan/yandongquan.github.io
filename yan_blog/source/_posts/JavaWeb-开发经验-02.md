---
title: JavaWeb 开发经验 02
date: 2019-04-26 16:56:00
tags:
- JavaWeb
- 开发经验
categories:
- 开发经验
---

<img style="height:75px" src="https://i.loli.net/2019/06/24/5d10392ae815c23251.png" alt="Java">

## java 对象转成 JSON 字符串，出现  $ref

原因：List 里含有重复对象
使用：DisableCircularReferenceDetect 来禁止循环引用检测

<!-- more -->
```
JSON.toJSONString(list, SerializerFeature.DisableCircularReferenceDetect)
```

> 引用是通过`$ref`来表示的

引用 | 描述
---|---
"$ref":".." |   上一级
"$ref":"@" |    当前对象，也就是自引用
"$ref":"$" |    根对象
"$ref":"$.children.0" | 基于路径的引用，相当于 root.getChildren().get(0)

## freemarker 判断对象是否为空
```
<#if name??>
${name }
</#if>

${name!'' }
```

## button 标签（问题：回车会提交的）

submit  该按钮是提交按钮（除了 Internet Explorer，该值是其他浏览器的默认值）

## 校验手机格式

由于运营商号段随时会变，故采取非严谨的规则。
- Java 版本

```java
/**
 * 校验手机号
 * @param number
 * @return
 */
private boolean checkPhoneNumber(String number) {
    String regex = "^[1]([3-9])[0-9]{9}$";
    Pattern pattern = Pattern.compile(regex);
    return pattern.matcher(number).matches();
}
```

- JS 版本

```js
var telStr = "/^[1]([3-9])[0-9]{9}$/";
if (!(telStr.test(number))) {
    console.log("手机号格式不合法");
}

```
## CSS 怎么引用字体包

加入一下代码，css 直接引用字体名称即可。

```css
@font-face {
  font-family: 'MyNewFont';   /*字体名称*/
  src: url('font.ttf');       /*字体源文件*/
}
```


<span style="color:#CA0C16">持续更新中...</span>
<img src="/images/Come on/Come on8.gif">
