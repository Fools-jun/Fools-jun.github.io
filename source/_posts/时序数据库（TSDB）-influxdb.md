---
title: 时序数据库（TSDB）- influxdb
copyright: true
mathjax: true
date: 2020-10-28 15:06:18
categories:
- 笔记
- 时序数据库
tags:
- influxDB
---

​		时序数据是基于时间的一系列的数据。在有时间的坐标中将这些数据点连成线，往过去看可以做成多纬度报表，揭示其趋势性、规律性、异常性;往未来看可以做大数据分析，机器学习，实现预测和预警。

​		时序数据库就是存放时序数据的数据库，并且需要支持时序数据的快速写入、持久化、多纬度的聚合查询等基本功能。

​		对比传统数据库仅仅记录了数据的当前值，时序数据库则记录了所有的历史数据。同时时序数据的查询也总是会带上时间作为过滤条件。

<!-- less -->





# 一、前言

## 1.1.什么是时序数据库

​		首先介绍一下什么是时序数据，时序数据是基于时间的一系列的数据。在有时间的坐标中将这些数据点连成线，往过去看可以做成多纬度报表，揭示其趋势性、规律性、异常性;往未来看可以做大数据分析，机器学习，实现预测和预警。

​		时序数据库就是存放时序数据的数据库，并且需要支持时序数据的快速写入、持久化、多纬度的聚合查询等基本功能。

​		对比传统数据库仅仅记录了数据的当前值，时序数据库则记录了所有的历史数据。同时时序数据的查询也总是会带上时间作为过滤条件。



## 1.2.时序数据库的特点

### 数据写入

- 写入平稳、持续、高并发高吞吐：时序数据的写入是比较平稳的，这点与应用数据不同，应用数据通常与应用的访问量成正比，而应用的访问量通常存在波峰波谷。时序数据的产生通常是以一个固定的时间频率产生，不会受其他因素的制约，其数据生成的速度是相对比较平稳的。
- 写多读少：时序数据上95%-99%的操作都是写操作，是典型的写多读少的数据。这与其数据特性相关，例如监控数据，你的监控项可能很多，但是你真正去读的可能比较少，通常只会关心几个特定的关键指标或者在特定的场景下才会去读数据。
- 实时写入最近生成的数据，无更新：时序数据的写入是实时的，且每次写入都是最近生成的数据，这与其数据生成的特点相关，因为其数据生成是随着时间推进的，而新生成的数据会实时的进行写入。数据写入无更新，在时间这个维度上，随着时间的推进，每次数据都是新数据，不会存在旧数据的更新，不过不排除人为的对数据做订正。



### 数据查询和分析

- 按时间范围读取：通常来说，你不会去关心某个特定点的数据，而是一段时间的数据。
- 最近的数据被读取的概率高
- 历史数据粗粒度查询的概率搞
- 多种精度查询
- 多维度分析



### 数据存储

- 数据量大：拿监控数据来举例，如果我们采集的监控数据的时间间隔是1s，那一个监控项每天会产生86400个数据点，若有10000个监控项，则一天就会产生864000000个数据点。在物联网场景下，这个数字会更大。整个数据的规模，是TB甚至是PB级的。

- 冷热分明：时序数据有非常典型的冷热特征，越是历史的数据，被查询和分析的概率越低。

- 具有时效性：时序数据具有时效性，数据通常会有一个保存周期，超过这个保存周期的数据可以认为是失效的，可以被回收。一方面是因为越是历史的数据，可利用的价值越低；另一方面是为了节省存储成本，低价值的数据可以被清理。

- 多精度数据存储：在查询的特点里提到时序数据出于存储成本和查询效率的考虑，会需要一个多精度的查询，同样也需要一个多精度数据的存储。



## 1.3.时序数据库适合的场景

​		所有有时序数据产生，并且需要展现其历史趋势、周期规律、异常性的，进一步对未来做出预测分析的，都是时序数据库适合的场景。



## 1.4.时序数据库的数据模型

时间序列数据可以分成两部分：

- 序列 ：就是标识符（维度），主要的目的是方便进行搜索和筛选
- 数据点：时间戳和数值构成的数组
    - 行存：一个数组包含多个点，如 [{t: 2017-09-03-21:24:44, v: 0.1002}, {t: 2017-09-03-21:24:45, v: 0.1012}]
    - 列存：两个数组，一个存时间戳，一个存数值，如[ 2017-09-03-21:24:44, 2017-09-03-21:24:45], [0.1002,  0.1012]

 **一般情况下：列存能有更好的压缩率和查询性能**



## 1.5.时序数据库的基本概念

**metric**：度量，相当于关系型数据库中的table。

**data point:** 数据点，相当于关系型数据库中的row。

**timestamp**：时间戳，代表数据点产生的时间。

**field**: 度量下的不同字段。比如位置这个度量具有经度和纬度两个field。一般情况下存放的是会随着时间戳的变化而变化的数据。

**tag**: 标签，或者附加信息。一般存放的是并不随着时间戳变化的属性信息。timestamp加上所有的tags可以认为是table的primary key。





# 二、influxDB简介

influxDB在Github上的地址：https://github.com/influxdata/influxdb

## 2.1.influxDB是什么

​		**InfluxDB**是一个由[InfluxData](https://www.influxdata.com/)开发的开源时序型数据库。它由Go写成，着力于高性能地查询与存储时序型数据。InfluxDB被广泛应用于存储系统的监控数据，IoT(物联网)行业的实时数据等场景。



## 2.2.influxDB的特点

- InfluxDB在技术实现上充分利用了Go语言的特性，无需任何外部依赖即可独立部署。
- InfluxDB提供了一个类似于SQL的查询语言并且一系列内置函数方便用户进行数据查询。
- InfluxDB存储的数据从逻辑上由 **Measurement**, **tag组**以及 **field组**以及一个**时间戳**组成的：
    - Measurement： 由一个字符串表示该条记录对应的含义。比如它可以是监控数据"cpu_load"，也可以是测量数据"average_temperature"
    - tag组： 由一组键值对组成，表示的是该条记录的一系列属性信息。同样的measurement数据所拥有的tag组不一定相同，它是无模式的(Schema-free)。tag信息是默认被索引的。
    - field组：也是由一组键值对组成，表示的是该条记录具体的value信息(有名称)。field组中可定义的value类型包括：64位整型，64位浮点型，字符串以及布尔型。Field信息是无法被索引的。
    - 时间戳：就是该条记录的时间属性。如果插入数据时没有明确指定时间戳，则默认存储在数据库中的时间戳则为该条记录的入库时间。
- InfluxDB支持基于HTTP的数据插入与查询。同时也接受直接基于TCP或UDP协议的连接。
- InfluxDB允许用户定义数据保存策略(Retention Policies)来实现对存储超过指定时间的数据进行删除或者降采样。



## 2.3.influxDB的关键概念

### 基本概念

InfluxDB 和传统数据库（如：MySQL）的一些区别

|  InfluxDB   | 传统数据库中的概念 |
| :---------: | :----------------: |
|  database   |       数据库       |
| measurement |    数据库中的表    |
|   points    |  表里面的一行数据  |



### 特有概念

1. tag–标签，在 InfluxDB 中，tag 是一个非常重要的部分，表名+tag 一起作为数据库的索引，是“key-value”的形式

2. field–数据，field 主要是用来存放数据的部分，也是“key-value”的形式

3. timestamp–时间戳，作为时序型数据库，时间戳是 InfluxDB 中最重要的部分，在插入数据时可以自己指定也可留空让系统指定

    **说明：在插入新数据时，tag、field 和 timestamp 之间用空格分隔**

4. series–序列，所有在数据库中的数据，都需要通过图表来展示，而这个 series 表示这个表里面的数据，可以在图表上画成几条线。具体可以通过 `SHOW SERIES FROM "表名"` 进行查询

5. Retention policy–数据保留策略，可以定义数据保留的时长，每个数据库可以有多个数据保留策略，但只能有一个默认策略

6. Point–点，表示每个表里某个时刻的某个条件下的一个 field 的数据，因为体现在图表上就是一个点，于是将其称为 point。Point 由时间戳（time）、数据（field）、标签（tags）组成

| Point 属性 |                  传统数据库中的概念                   |
| :--------: | :---------------------------------------------------: |
|    time    |   每个数据记录时间，是数据库中的主索引 (会自动生成)   |
|   fields   | 表中的列（没有索引的属性）也就是记录的值：温度， 湿度 |
|    tags    |                表中的索引：地区，海拔                 |



## 2.4.influxDB端口

- 8083：Web admin 管理服务的端口, [http://localhost:8083](http://localhost:8083/)
- 8086：HTTP API 的端口
- 8088：集群端口



# 三、influxDB安装

## 3.1.Linux上安装

```
wget https://dl.influxdata.com/influxdb/releases/influxdb-1.8.3.x86_64.rpm

sudo yum localinstall influxdb-1.8.3.x86_64.rpm
```



## 3.2.Windows上安装

[下载地址](https://dl.influxdata.com/influxdb/releases/influxdb-1.8.3_windows_amd64.zip):https://dl.influxdata.com/influxdb/releases/influxdb-1.8.3_windows_amd64.zip



## 3.3.docker上安装

```
docker pull influxdb
```



# 四、influxDB基本使用（以Windows为例）

## 4.1.启动

```
# 在cmd中进入到influxDB目录输入以下命令启动influxDB
influxd.exe -config influxdb.conf

# 启动客户端，如果没有修改过conf文件，那么则不用在后面指定端口
influx.exe -port 8081
```



## 4.2.用户

```mysql
#显示所有用户
show users

#创建用户
create user "username" with password 'password'

#创建管理员权限用户
create user "username" with password 'password' with all privileges

#修改用户密码
set password for username = "password"

#删除用户
drop user "username"
```



## 4.3.权限

```mysql
#查看权限
show grants for username

#赋予管理员权限
grant all privileges to username

#取消权限
revoke all on mydb from username
```



## 4.4.登录账号

**认证策略需要在配置文件中打开[http]下的auth-enabled = true**

```mysql
auth
然后输入账号密码
```



## 4.5.数据库操作

```mysql
#查看所有数据库
show databases

#创建数据库
create database "testDB"

#使用数据库
use testDB
```



## 4.6.表

```mysql
#显示所有表
show measurements

#创建表
insert test,host=127.0.0.1,monitor_name=test count=1

#删除表
drop measurement "measurement_name"
```







## 4.7插入数据

```mysql
insert weather,altitude=5000,area=北 humidity=3,temperature=23 1599630284710054000
```

- measurement(表名): weather
- tags（类似索引）: altitude、area。是String类型
- fields: temperature、humidity。是字符串、数字类型
- influx默认时间是纳秒（ns），即19位时间戳



## 4.16.查看表名

```mysql
show measurements
```



## 4.17.查看数据

```mysql
# 操作与mysql类似
select * from weather where time >=1599630284710ms

select * from weather where time >= '2018-11-23 14:30:39' and time <= '2018-11-23 14:32:32' tz('Asia/Shanghai')

# 涉及时间查询使用时间戳，避免时区问题
```



## 4.18.查看策略

```mysql
show retention policies on 'weather'
```



## 4.19.创建策略

```mysql
create retention policy "rp_test" on "testDB" duration 30d replication 1 default
```

- rp_test: 策略名

- testDB：数据库名

- 30d：数据保存时间，30天之前的数据将被删除

- replication 1: 副本个数

- default: 设为默认策略

| **duration**          | **shardGroupDuration** |
| --------------------- | ---------------------- |
| <2days                | 1hour                  |
| >=2days and <=6months | 1day                   |
| >6months              | 7days                  |



## 4.20.修改策略

```mysql
alter retention policy "rp_test" on "testDB" duration 1d replication 1
```



## 4.21.删除策略

```mysql
drop retention policy " rp_test" on " testDB"
```



## 4.22.查询连续查询

```mysql 
SHOW CONTINUOUS QUERIES 
```

- 这条命令得在命令行下输入，在web管理界面不能显示。
- QUERIES需要大写



## 4.23.创建新的Continuous Queries

```mysql
create continuous query cq_30m on testDB begin select mean(temperature) into weather30m from weather group by time(30m) end
```

- cq_30m：连续查询的名字

- testDB：具体的数据库名

- mean(temperature): 算平均温度

- weather： 当前表名

- weather30m： 存新数据的表名

- 30m：时间间隔为30分钟



## 4.24.删除Continuous Queries

```mysql
DROP CONTINUOUS QUERY <cq_name> ON <database_name>
```



 

