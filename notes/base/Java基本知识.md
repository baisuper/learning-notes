# 写在前头
本文主要讲解如何在Linux服务器安装JDK，主要分为两个部分，第一部讲解如何在服务器在线或离线安装jdk，第二部分为扩展阅读，主要是希望帮助大家补全平时经常遇到但是没有去深入了解的的一些知识点。

- [在线安装](#在线安装)  
  - [Centos快速安装](#Centos快速安装)  
- [离线安装](#离线安装) 
- [扩展内容](#扩展内容)  
  - [Jdk的不同提供商](#Jdk的不同提供商) 
  - [yum源中的文件都是什么](#yum源中的文件都是什么) 
  - [离线安装如何选择Oraacle官网的jdk下载版本](#离线安装如何选择Oraacle官网的jdk下载版本)  
  - [Developmental vision](#developmental-vision) 

# 在线安装
## Centos快速安装

***yum安装jdk***

```shell
sudo yum install java-1.8.0-openjdk.x86_64
sudo yum install java-1.8.0-openjdk-devel
```

***查看安装是否成功***

```shell
java -version
```
java -version 展示内容详解可以看下方的扩展内容。

# 离线安装 

***下载安装文件***  
首先下载离线安装文件，可以从[官网](https://www.oracle.com/java/technologies/javase-downloads.html)下载，如果不懂得如何选择下载文件可以看下面的扩展阅读，Oracle官网的下载需要登入才可以，如果没有账号可以注册一个。  

也可以从从笔者的百度网盘下载，笔者的文件也是从官网下的，为懒得注册Oracle账户或者下载慢的同学提供个方便，分享了两个版本的
- [jdk-8u271-linux-x64.rpm](https://pan.baidu.com/s/1S-1gEGWWxiOefI9x8nueag)(提取码：i0mg)。
- [jdk-8u271-linux-x64.tar.gz](https://pan.baidu.com/s/1G97AlP9pZ7E3AiXh9m58ig)(提取码：achj)。

下载完成后通过ftp工具上传到服务器，执行以下命令进行安装，看好自己选择的下载文件类型

- 后缀名为.tar.gz运行如下命令，如:jdk-8u171-Linux-X64.tar.gz
```shell
# 将上传后的文件放到想存放jdk的目录后，将文件解压
tar -zxvf jdk-8u171-linux-x64.tar.gz

# 进入到解压出来的文件目录，如果感觉名称不合适先改名
cd jdk1.8.0_171

# 配置环境变量
# 修改JAVA_HOME为真实目录
echo 'export JAVA_HOME=/jdk1.8.0_171' >> /etc/profile
echo 'export JRE_HOME=/${JAVA_HOME}/jre' >> /etc/profile
echo 'export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib' >> /etc/profile
echo 'export PATH=/${JAVA_HOME}/bin:$PATH' >> /etc/profile

export JAVA_HOME=/jdk1.8.0_171
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
# 保存后运行如下命令使环境变量生效
source /etc/profile
```

- 后缀名为.rpm的执行rpm安装命令

<!-- todo -->

# 扩展内容

### yum源中的文件都是什么文件

***查看yum列表***

```shell
yum list java*
```

<!-- 截图 -->

可以看到在yum


------------

### OpenJDK的众多提供商有什么区别
目前JDK主要有JDK和OpenJDK，而OpenJDK又包含了很多家厂商，所以有些小伙本可能在选择JDK的时候会有疑问，这么多JDK到底有什么区别？又应该选择哪一个版本，看完这一段希望能让小伙伴们找到答案。  
Java最早由Sun Microsystems发布于1995年5月23日，1996年1月JDK1.0诞生，在2006年JavaOne大会上Sun称将对Java开放源代码，于2009年4月15日正式发布OpenJDK（Oracle在2010年收购Sun Microsystem之后接管了这个项目)，从此大量的OpenJDK版本开始出现。  
下面罗列了一些常见厂商的OpenJDK的资料，会不断更新，希望对大家游有用：

| 版本  | 官网/下载地址  | 说明 
| ------------ | ------------ | ------------ | 
| Oracle OpenJDK  |  https://openjdk.java.net/ |  Oracle官方的商业JDK的社区版，免费。
| AdoptOpenJDK | https://adoptopenjdk.net/ | 目前由Eclipse基金会维护的基于OpenJDK发布的免费版本，支持Oracle公司的HotSpot虚拟机和Eclipse基金会的OpenJ9虚拟机。是目前比较好的替换Oralce JDK的选择。
| Azul Zulu OpenJDK | https://www.azul.com/downloads/zulu-community/?package=jdk | Azul 成立于 2002 年，是一家私营软件公司，总部位于加州桑尼维尔，在全球设有销售、技术支持和工程办事处，基于OpenJDK发布，免费。
|华为毕昇JDK | https://www.huaweicloud.com/kunpeng/software/bishengjdk.html | 华为发布的OpenJDK，在ARM架构上进行了性能优化。毕昇JDK运行在大数据等场景下可以获得更好的性能。毕昇JDK 8与Java SE标准兼容，目前仅支持Linux/AArch64平台。毕昇JDK同时是OpenJDK的下游，会持续稳定为OpenJDK社区做出贡献。源码在Gitee可见。
| 腾讯Kona JDK | https://cloud.tencent.com/product/tkjdk | 腾讯发布的JDK，基于OpenJDK8，支持 java8 所有特性，针对云应用场景定制新的功能及优化， 具备更快的云应用启动速度，更好的性能以及更为便捷的分析、诊断工具。
| 阿里巴巴Dragonwell JDK | https://www.aliyun.com/product/dragonwell | 阿里发布的OpenJDK版本，OpenJDK的下游版本，主要有JAVA8 和11两个版本，提供长期支持，包括性能增强和安全修复。Alibaba Dragonwell完全兼容 Java SE 标准，可以在任何常用操作系统（包括 Linux、Windows 和 macOS）上开发 Java 应用程序， 运行时生产环境选择Alibaba Dragonwell。
| Amazon Corretto  | https://aws.amazon.com/cn/corretto/  | 亚马逊的OpenJDk版本，目前主要有JDK8，11，15三个版本。
| Red Hat build of OpenJDK | https://developers.redhat.com/products/openjdk/overview | Red Hat(红帽子)公司基于OpenJDK发布的免费版。主要版本从JDK8开始。
|SAP SapMachine | https://sap.github.io/SapMachine/ | SAP发布的OpenJDK，主要版本从JDK11开始。
| Liberica JDK | https://bell-sw.com/ | BellSoft发布的OpenJDK，默认捆绑了JavaFX，为 Linux与Windows 提供 32 位构建，win安装包默认配置环境变量。


------------


### Oracle官网jdk那么多下载文件，该如何选择
[官方](https://www.oracle.com/java/technologies/javase-downloads.html)提供的下载有多个下载文件，如果不熟悉的人可能会很蒙，到底要下哪个文件，这里我给大家说下具体该如何选择，以JDK8为例，下图是所有官方提供的下载文件，通过文件名和平台的来区分，首先对几个名称做一下解释：  

-  .tar.gz/tar.Z：源代码压缩文件
- rpm(linux)/dmg(mac)/exe(windows)：安装文件，这个相信就不用多解释了
- ARM(Advanced RISC Machine)：是一个精简指令集（RISC）处理器架构家族，其广泛地使用在许多嵌入式系统设计，ARM处理器非常适用于移动通信领域，ARM架构版本从ARMv3到ARMv7支持32位空间和32位算数运算，2011年发布的ARMv8-A架构添加了对64位空间和64位算术运算的支持，同时也更新了32位定长指令集。苹果最新推出的Apple M1芯片也是ARM架构。
- x86：x86泛指一系列英特尔公司用于开发处理器的指令集架构，这类处理器采用CISC（复杂指令集Complex Instruction Set Computer），最早为1978年面市的“Intel 8086”CPU，后续型号有：80286、80386、80486、80586，当年所说的386电脑，486电脑指的就是这个，再后来改为叫奔腾，奔腾2，奔腾3，以及酷睿系列都是这一脉的。
- x64：应该叫做x86-64，是基于x86架构的64位拓展，向后兼容于16位及32位的x86架构。x64于1999年由AMD设计，AMD首次公开64位集以扩展给x86，称为“AMD64”。其后也为英特尔所采用，现时英特尔称之为“Intel 64”。
- SPARC(Scalable Processor ARChitecture)：名称源自于可扩展处理器架构（Scalable Processor ARChitecture）的缩写，是一种RISC指令集架构，最早于1985年由Sun Microsystems(就是研发Java的那个)所设计，SPARC架构原设计给工作站使用，及后应用在昇阳、富士通等制造的大型SMP服务器上。而Solaris操作系统也是为SPARC设计的系统之一，除Solaris外，NeXTSTEP、Linux、FreeBSD、OpenBSD及NetBSD系统也提供SPARC版本。
- Solaris：这个大家可能不怎么熟悉，是原先Sun Microsystems研制的类Unix操作系统，后来被Oracle收购后，改名为Oracle Solaris，Solaris支持多种系统架构：SPARC、x86及x64，Solaris传统上与基于Sun SPARC处理器的硬件体系结构结合紧密，在设计上和市场上经常捆绑在一起，整个软硬件系统的可靠性和性能也因此大大增强。
- SVR4：指UNIX System V，是Unix操作系统众多版本中的一支，全称是System V Release 4。
- LInux/macOS/Windows：这几个就不用多少了，目前主流的操作系统。

|  产品/平台 | 格式 | 说明  |
| ------------ | ------------ | ------------ |
| **Linux ARM 64 RPM Package**  | jdk-8u271-linux-aarch64.rpm |  Linux操作系统，ARM 64位架构的RPM安装包 
| Linux ARM 64 Compressed Archive | jdk-8u271-linux-aarch64.tar.gz | Linux操作系统，ARM 64位架构的源代码压缩包
| Linux ARM 32 Hard Float ABI  |  jdk-8u271-linux-arm32-vfp-hflt.tar.gz  | 
| Linux x86 RPM Package	  | jdk-8u271-linux-i586.rpm  | Linux操作系统，x86架构，32位linux下这个
| Linux x86 Compressed Archive |  jdk-8u271-linux-i586.tar.gz | 同上
| **Linux x64 RPM Package** | jdk-8u271-linux-x64.rpm | Linux操作系统，x86-64架构，64位linux下这个
| Linux x64 Compressed Archive | jdk-8u271-linux-x64.tar.gz  | 同上
| **macOS x64**  | jdk-8u271-macosx-x64.dmg  | 苹果64位操作系统，下这个
| Solaris SPARC 64-bit (SVR4 package) |  jdk-8u271-solaris-sparcv9.tar.Z | Solaris操作系统，SPARC架构，SVR4软件包
| Solaris SPARC 64-bit  |  jdk-8u271-solaris-sparcv9.tar.gz  | Solaris操作系统，SPARC 64位架构
| Solaris x64 (SVR4 package)  | jdk-8u271-solaris-x64.tar.Z  | Solaris操作系统，x86-64架构，SVR4软件包
| Solaris x64 | jdk-8u271-solaris-x64.tar.gz  | Solaris操作系统，x86-64架构
|  Windows x86 | jdk-8u271-windows-i586.exe  | Windows x86架构，32位的Windows下这个
|  **Windows x64** |  jdk-8u271-windows-x64.exe | Windows x86架构，64位的Windows下这个，目前主流


------------



### Jdk的环境变量有那些，如何设置

### JDK和JRE有啥区别
简单来说：JDK包含JRE，JDK面向开发者，JRE面向使用者。

- JRE(Java Runtime Enviroment)：顾名思义，JRE提供的Java运行时所需的软件支持。
- JDK(Java Development Kit): JDK拥有JRE所拥有的一切，还有编译器（javac）和工具（如javadoc和jdb）。它能够创建和编译程序。

如果只是为了运行一下Java程序的话，那么只需要安装JRE就可以了。如果你需要进行一些Java编程方面的工作，那么你就需要安装JDK了。
### Java EE(J2EE)/Java SE(J2SE)的区别

### java -version命令详解
![](https://oscimg.oschina.net/oscnet/up-bb73d5d4c91f786feadc1958d1e7f2d87f7.png) 

TM:trade mark，商标

![](https://oscimg.oschina.net/oscnet/up-ef82fc602042ef773bafb32ac499ad77638.png)
###  Java小版本详解



参考资料：  
https://blog.adoptopenjdk.net/2020/06/adoptopenjdk-to-join-the-eclipse-foundation/  
https://adoptopenjdk.net/support.html#roadmap  
http://www.360doc.com/content/20/1111/15/31905426_945300170.shtml  
https://docs.oracle.com/cd/E19455-01/805-6331/6j5vgg66j/index.html
http://www-evasion.imag.fr/Membres/Francois.Faure/enseignement/ressources/java/jdk1.3/guide/performance/hotspot.html
https://docs.oracle.com/javase/8/docs/technotes/guides/vm/server-class.html
https://wiki.jikexueyuan.com/project/jvm-parameter/types-and-compiler.html