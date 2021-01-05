---
title: Idea插件之Jrebel激活
copyright: true
mathjax: true
date: 2021-01-04 16:29:52
categories:
- 激活教程
tags:
- Idea
- JRebel
- XRebel
---

# Idea插件之Jrebel激活

## 前言

Rebel是一款JVM插件，它使得Java代码修改后不用重启系统，立即生效。IDEA上原生是不支持热部署的，一般更新了 Java 文件后要手动重启 Tomcat 服务器，才能生效，浪费时间浪费生命。

目前对于idea热部署最好的解决方案就是安装JRebel插件。




## 激活步骤

- 步骤1: 生成一个GUID: [在线生成GUID](https://www.guidgen.com/)

- 步骤2: 根据反向代理服务器地址拼接激活地址

  服务器地址： [jrebel.qekang.com/{GUID}](https://jrebel.qekang.com/{GUID})

  PS：如果失效刷新GUID替换就可以！

- 步骤3: 打开JRebel 激活面板，选择Connect to online licensing service .

  ![](https://gitee.com/junpzx/blog-img/raw/master//img/20210105135219.png)

激活成功的界面：

![激活成功](https://gitee.com/junpzx/blog-img/raw/master//img/20210105135233.png)



## 如何使用

安装激活完毕后，下面就可以愉快的玩耍了，激活后，你原本启动项目的右边会出现JRebel启动项目的图标，你就可以通过JRebel启动你的项目，这样你修改完Java代码后，而不再需要重启项目这样繁琐浪费时间的操作了。



## 相关提示

上面的激活使用了别人的代理地址，如果别人代理地址下线了，你的激活状态会不可用状态， 哈哈，如果靠谱点，有自己的服务器，可以自己搭建一个自己的反向代理服务。