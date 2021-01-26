+++
title = "nginx安装"
description = "nginx安装"
date = "2017-08-01"
draft = false
weight = 8
+++
# 一，概览

# 二，安装部署

### 2.1 编译安装

***下载安装文件***  
- nginx安装包，本文档下载版本为1.14.0：https://nginx.org/en/download.html  
- pcre安装包：http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz

***安装gcc等依赖***  
```
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```
***安装pcre***  
```
tar -xzf pcre-8.35.tar.gz
# 进入解压目录运行
sudo ./configure
sudo make 
sudo make install
```

***安装nginx***  
```
tar -zxvf nginx-1.14.0.tar.gz
# 进入解压目录运行
# prefix：安装目录
# --with-pcre= pcre地址
sudo ./configure --prefix=/hap/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/hap/pcre-8.35
sudo make 
sudo make install
```

***配置nginx***  
进入nginx安装目录,运行命令配置nginx  
```
vi conf/nginx.conf
```

***启停服务***  
进入nginx安装目录下的sbin目录
```
./nginx -c /home/jiantou/nginx/conf/nginx.conf
./nginx -s stop
./nginx -s reload
```

### 2.2 yum安装


```shell
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
```