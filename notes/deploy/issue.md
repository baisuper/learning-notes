+++
title = "问题记录"
description = "问题记录"
date = "2017-08-01"
draft = false
weight = 15
+++
# 问题记录

#### zookeeper报错Will not attempt to authenticate using SASL
1，zookeeper版本客户端和服务器不一致导致

### redis尝试在cron下创建定时任务
***问题现象***  
查看redis日志发现报错：  
```
Failed opening the RDB file root (in server root dir /etc/cron.d) for saving: Permission denied
```
***问题原因***  
此问题是因为redis使用不安全配置并且发布到公网，被攻击者扫描后攻击了，尝试在系统中创建定时任务来获取权限。但是由于我们的redis使用的是redis用户启动的所以守住了最后的阵地。

***解决方法***  
- 关闭redis端口外网访问端口权限，使外网无法连接  
- 修改redis默认端口，并添加访问密码  

### 中文乱码
***问题现象***  
合同打印中文显示乱码，在线编辑中文乱码

***问题原因***   
服务器中文字体缺失

***解决方法***  
在服务器安装中文字体
详细步骤

### maven无法打包，报错无法下载ojdbc7依赖
***解决方法***  
- 下载依赖并上传
- 运行命令导入
mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc7 -Dversion=12.1.0.1.0 -Dpackaging=jar -Dfile=ojdbc7.jar

### 更新Pyhton2.6到2.7
参考链接: 
https://www.cnblogs.com/yaoyuanmengjing/p/7853228.html  