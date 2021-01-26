v+++
title = "maven安装"
description = "maven安装"
date = "2017-08-01"
draft = false
weight = 9
+++
# 一，概览

# 二，安装部署

### 2.1 离线安装  
***版本：3.6.0***  
版本：3.6.0  
操作系统：Red HAT 8.8
***下载安装包***  
- 下载地址：http://mirror.bit.edu.cn/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz  
- 下载界面,如果下载地址无效则从这里下载：http://maven.apache.org/download.cgi  
***安装配置***  

```
# 创建目录，下载安装包
mkdir maven 
cd maven
wget http://mirror.bit.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
tar -zxvf apache-maven-3.6.3-bin.tar.gz
```
配置环境变量  
```
vi /etc/profile
# 添加如下内容到最后，注意修改路径
export MAVEN_HOME=/root/maven/apache-maven-3.6.3
export PATH=${PATH}:${MAVEN_HOME}/bin
# 生效新的配置文件
source /etc/profile
```


#### 2.2 yum安装
````
wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
yum -y install apache-maven
````

#### 三，运维
***查看安装是否成功***  

```
mvn -v
```

***拷贝maven仓库***
```shell
scp -r repository cmsapp_uat@10.64.227.67:/home/cmsapp_uat/.m2
```
