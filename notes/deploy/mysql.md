+++
title = "MySQL安装（必须）"
description = "MySQL安装，"
date = "2017-08-01"
draft = false
weight  = 10
+++
# 一，概览  

* 版本：MySQL5.7 
* 本文档只描述了单机的MySQL搭建，集群搭建文档可以联系研发团队。  

# 二，安装

### 2.1 直接安装

https://blog.csdn.net/h363659487/article/details/77508355

使用root用户新建 /etc/yum.repos.d/mysql57.repo 文件，内容如下:  

````
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
````

***执行安装命令***  

```
sudo yum makecache
sudo yum install mysql-community-server
```

***启动MySQL并用初始化密码登入***
```
systemctl start mysqld.service
mysql -uroot -p
```
# 三，docker安装MySQL
### 3.1 前置条件
* 已安装docker&docker-compose 

### 3.2 详细步骤
***创建docker-compose.yml文件***
```
vi docker-compose.yml
```
复制如下内容到文件中
```
version: "2"
services:
  mysql:
    container_name: mysql  # 容器名
    image: registry.saas.hand-china.com/tools/mysql:5.7.17 # 容器所使用的镜像，镜像形式为[username 或 url]/repository:tag，该镜像为公司搭设的docker仓库内的镜像
    ports:
      - "3306:3306" # [本机端口:容器内端口] 将本机端口与docker容器内部应用的端口映射，以提供外部对容器内应用的访问能力
    environment:
      MYSQL_ROOT_PASSWORD: root # 设置mysql密码
    volumes:
      - ./mysql/mysql_data:/var/lib/mysql # 将mysql中的数据文件映射到本机文件夹，":"前的为本机地址，后的为容器内地址
      - ./mysql/mysql_db.cnf:/etc/mysql/conf.d/mysql_db.cnf # 将mysql的配置文件映射到本机文件
```
***启动***
```
docker-compose -up -d
# 查看结果
docker ps
```


### 3.3 docker修改配置参考  

配置文件已经映射到宿主机上，可以直接修改宿主机的配置文件，下面方法是修改docker内的配置文件
```
# 进入容器
docker exec -i -t mysql /bin/bash
# 在容器中安装vim
apt-get update
apt-get install vim
# 修改配置
vim /etc/mysql/mysql.conf.d/mysqld.cnf
# 添加如下内容
lower_case_table_names=1
character_set_server=utf8
max_connections=500
```