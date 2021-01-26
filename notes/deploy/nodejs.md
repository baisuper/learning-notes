+++
title = "nodejs"
description = "通过阅读本文章，了解安装nodejs"
date = "2017-08-01"
draft = false
weight = 5
+++

## 安装Node.js
```这里要注意，默认使用yum isntall nodejs安装的node是6版本的，而要求是8.0版本及以上```
这里直接下载最新版本进行安装
````
cd /root
wget https://nodejs.org/dist/v10.15.3/node-v10.15.3-linux-x64.tar.xz
tar -xvf node-v10.15.3-linux-x64.tar.xz
# 创建软连接，注意修改目录
sudo ln -s /root/node-v10.15.3-linux-x64/bin/node /usr/bin/node
sudo ln -s /root/node-v10.15.3-linux-x64/bin/npm /usr/bin/npm
sudo ln -s /root/node-v10.15.3-linux-x64/bin/yarn /usr/bin/yarn
````
***如果之前不小心用yum装错了，用如下命令删除后再运行上面的***
```
yum remove nodejs
sudo rm -rf /usr/bin/node
sudo rm -rf /usr/bin/npm
```



sudo ln -s /data/node/node-v10.15.3-linux-x64/bin/node /usr/bin/node
sudo ln -s /data/node/node-v10.15.3-linux-x64/bin/npm /usr/bin/npm
sudo ln -s /data/node/node-v10.15.3-linux-x64/bin/yarn /usr/bin/yarn


sudo ln -s /data/node/node-v10.15.3-linux-x64/bin/lerna /usr/bin/lerna