---
title: JavaWeb 开发经验 02
date: 2019-04-26 16:56:00
tags:
- JavaWeb
- 开发经验
categories:
- 开发经验
---

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

<span style="color:#CA0C16">持续更新中...</span>
<img src="/images/Come on/Come on8.gif">
