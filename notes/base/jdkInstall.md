# 写在前头
本文主要讲解如何在Linux服务器安装JDK，主要分为两个部分，第一部讲解如何在服务器在线或离线安装jdk，第二部分为扩展阅读，主要是希望帮助大家补全平时经常遇到但是没有去深入了解的的一些知识点。

## Centos yum快速安装
在服务器上如何快速安装jdk，如果
### yum安装java

***安装jdk***

```shell
sudo yum install java-1.8.0-openjdk.x86_64
sudo yum install java-1.8.0-openjdk-devel
```

***查看安装是否成功***

```shell
java -version
```

### 离线安装 

***下载安装文件***  
首先下载离线安装文件，可以从[官网](https://www.oracle.com/java/technologies/javase-downloads.html)下载，如果不懂得如何选择下载文件可以看下面的扩展阅读，Oracle官网的下载需要登入才可以，如果没有账号可以注册一个。  

也可以从从笔者的百度网盘下载，笔者的文件也是从官网下的，为懒得注册Oracle账户或者下载慢的同学提供个方便，分享了两个版本的
- [jdk-8u271-linux-x64.rpm](https://pan.baidu.com/s/1S-1gEGWWxiOefI9x8nueag)(提取码：i0mg)。
- [jdk-8u271-linux-x64.tar.gz](https://pan.baidu.com/s/1G97AlP9pZ7E3AiXh9m58ig)(提取码：achj)。

下载完成后通过ftp工具上传到服务器，执行以下命令进行安装，看好自己选择的下载文件类型

```shell
java -version
```

# 扩展内容

### Jdk的不同提供商

OpenJdk其实有很多的提供商，比如Oracle，IBM，Adopt等等，不同的提供商提供的jdk也有少许差异，部分提供商的jdk只是为了运行在自己的服务器商更流畅，比如IBM的，同时不同的提供商提供的jdk的开放程度也不同，下面列举了一些常见的Openjdk版本和差异。

<!-- todo 列表 -->

### yum安装文件区别

***查看yum列表***

```shell
yum list java*
```

<!-- 截图 -->

可以看到在yum

### 离线安装如何选择Oraacle官网的jdk下载版本