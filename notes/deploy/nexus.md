x+++
title = "Nexus2安装"
description = "Nexus2安装"
date = "2017-08-01"
draft = false
weight = 12
+++
# 一，概览

# 二，离线部署

### 2.1 离线部署

***安装环境***  

***下载安装程序***  
- nexus下载界面，本文档使用的是nexus2: https://www.sonatype.com/download-oss-sonatype  
- nexus2 下载地址：https://download.sonatype.com/nexus/oss/nexus-2.14.9-01-bundle.tar.gz  
- nexus3 下载界面：https://help.sonatype.com/repomanager3/download/download-archives---repository-manager-3


***安装***  
```
#解压 
tar -zxvf latest-unix.tar.gz
cd nexus-3.14.0-04/bin
# 第一次运行run,以后运行start,start是后台运行
# 以root用户启动需要修改环境变量
export RUN_AS_USER=root
./nexus console
#./nexus start 
```
访问地址ip:8081/nexus   
默认账户密码：admin/admin123  

***迁移jar包***  
由于是内网的nexus，需要迁移的我们需要的jar到服务器。  
jar包的上传有两种方式：  
- 在nexus界面上传单个jar包  

- 直接将jar上传到服务器目录下，在nexus界面更新索引  
将本地的




***查看安装是否成功***  

```
java -version
```
