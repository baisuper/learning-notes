+++
title = "redis安装"
description = "redis安装"
date = "2017-08-01"
draft = false
weight = 4
+++
# 一，概览

# 二，安装

### 2.1 yum安装redis

***装机环境***
操作系统：Centos7

***查看yum列表***  

```
yum list redis*
```
***安装redis***  

```
yum install redis
```

***如果提示No package redis available. 则运行如下命令***
```
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -ivh epel-release-6-8.noarch.rpm
```

***修改配置***  
修改配置文件 /etc/redis.conf
主要关注以下参数
```
#注释或配置0.0.0.0代表允许所有外网链接
#配置127.0.0.1代表只允许本机链接
#配置本机网卡ip，表示只接受对应网卡过来的链接
bind 127.0.0.1

...
#保护模式是否启动:
#开发环境建议关闭，生产环境开启并配置密码
#protected-mode no

```

***启动reids***  

```
service redis start
```

### 2.2 离线安装redis 

***装机环境***  
操作系统：Red HAT 8.8  

***下载安装程序***  
- redis安装包，本文档下载的版本是5.0.0：http://redis.io/download  
- gcc 安装包：  
- tcl下载地址：http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz  

***安装gcc***  
由于本环境包含gcc yum资源，所以直接使用yum安装，离线安装后面再补充
```
sudo yum install gcc-c++
```

***安装tcl***
```
sudo tar xzvf tcl8.6.1-src.tar.gz  -C /usr/local/
cd  /usr/local/tcl8.6.1/unix/
sudo ./configure
sudo make
sudo make install
```

***安装redis***  
```
# 解压
tar -zxvf redis-5.0.0.tar.gz
# 进入解压目录执行
make
# 继续执行安装
make test
```

***配置reids***  
新建三个目录：tmp、run、logs  
```
mkdir tmp run logs
```

修改redis配置文件
```
# 进入解压目录执行
# 复制redis配置文件并重命名
cp redis.conf redis_6379.conf
vi redis_6379.conf
```

关注如下参数：  
```
bind 0.0.0.0
#非保护模式
protected-mode no
#是否以后台daemon方式运行
daemonize yes
#双引号必备，pid文件位置
pidfile "/home/jiantou/redis/run/redis_6379.pid"
#监听的端口号
port 6379
# 日志文件位置
logfile "/home/jiantou/redis/logs/redis_6379.log"
#数据快照的保存目录（这个是目录）
dir "/home/jiantou/redis/tmp"
```

***启动脚本***  
复制utils/redis_init_script到用户home下  
```
cp redis_init_script $HOME/redis.sh
vi $HOME/redis.sh
```
主要关注如下标注的内容，如果没有则修改为如下形式：
![image](/img/docs/deploy/jar/images/redis2.png)

***服务注册***  
```
sudo cp $HOME/redis.sh /etc/rc.d/init.d/redis
sudo chmod +x /etc/rc.d/init.d/redis
sudo chkconfig --add redis
sudo chkconfig --level 345 redis on
```

***服务启停***  
```
service redis start
service redis stop

```
# 三，运维