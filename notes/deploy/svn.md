+++
title = "svn"
description = "svn"
date = "2017-08-01"
draft = false
weight = 5
+++

# 一，前置安装
无
# 二，yum安装 

***yum安装***  
```
yum install subversion
```

# 三，配置svn

***建立版本库目录***  
```
mkdir -p /data/svn/repo
```

***初始化目录***   
```
svnadmin create  /data/svn/repo
```
执行上面的命令后，自动建立repo测试库，查看/data/svn/repo 文件夹发现包含了conf, db,format,hooks, locks, README.txt等文件，说明一个SVN库已经建立

***配置用户密码***  
```
cd /data/svn/repos/conf
vi passwd
```

创建用户  
```
[users]
# harry = harryssecret
# sally = sallyssecret
hello=123

# 格式：用户名=密码
# 这样我们就建立了hello用户， 123密码
# 以上语句都必须顶格写, 左侧不能留空格, 否则会出错.
```

***配置访问权限***  
```
cd /data/svn/repos/conf
vi authz
```

目的是设置哪些用户可以访问哪些目录，向authz文件追加以下内容：  
```
#设置[/]代表根目录下所有的资源   或者写成[repl:/]
[/]
hello = rw

# 意思是hello用户对repo测试库下所有的目录有读写权限，当然也可以限定。
# 如果是自己用，就直接是读写吧。
# 以上语句都必须顶格写, 左侧不能留空格, 否则会出错.
```

修改配置文件svnserve.conf  
打开password-db = passwd  

# 四，启动运维
***启动***  
```
ps -ef|grep svn
svnserve -d -r /data/svn/repo  --listen-port=3960 
```
***端口配置***   
```
/sbin/iptables -I INPUT -p tcp --dport 3690 -j ACCEPT
/etc/rc.d/init.d/iptables save
/etc/init.d/iptables restart
/etc/init.d/iptables status
```

***访问***  
svn://ip地址  
端口默认为3690，输入配置好的用户名和密码即可。