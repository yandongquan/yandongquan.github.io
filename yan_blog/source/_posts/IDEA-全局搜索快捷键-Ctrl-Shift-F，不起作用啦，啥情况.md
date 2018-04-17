---
title: IDEA 全局搜索快捷键 Ctrl +Shift+F，不起作用啦，啥情况
date: 2018-04-17 18:03:25
tags:
 - IDEA
categories: 
 - 工具
copyright: true
---
#### **问题描述**

IDEA 工具很强大，其中有个全局搜索快捷键：Ctrl +Shift+F也是在开发中经常用到的，但是不知道为什么按了就是不起作用，原来是和<font color=red>输入法的简繁体切换冲突了</font> 

#### **给出一下三种解决方法**

##### **方案一   如你不想要输入法的简繁体切换快捷键，win10 最新版2017年7月可以直接取消简繁体切换快捷键（搜狗输入法可以在设置里改）如下**

<!-- more -->

打开win设置（右键任务栏左下角微软的LOGO，单击设置）

![这里写图片描述](http://img.blog.csdn.net/20171019101300321?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

点击时间和语言，区域和语言，中文，选项

![这里写图片描述](http://img.blog.csdn.net/20171019101500969?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

微软拼音，选项

![这里写图片描述](http://img.blog.csdn.net/20171019101723945?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

点击按键

![这里写图片描述](http://img.blog.csdn.net/20171019102004724?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

关掉简繁体输入切换

![这里写图片描述](http://img.blog.csdn.net/20171019102018959?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在我们的idea中试一下，哈哈终于可以了。。。（赶紧撸代码吧）

![这里写图片描述](http://img.blog.csdn.net/20171019102253149?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### **方案二，若不是最新版或者其他系统，可以在控制面板里改。如下**

我的是win10最新版（右键任务栏左下角微软的LOGO，单击控制面板已取消，不知道为什么，哈哈哈）

我们可以这样<font color=red>打开运行框（windows键+R）,输入Control</font>

![这里写图片描述](http://img.blog.csdn.net/20171019102901531?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

按这个顺序进去
控制面板\时钟、语言和区域\语言\高级设置

![这里写图片描述](http://img.blog.csdn.net/20171019102838557?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

更改语言栏热键

![这里写图片描述](http://img.blog.csdn.net/20171019103048375?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 
 在输入语言之间，更改按键顺序，Ctrl+Shift，应用，确定
![这里写图片描述](http://img.blog.csdn.net/20171019103427573?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在我们的idea中试一下，哈哈终于可以了。。。（赶紧撸代码吧）

![这里写图片描述](http://img.blog.csdn.net/20171019102253149?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

<font color=red>如你的没有好使，不要急于给我评论，问我这个问题，看看你的输入法是不是中文，切换成英文，再次按Ctrl +Shift+F，是不是出来了</font>


##### **方案三  若你要保留输入法的简繁体切换快捷键，可以在idea的全局搜索快捷键，新增一个快捷键，如：Ctrl +Shift+o。**

Ctrl +Alt+S 打开配置选择 Keymap，执行以下三步找到全局搜索快捷键
第一步：点击shortcut查询
第二步：勾上 second stroke
第三步：在输入，按下Ctrl +Shift+F
![这里写图片描述](http://img.blog.csdn.net/20171019095457214?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在Find in Path右键，Add Keyboard Shortcut

![这里写图片描述](http://img.blog.csdn.net/20171019095750705?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

直接按下你想用的快捷键（不要和其他的冲突了），如：Ctrl +Shift+o
应用保存，就好了。

在我们的idea中试一下，哈哈终于可以了。。。（赶紧撸代码吧）

![这里写图片描述](http://img.blog.csdn.net/20171019102253149?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VudGVyeWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
