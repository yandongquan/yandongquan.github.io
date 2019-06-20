---
title: JavaWeb 开发经验 01
date: 2019-04-20 16:31:33
tags:
- JavaWeb
- 开发经验
categories:
- 开发经验
---

## Set 无序不重复可用于比较对象

Set 里的元素无放入顺序，元素不可重复。

<!-- more -->

```
Person person1 = new Person("张三", "男", 20);
Person person2 = new Person("张三", "男", 20);
Set<Person> hasSet = new HashSet<>();
hasSet.add(person1);
hasSet.add(person2);
System.out.println(hasSet.size());
```
## 字符串转 `List<Class>`

使用的是 `JSONArray.parseArray` 而不是 `JSONArray.parseObject`

```
List<Mobile> mobiles = (List<Mobile>) JSONArray.parseArray(str, Mobile.class);
```

## 关于`ajaxupload.js`上传图片问题

谷歌浏览器慎用有道词典，有道词典会导致body里面多一个元素
`<audio controls="controls" style="display: none;"></audio>`

大多数的上传插件，为了实现无刷新页面上传，通常都会构建一个虚拟的 iframe 和 form，比如 ajaxupload，它会把 form 的 target 属性指定为 iframe 中的 name 值，目的是指定返回的页面在哪里打开，上传一般都是返回的 json 字符串，所以这时候返回 json 字符串就会被添加到 iframe 的 body 中，再获取 iframe 中 body 的值作为上传文件的返回结果。有道词典会在返回结果中多了一行`<audio controls="controls" style="display: none;"></audio>` 

- 可以关掉插件
- 更换浏览器

## Json字符串有转义字符

分析：Json 多次转String会产生转义字符如`"，\`
解决方法：封装方法。
```
/**
 * 处理 json 字符串多出前后双引号和转义符
 * @param rspStr
 * @return
 */
public static String jsonRemoveEscaping(String rspStr) {
    rspStr = rspStr.replace("\\","").replace("\"{","{").replace("}\"","}");
    return rspStr;
}
```

## 携程 Apollo 配置中心本地启动

> 注意事项

- 一般只需要在`/opt/settings/server.properties`中配置了 env=DEV 就可以直接直接启动（因为 Client 在本地仓库的包上已经有了 meta_server 的信息）
- IDE 上也可以通过指定 VM 的参数，增加系统属性变量 -D 来实现调试

```properties
-Denv=DEV -Ddev_meta=http://10.20.25.119:18020
```

## 判断线程池执行完，再执行下一步

- isShutDown当调用shutdown()或shutdownNow()方法后返回为true。 
- isTerminated当调用shutdown()方法后，并且所有提交的任务完成后返回为true;
- isTerminated当调用shutdownNow()方法后，成功停止后返回为true;
- 如果线程池任务正常完成，都为false

```java
List<Integer> typeList = new ArrayList<>();  
typeList.add(1);  
typeList.add(2);  
typeList.add(3);
ExecutorService pool = Executors.newFixedThreadPool(5);
try {
    for (final int type:typeList) {
        pool.execute(new Runnable() {
            @Override
            public void run() {
                System.out.println(type);
            }
        });
    }
} finally {
    if (pool != null) {
        pool.shutdown();
    }
}

while (!pool.isTerminated()) {
    // 等待所有子线程结束
}
System.out.println("end");

// 或者
// if(pool.isTerminated()){执行完后，要执行的部分（这个会往后面继续走的）}

```

## ibatis 查询日期会去掉时分秒

- 写 sql 的时候可以用 to_char 来转换
- 自定义转换

**自定义转换**

通过配置一个 TypeHandler，让 TypeHandler 在转换的时候把`java.sql.Date`转换成`java.sql.Timestamp`。

在 sqlMapConfig 中配一下:

```xml
<typeHandler javaType="object" callback="xxx.xxx.OracleObjectTypeHandler"/>
```

代码
```
public class OracleObjectTypeHandler extends BaseTypeHandler implements
        TypeHandler {
    public Object getResult(ResultSet rs, String columnName)
            throws SQLException {
        Object object = rs.getObject(columnName);
        if (rs.wasNull()) {
            return null;
        } else {
            boolean b = object instanceof java.sql.Date;
            if (b)
                object = new Date(rs.getTimestamp(columnName).getTime());
            return object;
        }
    }
 
    public Object getResult(ResultSet rs, int columnIndex) throws SQLException {
        Object object = rs.getObject(columnIndex);
        if (rs.wasNull()) {
            return null;
        } else {
            boolean b = object instanceof java.sql.Date;
            if (b)
                object = new Date(rs.getTimestamp(columnIndex).getTime());
            return object;
        }
    }
 
    public Object getResult(CallableStatement cs, int columnIndex)
            throws SQLException {
        Object object = cs.getObject(columnIndex);
        if (cs.wasNull()) {
            return null;
        } else {
            boolean b = object instanceof java.sql.Date;
            if (b)
                object = new Date(cs.getTimestamp(columnIndex).getTime());
            return object;
        }
    }
 
    public Object valueOf(String s) {
        return s;
    }
 
    public void setParameter(PreparedStatement ps, int i, Object parameter,
            String jdbcType) throws SQLException {
        ps.setObject(i, parameter);
    }
}
```
## SpringMvc 表单提交时 date 类型

form表单中的数据是基本类型的，对时间类型是不支持的

- 方法一：在对应的 Controller 中新增下面的方法（针对一个类）

```java
/** 
* form表单提交 Date类型数据绑定 
* @param binder 
*/  
@InitBinder    
public void initBinder(WebDataBinder binder) {    
    SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    dateFormat.setLenient(false);    
    binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true));    
}  
```

- 方法二：在实体类中添加注解

```java
@DateTimeFormat(pattern="yyyy-MM-dd") 
private Date birthday;
```

> 注：配置`<mvc:annotation-driven/> `，默认就启用 FormattingConversionServiceFactoryBean 了。

## idea 工程上传的图片，页面显示不出来

1）idea里面配置`static/image/upload`文件资源

![idea里面配置static/image/upload文件资源](https://i.loli.net/2019/06/20/5d0b450872aa152027.png
)

2）idea中tomcat发布项目的默认路径是项目所在地里的target目录里面，修改工程输出到Tomcat下

![修改工程输出到Tomcat下](https://i.loli.net/2019/06/20/5d0b4508dad9c11853.png
)

<img src="/images/Come on/Come on7.gif">
