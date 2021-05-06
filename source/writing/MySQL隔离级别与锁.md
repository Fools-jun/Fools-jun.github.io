---
title: MySQL隔离级别与锁
copyright: true
mathjax: true
date: 2021-04-28 09:49:17
categories:
tags:
---

## 前言

>MySQL是目前世界上最流行的数据库，InnoDB是MySQL最流行的存储引擎，它在大数据量高并发的业务场景下，有着非常良好的性能表现，之所以如此，是和InnoDB的**锁机制**相关。
>
>我们都知道事务的几种性质，数据库为了维护这些性质，尤其是一致性和隔离性，一般使用加锁这种方式。同时数据库又是一个高并发的应用，同一时间会有大量的并发访问，如果加锁过度，会极大的降低并发处理能力。所以对于加锁的处理，可以说就是数据库对于事务处理的精髓所在。



## 一次封锁or两段锁？

因为有大量的并发访问，为了预防死锁，一般应用中推荐使用一次封锁法，就是在方法的开始阶段，已经预先知道会使用到哪些数据，然后全部锁住，在方法运行之后，再全部解锁。这种方式可以有效的避免循环死锁，但在数据库中却不适用，因为在事务开始阶段，数据库并不知道会用到哪些数据。

数据库遵循的是两段锁协议，将事务分为两个阶段，加锁阶段和解锁阶段（所以叫两段锁）

- 加锁阶段：在该阶段可以进行加锁操作。在对任何数据进行读操作之前要申请并获得S锁（共享锁，其它事务可以继续加共享锁，但不能加排它锁），在进行写操作之前要申请并获得X锁（排它锁，其它事务不能再获得任何锁）。加锁不成功，则事务进入等待状态，直到加锁成功才继续执行。
- 解锁阶段：当事务释放了一个封锁以后，事务进入解锁阶段，在该阶段只能进行解锁操作，不能再进行加锁操作。

| 事务                  | 加锁/解锁处理                                      |
| --------------------- | -------------------------------------------------- |
| begin；               |                                                    |
| insert into test .... | 加insert对应的锁                                   |
| update test set ...   | 加update对应的锁                                   |
| delete from test .... | 加delete对应的锁                                   |
| commit;               | 事务提交时，同时释放insert、update、delete对应的锁 |

这种方式虽然无法避免死锁，但是两段锁协议可以保证事务的并发调度是串行化（串行化很重要，尤其是数据恢复和备份的时候）的。



## 事务中的加锁方式

### 事务的隔离级别

在数据库操作中，为了有效保证并发读取数据的正确性，提出的数据隔离级别。我们的数据库锁，也是为了构建这些隔离级别存在的。

| 隔离级别                     | 脏读（Dirty Read） | 不可重复读（NonRepeatable Read） | 幻读（Phantom Read） |
| ---------------------------- | ------------------ | -------------------------------- | -------------------- |
| 未提交度（Read Uncommitted） | 可能               | 可能                             | 可能                 |
| 已提交读（Read Committed）   | 不可能             | 可能                             | 可能                 |
| 可重复读（Repeatable Read）  | 不可能             | 不可能                           | 可能                 |
| 可串行化（Serializable）     | 不可能             | 不可能                           | 不可能               |

- 未提交读（Read Uncommitted）：允许脏读。也就是可能读取到其他会话中未提交事务修改的数据。
- 提交读（Read Committed）：只能读取到已经提交的数据。Oracle等多数数据库默认都是该级别（不重复读）。
- 可重复读（Repeated Read）：可重复读。在同一个事务内的查询都是事务开始时刻一致的，InnoDB默认级别。在SQL标准中，该隔离级别消除了不可重复读，但是还存在幻象读。
- 串行化（Serializable）：完全串行化的读，每次读都需要获得表级共享锁，读写相互都会阻塞。

Read Uncommitted这种级别，数据库一般都不会用，而且任何操作都不会加锁，这里都不多做赘述了。



### MySQL中锁的种类

MySQL中锁的种类很多，有常见的表锁和行锁，也有新加入的Metadata Lock等等，表锁是对一整张表加锁，虽然可分为读锁和写锁，但毕竟是锁住整张表，会导致并发能力下降，一般是做ddl处理时使用。

行锁则是锁住数据行，这种加锁方式比较复杂，但是由于只锁住有限的数据，对于其它数据不加限制，所以并发能力强，MySQL一般都是用行锁来处理并发事务。这是主要讨论的也就是行锁。

#### Read Committed(读取提交内容)

在RC级别中，数据的读取都是不加锁的，但是数据的写入、修改和删除时需要加锁的。效果如下

````mysql
MySQL> show create table class_teacher \G\
Table: class_teacher
Create Table: CREATE TABLE `class_teacher` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `class_name` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `teacher_id` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `idx_teacher_id` (`teacher_id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci

1 row in set (0.02 sec)

MySQL> select * from class_teacher;
+----+--------------+------------+
| id | class_name   | teacher_id |
+----+--------------+------------+
|  1 | 初三一班     |          1 |
|  3 | 初二一班     |          2 |
|  4 | 初二二班     |          2 |
+----+--------------+------------+
````

由于MySQL的innoDB默认是使用的RR级别，所以我们先要将该session开启成RC级别，并且设置binlog的模式。

```mysql
SET session transaction isolation level read committed;
SET SESSION binlog_format = 'ROW';（或者是MIXED）
```

| 事务A                                                        | 事务B                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| begin;                                                       | begin;                                                       |
| update class_teacher set class_name = '初三二班' where teacher_id= 1; | update class_teacher set class_name ='初三三班' where teacher_id = 1; |
| commit;                                                      |                                                              |

为了防止并发过程中的修改冲突，事务A中MySQL给teacher_id = 1的数据行加锁，并且一直不commit（释放锁），那么事务B也就一直拿不到该行锁，wait直到超时。

这是我们需要注意到，teacher_id是有索引的，如果是没有索引的class_name呢?`update class_teacher set teacher_id = 3 where class_name = '初三一班';` 

