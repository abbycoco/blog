###网站分为部署网站（对应分支abbycoco.github.io）和源码(blog)
## 从blog分支获取源码
>> git remote add origin https://github.com/abbycoco/blog.git
>> git pull origin master

##新建文章命令
hugo new post/新建文章名字.md
***
新生成的文件会有如下代码：
```+++
title = "AC"
date = 2019-03-14T16:44:25+08:00
tags = ["ACM"]
categories = ["ACM"]
draft = false
description = 'AC的缘由'
+++```
***
###tags就是标签，分组用的,categories就是目录，尽量和tags保持一致，draft是否是草稿。

中文网址：https://www.gohugo.org/


写完之后，本地预览项目，hugo server -D


完成部署项目，hugo，就会生成public文件夹，命令行进入public文件夹。
##初次进入
>> git remote add origin https://github.com/abbycoco/blog.git
>> git pull origin master
##已经初始化过，直接按照平时项目提交流程走。