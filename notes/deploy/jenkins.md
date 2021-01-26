# Docker部署Jenkins

### 1.1 前置安装

-   安装docker
-   安装docker-compose

### 1.2 拉取镜像

```bash
docker pull jenkins


```

_**离线获取镜像**_

如果处于离线环境，可在能访问外网的机器上执行如下命令拉取镜像，打包

```bash
docker pull jenkins
docker save -o jenkins.tar jenkins


```

将打包文件上传到服务器之后，在服务器上执行如下命令加载镜像

```bash
docker load -i jenkins.tar


```

### 1.3 docker-compose安装、启动容器

_**创建docker-compose文件**_

创建docker.compose.yml文件，内容如下

```yml
version: "2"
services:
    jenkins:
    container_name: jenkins
    image: jenkins:latest
    restart: always
    privileged: true
    ports:
      - "9001:8080"
      - "50000:50000"
    volumes:
      - "./jenkins/home:/home/jenkins_home"


```

容器端口如下，可根据情况映射到本地端口

| 容器端口 | 本地端口 | 说明 |
| --- | --- | --- |
| 8080 | 9001 | http访问段口 |
| 50000 | 50000 |  |

数据路径映射如下，可根据情况映射到本地路径

| 容器路径 | 本地路径 | 说明 |
| --- | --- | --- |
| /var | ./jenkins/var | jenkins主目录 |

_**创建并启动容器**_

```bash
docker-compose up -d


```

_**查看启动日志**_

```bash
docker logs -f jenkins


```

### 1.4 访问并初始化管理员密码

访问jenkins地址，如 [http://localhost:9001](http://localhost:9001) ，首次访问出现如下页面

![](https://oscimg.oschina.net/oscnet/up-2a3ca3ffdbedf80dd3051a65aca400b679a.png)

进入容器中，复制 `/var/jenkins_home/secrets/initialAdminPassword` 文件内容并输入到密码框中

```bash
docker exec -i -t jenkins /bin/bash
cat /var/jenkins_home/secrets/initialAdminPassword


```

输入密码之后，可选择是否安装插件，点击取消（或者使用默认安装），关掉页面之后，进入主页面

_**修改密码**_

![](https://oscimg.oschina.net/oscnet/up-c90382fa12e45f290ff1de15adab4aab365.png)

# 二，yum安装Jenkins

_**下载依赖**_

```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

```

_**导入秘钥**_

```
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

```

_**安装**_ 此步骤可能会很慢

```
yum install jenkins

```

### 2.2 配置Jenkins

_**配置Java**_ 编辑/etc/init.d/jenkins文件查看Java路径是否正确，一般如果先安装Java后使用yum安装会自动配置

_**配置端口**_ 编辑jenkins的/etc/sysconfig/jenkins配置文件，修改端口、系统运行账户

```
JENKINS_USER="jenkins"
...
JENKINS_PORT="9001"

```

### 2.3启动Jenkins

_**运行**_

```
service jenkins start

```

_**复制临时密码**_

```
cat /var/lib/jenkins/secrets/initialAdminPassword

```

_**访问**_

```
http://ip:9001/

```

![](https://oscimg.oschina.net/oscnet/up-a243096b96716f5a3b9b589eefe6c2a7839.png)

# 三，安装插件（非必须）

_**安装publish-over-ssh**_

`publish-over-ssh`用于远程部署，下载插件最新版 [publish-over-ssh](http://updates.jenkins-ci.org/download/plugins/publish-over-ssh/)，如图

![](https://oscimg.oschina.net/oscnet/up-e2a7689287671d931e95a329e6817c487cc.png)

路径`系统管理 -> 管理插件 -> 高级`，上传插件，上传完成之后将自动安装。

![](https://oscimg.oschina.net/oscnet/up-b7e7f651189e381c47ff7b222f846c72944.png)

如果Jenkins所在服务器能访问外网，则直接能安装成功。如果处于内网环境，需要先手动安装其依赖插件，依赖插件列表如下

-   [Structs](http://updates.jenkins-ci.org/download/plugins/structs/)
-   [bouncycastle API](http://updates.jenkins-ci.org/download/plugins/bouncycastle-api/)
-   [Credentials](http://updates.jenkins-ci.org/download/plugins/credentials/)
-   [SSH Credentials](http://updates.jenkins-ci.org/download/plugins/ssh-credentials/)
-   [JSch dependency](http://updates.jenkins-ci.org/download/plugins/jsch/)
-   [publish-over](http://updates.jenkins-ci.org/download/plugins/publish-over/)

# 四，运维

### 查看日志

如果没有修改配置，一般为默认日志路径

```
cat /var/log/jenkins/jenkins.log

```

---

### 清理Jenkins历史数据

_**访问脚本前端页面**_  
在Jenkins访问路径后加上/script 访问脚本运行界面，如：  
`http://ip:端口/script`  
运行以下代码

```
def jobName = "jobName"
def maxNumber = 100

Jenkins.instance.getItemByFullName(jobName).builds.findAll {
  it.number <= maxNumber
}.each {
  it.delete()
}

```