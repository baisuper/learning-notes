+++
title = "minio安装"
description = "minio安装"
date = "2017-08-01"
draft = false
weight = 5
+++
# 一，概览

# 二，部署

### 2.1 安装

***获取minio文件***  
服务器下载或者下载后上传到服务器都可以
```
wget https://dl.min.io/server/minio/release/linux-amd64/minio
```
***修改权限***  
```
chmod +x minio
```

***设置账号密码***  
```
export MINIO_ACCESS_KEY=hcbm
export MINIO_SECRET_KEY=adminhcbm
```
***运行***  
以后台方式运行
```
nohup ./minio server ~/minio >.minio.log &
```


### 2.2 分布式部署

#### 原理
MinIO分布式程序使用就删编码


#### 注意事项
* 所有的节点都需要设置环境变量`MINIO_ACCESS_KEY`和`MINIO_SECRET_KEY`，并且值相同
* 所有参与的节点建议使用相同的环境，比如：操作系统、硬盘数量并且可以相同联通
* 建议新建存储目录
* 所有节点所在服务器时间一致（官网文档上为时间差小于15分钟，这里建议服务器时间一致比较好），可以使用[NTP]](http://www.ntp.org/)来设置
* Windows系统部署MinIO还处于验证阶段，需要谨慎使用

#### 部署步骤

服务器信息

47.103.117.121
47.103.121.173 
47.103.119.77 
47.103.33.234

在每台服务器上运行
```shell
wget https://dl.min.io/server/minio/release/linux-amd64/minio
chmod +x minio
export MINIO_ACCESS_KEY=minio
export MINIO_SECRET_KEY=adminminio
# 为了后续方便，修改/etc/profile 将上面连个环境变量加到配置中
```

启动minio
```shell
nohup ./minio server http://172.19.239.206/data \
http://172.19.239.207/data \
http://172.19.239.208/data \
http://172.19.239.209/data \
http://172.19.239.210/data \
http://172.19.239.211/data \
http://172.19.239.212/data \
http://172.19.239.213/data > minio.log &

```


#### 2.2.1 分布式验证

分布式测试
* 首先启动8台服务器上的MinIO服务
* 随意找一个节点上传测试文件
* 打开其他节点的9000端口界面，可以看到通过其他节点上传的文件
* 连接服务器观察/data 目录可以看到每个服务器下都创建了这个文件了

故障测试
* 停掉其中一个节点
* 通过其中一个节点上传一个测试文件，可以正常上传
* 停掉其中3台时上传正常，停掉其中4台后，上传失败，删除失败，下载正常
* 停掉5台后，下载失败
* 回复停止的MinIO服务，发现/data 目录下的文件并没有被补全，约过了一分钟后查看补全了文件信息




### 3 Minio Client部署

```shell
wget https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc
./mc --help
```
 mc config host add myminio http://172.19.239.206:9000 minio adminminio


47.103.117.121 node1.example.com
47.103.121.173 node2.example.com
47.103.119.77 node3.example.com
47.103.33.234 node4.example.com

upstream minio {
    server 47.103.117.121:9000 weight=10 max_fails=2 fail_timeout=30s;
    server 47.103.121.173:9000 weight=10 max_fails=2 fail_timeout=30s;
    server 47.103.119.77:9000 weight=10 max_fails=2 fail_timeout=30s;
    server 47.103.33.234:9000 weight=10 max_fails=2 fail_timeout=30s;
}
server {
        listen 80;
        server_name localhost;
        charset utf-8;
        default_type text/html;
        location /{
            proxy_set_header Host $http_host;
                proxy_set_header X-Forwarded-For $remote_addr;
            client_body_buffer_size 10M;
                client_max_body_size 10G;
                    proxy_buffers 1024 4k;
                proxy_read_timeout 300;
                proxy_next_upstream error timeout http_404;
                proxy_pass http://minio;
}
