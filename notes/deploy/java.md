+++
title = "JDK安装（必须）"
description = "JDK安装，合同系统要求jdk版本在1.8及以上"
date = "2017-08-01"
draft = false
weight = 2
+++
# 一，概览

# 二，Centos7 部署

### 2.1 yum安装java

***查看yum列表***

```
yum list java*
```
***安装jdk***

```
sudo yum install java-1.8.0-openjdk.x86_64
sudo yum install java-1.8.0-openjdk-devel
```

***查看安装是否成功***  

```
java -version
```
