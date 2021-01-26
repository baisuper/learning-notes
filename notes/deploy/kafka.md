+++
title = "kafka安装"
description = "kafka安装"
date = "2017-08-01"
draft = false
weight = 3
+++
# 一，概览

# 二，Centos7 部署

### 2.1 安装Kafka

***下载***  
从官网下载kafka安装包，本文档下载的版本为最新2.0.0版本.  
官网地址：http://kafka.apache.org
```
wget http://mirrors.shu.edu.cn/apache/kafka/2.0.0/kafka_2.11-2.0.0.tgz
```
***解压缩***

```
tar -zxvf kafka_2.11-2.0.0.tgz
```

***修改配置文件***  
在解压后的目录下执行
```
vi config/server.properties
```
需要关注的参数：  
```
#集群部署时，每个节点的id值不同
broker.id=0
...
#zookeeper地址
zookeeper.connect=localhost:2181
...
#log地址,kafka的日志如果不控制可能会变的很大
log.dirs=/kafka/logs
...
#日志保存时间 (hours|minutes)，默认为7天（168小时）。超过这个时间会根据policy处理数据。bytes和minutes无论哪个先达到都会触发。
#建议修改日志相关参数控制日志大小
log.retention.hours=1
# 日志数据存储的最大字节数。超过这个时间会根据policy处理数据。
#log.retention.bytes=1073741824

```

### 2.2 启动

***启动zookeeper***  
在解压的kafka目录下执行  

```
#daemon表示后台运行
sh bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
```

***启动kafka***
```
#daemon表示后台运行
sh bin/kafka-server-start.sh -daemon config/server.properties
```

# 三，运维