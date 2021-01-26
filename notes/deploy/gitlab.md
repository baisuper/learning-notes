+++
title = "GitLab安装"
description = "GitLab安装"
date = "2018-11-23"
draft = false
weight = 13
+++
# 一，概览

# 二，Docker部署Gitlab

### 2.1 前置安装

- 安装docker
- 安装docker-compose

### 2.2 拉取镜像

```bash
sudo docker pull gitlab/gitlab-ce:latest
```

***离线获取镜像***

如果处于离线环境，可在能访问外网的机器上执行如下命令拉取镜像，打包

```bash
docker pull gitlab
docker save -o gitlab.tar gitlab
```

将打包文件上传到服务器之后，在服务器上执行如下命令加载镜像

```bash
docker load -i gitlab.tar
```

### 2.3 docker-compose安装、启动容器

***创建docker-compose文件***

创建docker.compose.yml文件，内容如下

```yml
version: "2"
services:
  gitlab:
    container_name: gitlab
    image: gitlab/gitlab-ce:latest
    restart: always
    ports:
      - "8080:80"
      - "443:443"
      - "22:22"
    volumes:
      - "./gitlab/config:/etc/gitlab"
      - "./gitlab/logs:/var/log/gitlab"
      - "./gitlab/data:/var/opt/gitlab"
```

容器端口如下，可根据情况映射到本地端口

容器端口 | 本地端口 | 说明 
----|------|-----
80 | 8080 | http访问段口
443 | 443 | https访问端口
22 | 22 | ssh访问端口

数据路径映射如下，可根据情况映射到本地路径

容器路径 | 本地路径 | 说明 
----|------|-----
/etc/gitlab | ./gitlab/config | 存储配置文件
/var/log/gitlab | ./gitlab/logs | 存储日志文件
/var/opt/gitlab | ./gitlab/data | 存储应用数据


***创建并启动容器***

```bash
docker-compose up -d
```

***查看启动日志***

```bash
docker logs -f gitlab
```

***修改git访问地址***

在gitlab创建项目后，项目地址的显示会不正常，需要在容器中修改如下配置：

```bash
docker exec -it gitlab /bin/bash
vim /etc/gitlab/gitlab.rb
```

将external_url修改为gitlab的访问地址即可，如

<img src="/img/docs/deploy/jar/images/gitlab1.jpg" width="60%" style="min-width: 400px">

修改完成后，更新配置并重启

```bash
gitlab-ctl reconfigure
gitlab-ctl restart
```

### 2.4 访问并设置管理员密码

访问gitlab地址，如 http://localhost:8080 ，首次访问出现如下页面

<img src="/img/docs/deploy/jar/images/gitlab2.jpg" width="80%" style="min-width: 400px">

输入密码之后，可使用管理员账号 `root` 和刚刚设置的密码登录，登录之后如图

<img src="/img/docs/deploy/jar/images/gitlab3.jpg" width="80%" style="min-width: 400px">