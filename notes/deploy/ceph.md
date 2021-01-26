# 一，概览

# 二，部署

### 2.1 安装


sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm


```
cat << EOM > /etc/yum.repos.d/ceph.repo
[ceph-noarch]
name=Ceph noarch packages
baseurl=https://download.ceph.com/rpm-luminous/el7/noarch
enabled=1
gpgcheck=1
type=rpm-md
gpgkey=https://download.ceph.com/keys/release.asc
EOM
```

[ceph]
name=ceph
baseurl=http://mirrors.163.com/ceph/rpm-jewel/el7/x86_64/
gpgcheck=0
[ceph-noarch]
name=cephnoarch
baseurl=http://mirrors.163.com/ceph/rpm-jewel/el7/noarch/
gpgcheck=0
[ceph-source]
name=Ceph source packages
baseurl=http://mirrors.163.com/ceph/rpm-jewel/el7/SRPMS
enabled=1
gpgcheck=1
type=rpm-md
gpgkey=https://mirrors.163.com/ceph/keys/release.asc
priority=1



sudo yum install ceph-deploy
sudo yum install ntp ntpdate ntp-doc
sudo yum install openssh-server

修改hosts
```shell

172.19.239.220         node1
172.19.239.222         node2
172.19.239.221         node3
```
ssh-keygen
ssh-copy-id root@node1
ssh-copy-id root@node2
ssh-copy-id root@node3


```
Host node001
   Hostname node001
   User root
Host node002
   Hostname node002
   User root
Host node003
   Hostname node003
   User root
```

mkdir my-cluster
cd my-cluster

```
sudo yum install ceph-deploy -y
```

ceph-deploy new node1 node2 node3
ceph-deploy install node1 node2 node3


```
pip uninstall urllib3
pip install --upgrade urllib3
