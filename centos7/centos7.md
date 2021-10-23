### CentOS 7

#### Gcc

```bash
sudo yum install -y centos-release-scl
sudo yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
scl enable devtoolset-9 bash

# 开机启动
echo "source /opt/rh/devtoolset-9/enable" >> /etc/profile
```



### Basic

```bash
root asdf

vi /etc/sysconfig/network-scripts/ifcfg-ens33
BOOTPROTO=static
IPADDR=10.10.10.10
GATEWAY=10.10.10.2
DNS1=114.114.114.114
ONBOOT=yes

systemctl restart network
ip addr show

vi /etc/sysconfig/selinux
SELINUX=disabled

systemctl disable iptable

```



### ssh

```bash
ssh-keygen -t rsa -P ''

ssh-copy-id -i ~/.ssh/id_rsa.pub root@c1
ssh-copy-id -i ~/.ssh/id_rsa.pub root@c2
ssh-copy-id -i ~/.ssh/id_rsa.pub root@c3

```

















