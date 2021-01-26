+++
title = "hugo"
description = "hugo"
date = "2017-08-01"
draft = false
weight = 1
+++

# 一，前置安装

# 二，centos7安装

***运行如下命令***  

```
# 从GitHub下载代码
wget https://github.com/gohugoio/hugo/releases/download/v0.41/hugo_0.41_Linux-64bit.tar.gz
# 解压
tar -zxvf ./hugo_0.41_Linux-64bit.tar.gz
# 复制命令到系统bin下
cp ./hugo /usr/local/bin/

```

## 1.1 本地安装hugo

```
从 hugo 的 GitHub 仓库 https://github.com/gohugoio/hugo/releases 
下载适合自己的hugo版本，解压后添加到 Windows 的系统环境变量的 PATH 中即可，不需安装。

```
## 1.2 拉取文档平台，安装hugo
选择需要克隆文档平台的地址，鼠标右键单击，选择GIT Bash Here，执行 
```
git clone https://code.choerodon.com.cn/hzero-hcbm/hcbm-website.git 
```
进入克隆文档服务根目录，执行hugo server，文档服务即可启动,如下图所示，会看到一个地址，其中数字对应端口号，每次运行一定要查看这个地址，端口号可能会变，所以每次访问地址以这个界面显示地址为准。

![image](/img/docs/deploy/jar/images/hugo01.png)