---
title: 科学上网（Vultr）
copyright: true
mathjax: true
date: 2020-10-26 15:36:52
categories:
tags:
---

使用vultr科学上网

<!--less-->

# 一.准备

## 注册账号

<a href="https://www.vultr.com/?ref=8321130">vultr注册地址</a>

![](https://gitee.com/junpzx/blog-img/raw/master//img/20201026175337.png)



![](https://gitee.com/junpzx/blog-img/raw/master//img/20201026175821.png)



![](https://gitee.com/junpzx/blog-img/raw/master//img/20201026175830.png)

## 充值

![](https://gitee.com/junpzx/blog-img/raw/master//img/20201026180004.png)

# 二.购买服务器

1. 选择左边栏的Products
2. 服务器类型选择Cloud Compute
3. 服务器地址选择日本（日本的服务器节点网速还行，不容易被墙，也可以选择美国的）
4. 系统选择CentOS7
5. 收费就选择5$每月的（如果也就平常查找资料，看一下新闻和视频的话，选择5刀的够用了，如果使用一段时间后，感觉不够用，也可以升级的）
6. 下面的配置都不用管，直接点击Deploy Now
7. 等待安装完成

![1,2,3步骤](https://gitee.com/junpzx/blog-img/raw/master//img/20201026154313.png)





![4,5步骤](https://gitee.com/junpzx/blog-img/raw/master//img/20201026154320.png)







# 三.配置服务器

1. 首先下载安装xshell（xshell，一个远程连接命令行工具）
2. 查看刚刚创建完成的服务器

![查看所有服务器](https://gitee.com/junpzx/blog-img/raw/master//img/20201026160433.png)

3. 查看具体服务器的ip等信息

![查看服务器信息](https://gitee.com/junpzx/blog-img/raw/master//img/20201026160713.png)

4. 连接xshell
    1. 点击左上角的的文件->新建
    
    2. 名称随便输入（尽量设置为该服务的ip）
    
    3. 协议选择默认的SSH
    
    4. 主机输入服务器的ip地址（也就是上图中的IP Address）
    
    5. 端口不用改，默认为22
    
    6. 点击左侧的`用户身份验证`
    
        ![](https://gitee.com/junpzx/blog-img/raw/master//img/20201026162251.png)
        
    7. 设置登录账号和密码（用户名：服务器详情图片中的Username，密码为服务器详情图片中的Password）
    
    ​	![xshell配置2](https://gitee.com/junpzx/blog-img/raw/master//img/20201026162359.png)
    
    ​	
    
    8. 配置完后，点击连接
    
    ![连接成功](https://gitee.com/junpzx/blog-img/raw/master//img/20201026163106.png)



5. 配置服务器ssr

    - 接下来根据大神们建好的轮子来进行梯子搭建

    - 依次输入以下命令

        - `wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh`
        - `chmod +x shadowsocks.sh`
        - `./shadowsocks.sh 2>&1 | tee shadowsocks.log`

    - 然后根据提示输入信息

        - Default password: teddysun.com：梯子密码
        - Default port: 16846：梯子连接端口
        - Please select stream cipher for shadowsocks-python: 选择加密方式

        ![梯子设置](https://gitee.com/junpzx/blog-img/raw/master//img/20201026164840.png)

    - 然后按下回车，等待设置完成

    - 设置完成后，会出现以下图

        ![设置完成](https://gitee.com/junpzx/blog-img/raw/master//img/20201026165302.png)



# 四.使用梯子

- 下载连接客户端

    - 下载地址：https://github.com/shadowsocks/shadowsocks-windows/releases

    ![下载客户端](https://gitee.com/junpzx/blog-img/raw/master//img/20201026165552.png)

- 解压下载好的zip文件

- 运行根目录中的.exe文件

    ![客户端](https://gitee.com/junpzx/blog-img/raw/master//img/20201026170139.png)

- 对客户端进行配置

    ![](https://gitee.com/junpzx/blog-img/raw/master//img/20201026173236.png)

    ​										

    ![](https://gitee.com/junpzx/blog-img/raw/master//img/20201026173537.png)

    ![](https://gitee.com/junpzx/blog-img/raw/master//img/20201026173633.png)

- 测试是否可以使用

    ![测试](https://gitee.com/junpzx/blog-img/raw/master//img/20201026173747.png)

- ok，配置成功