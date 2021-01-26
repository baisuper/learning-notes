+++
title = "kubernetes安装"
description = "本文描述了如何在服务器安装kubernetes"
date = "2017-08-01"
draft = false
weight = 4
+++
# 一，概述

# 二，安装步骤  
上传所有的离线安装包到服务器  

### 2.1 上传离线安装包   

```
tar -zxvf kubeadm-offline-package.tar.gz
```

### 2.2 安装nginx  

```
cd kubeadm-offline-package/nginx
sudo rpm -ivh --force *.rpm
# 修改nginx配置搭理的目录
# 默认是用nginx用户启动，需要注意目录权限问题，或者改为root
vi /root/kubeadm-offline-package/nginx.conf
# 复制配置文件，如果已经安装nginx，参考配置文件修改
sudo cp /root/kubeadm-offline-package/nginx.conf /etc/nginx/nginx.conf
# 启动nginx服务
sudo systemctl enable nginx && sudo systemctl start nginx
```
访问ip:9080验证是否成功  

### 2.3 配置yum仓库
***此步骤需要在所有节点执行***  

```
sudo mv /etc/yum.repos.d/ /etc/yum.repos.d.bak && sudo mkdir /etc/yum.repos.d

# 将如下命令的<HOST_IP>修改为实际的ip后运行
cat >> /etc/yum.repos.d/k8s.repo <<EOF
[k8s]
name=K8s Repository
baseurl=http://<HOST_IP>:9080
enabled=1
gpgcheck=0
EOF
# 比如
cat >> /etc/yum.repos.d/k8s.repo <<EOF
[k8s]
name=K8s Repository
baseurl=http://10.137.61.31:9080/
enabled=1
gpgcheck=0
EOF
# 更新缓存
sudo yum clean all && sudo yum makecache

```

### 2.4安装ansible  
***在部署节点执行***  
```
sudo yum install -y \
    ansible \
    httpd-tools \
    pyOpenSSL \
    python-cryptography \
    python-lxml \
    python-netaddr \
    python-passlib \
    python-pip
# 验证
ansible --version
```

### 2.5 安装docker，导入镜像  
***此步骤需要在所有节点执行***  
```
sudo yum install docker docker-engine-17.05.0.ce-1.el7.centos -y
systemctl enable docker && systemctl start docker
# 上传kube.tar到服务器，运行命令导入镜像
docker load -i kube.tar
```
### 2.6 建立ssh免密登入  
在deploy机器上运行
```
ssh-keygen -t rsa -b 2048 回车回车回车
ssh-copy-id $IPs #$IPs为所有节点地址包括自身，按照提示输入yes 和root密码
```
### 2.7 安装k8s  
```
tar -zxvf kubeadm-ansible.tar.gz
# 编辑inventory/vars添加如下变量
k8s_yum_repo: "http://<HOST_IP>:9080"
```
k8s_yum_repo: "http://10.137.61.31:3960"
***从此开始的安装不走可以参考猪齿鱼官方文档***  


helm install c7n/nfs-provisioner \
    --set rbac.create=true \
    --set service.enabled=true \
    --set persistence.enabled=true \
    --set persistence.nodeName=master \
    --set persistence.hostPath=/u01/prod \
    --version 0.1.0 \
    --name nfs-provisioner \
    --namespace c7n-system