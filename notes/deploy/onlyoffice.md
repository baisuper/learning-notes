+++
title = "Onlyoffice安装"
description = "Onlyoffice安装"
date = "2018-11-23"
weight = 14

draft = false
+++
# 一，概览

# 二，Docker部署Onlyoffice

### 2.1 前置安装

- 安装docker
- 安装docker-compose

### 2.2 加载镜像

将ubuntu-onlyoffice.tar上传到服务器之后，在服务器上执行如下命令加载镜像

```bash
docker load -i ubuntu-onlyoffice.tar
```

### 2.3 docker-compose安装、启动容器

***创建docker-compose文件***

创建docker.compose.yml文件，内容如下

```
version: "2"
services:
  onlyoffice:
    container_name: onlyoffice
    image: onlyoffice/ubuntu:0.0.1
    restart: always
    ports:
    - "8000:8000"
```

容器端口如下，可根据情况映射到本地端口

容器端口 | 本地端口 | 说明 
----|------|-----
8000 | 8000 | onlyoffice服务端口

***创建并启动容器***

```bash
docker-compose up -d
```

***查看启动日志***

```bash
docker logs -f gitlab
```

### 2.4 访问

访问onlyoffice地址，如 http://localhost:8000/welcome ，出现如下页面即搭建成功

<img src="/img/docs/deploy/jar/images/onlyoffice.jpg" width="80%" style="min-width: 400px">

