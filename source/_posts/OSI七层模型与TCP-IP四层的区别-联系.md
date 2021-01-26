---
title: OSI七层模型与TCP/IP四层的区别/联系
copyright: true
mathjax: true
categories:
- 笔记
- 基础
- 网络
tags:
- OSI
- TCP/IP
abbrlink: 15594
date: 2021-01-26 10:05:11
---

## 什么是OSI

OSI（Open System Interconnect），即开放式系统互联。 一般都叫OSI参考模型，是ISO（国际标准化组织）组织在1985年研究的网络互连模型。ISO为了更好的使网络应用更为普及，推出了OSI参考模型。其含义就是推荐所有公司使用这个规范来控制网络。这样所有公司都有相同的规范，就能互联了。
OSI定义了网络互连的七层框架（物理层、数据链路层、网络层、传输层、会话层、表示层、应用层），即ISO开放互连系统参考模型。

![OSI七层模型及其解释](https://gitee.com/junpzx/blog-img/raw/master//img/20210126101946.png)

每一层实现各自的功能和协议，并完成与相邻层的接口通信。OSI的服务定义详细说明了各层所提供的服务。某一层的服务就是该层及其下各层的一种能力，它通过接口提供给更高一层。各层所提供的服务与这些服务是怎么实现的无关。

众所周知，OSI参考模型是学术上和法律上的国际标准，是完整的权威的网络参考模型。而TCP/IP参考模型是事实上的国际标准，即现实生活中被广泛使用的网络参考模型。



## OSI七层和TCP/IP四层的关系

OSI引入了服务、接口、协议、分层的概念，TCP/IP借鉴了OSI的这些概念建立TCP/IP模型。

OSI先有模型，后有协议，先有标准，后进行实践；而TCP/IP则相反，先有协议和应用再提出了模型，且是参照的OSI模型。

OSI是一种理论下的模型，而TCP/IP已被广泛使用，成为网络互联事实上的标准。



- TCP：transmission control protocol 传输控制协议
- UDP：user data protocol 用户数据报协议



| OSI七层网络模型         | TCP/IP四层概念模型 | 对应网络协议                             |
| ----------------------- | ------------------ | ---------------------------------------- |
| 应用层（Application）   | 应用层             | **HTTP**、TFTP, **FTP**, NFS, WAIS、SMTP |
| 表示层（Presentation）  | 应用层             | Telnet, Rlogin, SNMP, Gopher             |
| 会话层（Session）       | 应用层             | **SMTP**, **DNS**                        |
| 传输层（Transport）     | 传输层             | TCP, UDP                                 |
| 网络层（Network）       | 网络层             | IP, ICMP, ARP, RARP, AKP, UUCP           |
| 数据链路层（Data Link） | 数据链路层         | FDDI, Ethernet, Arpanet, PDN, SLIP, PPP  |
| 物理层（Physical）      | 数据链路层         | IEEE 802.1A, IEEE 802.2到IEEE 802.11     |



## OSI七层和TCP/IP的区别

- TCP/IP他是一个协议簇；而OSI（开放系统互联）则是一个模型，且TCP/IP的开发时间在OSI之前。
- TCP/IP是由一些交互性的模块做成的分层次的协议，其中每个模块提供特定的功能；OSi则指定了哪个功能是属于哪一层的。
- TCP/IP是五层结构，而OSI是七层结构。OSI的最高三层在TCP中用应用层表示。



**注：TCP/IP分层有两种分法，四层是把物理层和数据链路层合一起叫网络接口层， 五层是拆开了**

附一张经典图：

![](https://gitee.com/junpzx/blog-img/raw/master//img/20210126103224.jpg)