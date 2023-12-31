---
layout: page

title: MySQL线上生产环境故障及解决方案
category: mysql
categoryStr: MySQL
tags:
keywords:
description:
---


## MySQL数据库cpu飙升的话，要怎么处理呢？

排查过程：

（1）使用top 命令观察，确定是mysqld导致还是其他原因。
（2）如果是mysqld导致的，show processlist，查看session情况，确定是不是有消耗资源的sql在运行。
（3）找出消耗高的 sql，适用explain看看执行计划是否准确， 索引是否缺失，数据量是否太大。

处理：

（1）kill 掉这些线程(同时观察 cpu 使用率是否下降)， 
（2）进行相应的调整(比如说加索引、改 sql、改内存参数) 
（3）重新跑这些 SQL。

其他情况：

也有可能是每个 sql 消耗资源并不多，但是突然之间，有大量的 session 连进来导致 cpu 飙升，
这种情况就需要跟应用一起来分析为何连接数会激增，再做出相应的调整， 比如说限制连接数等

## MYSQL的主从延迟，你怎么解决？

主从复制分了五个步骤进行：（图片来源于网络）


步骤一：主库的更新事件(update、insert、delete)被写到binlog  
步骤二：从库发起连接，连接到主库。  
步骤三：此时主库创建一个binlog dump thread，把binlog的内容发送到从库。  
步骤四：从库启动之后，创建一个I/O线程，读取主库传过来的binlog内容并写入到relay log  
步骤五：还会创建一个SQL线程，从relay log里面读取内容，从Exec_Master_Log_Pos位置开始执行读取到的更新事件，将更新内容写入到slave的db  

主从同步延迟的原因

一个服务器开放Ｎ个链接给客户端来连接的，这样有会有大并发的更新操作, 但是从服务器的里面读取binlog的线程仅有一个，
当某个SQL在从服务器上执行的时间稍长 或者由于某个SQL要进行锁表就会导致，主服务器的SQL大量积压，未被同步到从服务器里。
这就导致了主从不一致， 也就是主从延迟。

主从同步延迟的解决办法

主服务器要负责更新操作，对安全性的要求比从服务器要高，所以有些设置参数可以修改，比如sync_binlog=1，innodb_flush_log_at_trx_commit = 1 之类的设置等。
选择更好的硬件设备作为slave。
把一台从服务器当度作为备份使用， 而不提供查询， 那边他的负载下来了， 执行relay log 里面的SQL效率自然就高了。
增加从服务器喽，这个目的还是分散读的压力，从而降低服务器负载。

## 如果让你做分库与分表的设计，简单说说你会怎么做？

### 分库分表方案:

水平分库：以字段为依据，按照一定策略（hash、range等），将一个库中的数据拆分到多个库中。
水平分表：以字段为依据，按照一定策略（hash、range等），将一个表中的数据拆分到多个表中。
垂直分库：以表为依据，按照业务归属不同，将不同的表拆分到不同的库中。
垂直分表：以字段为依据，按照字段的活跃性，将表中字段拆到不同的表（主表和扩展表）中。
常用的分库分表中间件：

sharding-jdbc
Mycat
### 分库分表可能遇到的问题

事务问题：需要用分布式事务啦
跨节点Join的问题：解决这一问题可以分两次查询实现
跨节点的count,order by,group by以及聚合函数问题：分别在各个节点上得到结果后在应用程序端进行合并。
数据迁移，容量规划，扩容等问题
ID问题：数据库被切分后，不能再依赖数据库自身的主键生成机制啦，最简单可以考虑UUID
跨分片的排序分页问题