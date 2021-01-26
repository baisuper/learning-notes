+++
title = "docker"
description = "docker"
date = "2017-08-01"
draft = false
weight = 2
+++
# 一，概览

# 二，安装

***前置安装***  

- git工具     

### 2.1 docker安装  
***文档装机环境：centos7***  

***添加yum源文件***  
使用root用户新建 /etc/yum.repos.d/docker.repo 文件，内容如下:  

```
[dockerrepo]
name=Docker Repository
baseurl=https://mirrors.tuna.tsinghua.edu.cn/docker/yum/repo/centos7
enabled=1
gpgcheck=1
gpgkey=https://mirrors.tuna.tsinghua.edu.cn/docker/yum/gpg
```
***执行安装命令***  

```
sudo yum makecache
sudo yum install docker-engine
```
***启动***  

```
systemctl start docker.service
```

#### 2.2 安装docker-compose  
***选在执行命令***  

```
sudo curl -L https://github.com/docker/compose/releases/download/1.8.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose 
```

