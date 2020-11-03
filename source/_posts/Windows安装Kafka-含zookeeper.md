---
title: Windows安装Kafka(含zookeeper)
copyright: true
mathjax: true
date: 2020-11-03 10:13:02
categories:
- 安装教程
tags:
- Kafka
- zookeeper
- windows
---

windows环境下安装kafka和zookeeper教程

<!-- less -->



## 1.下载Kafka

- 打开 [下载地址](http://kafka.apache.org/downloads.html)

- 选择下图红框中的版本，Kafka包名组成： Scala版本 - Kafka自身版本

    ![kafka下载](https://gitee.com/junpzx/blog-img/raw/master//img/20201103102115.png)

    ​		

- 点击进去以后选择下载站点

- 等待下载完成后，进行解压，解压目录如下：

    ![kafka解压目录](https://gitee.com/junpzx/blog-img/raw/master//img/20201103102546.png)



## 2.配置Kafka

- 修改config目录中的zookeeper.properties文件中的`dataDir=/temp/zookeeper`，因为默认是linux路径，所以需要将其修改为windows路径，该配置在配置文件中的16行
- 修改config目录中的server.properties文件中的`log.dirs=/temp/kafka-logs`,将其修改成windows路径



## 3.启动Kafka、zk

- 进入到`kafka2.12-2.6.0` => `bin` => `windows`

- 在此目录中打开cmd界面

- 启动zk ，启动命令`zookeeper-server-start.bat ..\..\config\zookeeper.properties`

- 重新打开一个cmd界面

- 启动kafka，启动命令`kafka-server-start.bat ..\..\config\server.properties`

    **注意：如果启动时报`输入行太长，命令语法不正确`，可能是因为上述的配置文件没有修改，也可能是kafka文件夹的目录太深，可以直接放在D盘下试一下**



## 4.测试Kafka

- 创建一个主题:

    `kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test-topic`

- 查看创建的主题列表:

    `kafka-topics.bat --list --zookeeper localhost:2181`

![创建kafka主题](https://gitee.com/junpzx/blog-img/raw/master//img/20201103104143.png)

- 启动生产者:

    `kafka-console-producer.bat --broker-list localhost:9092 --topic test-topic`

    此时可以从控制台输入信息，待消费者启动后可接收到生产者发布的消息。

    ![启动kafka生产者](https://gitee.com/junpzx/blog-img/raw/master//img/20201103104913.png)

- 启动消费者：

    `kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test-topic --from-beginning`

- 在生产者中生成数据，查看消费者是否可以正常消费



**以上就是kafka的基本安装和使用**