---
title: Installation
weight: 15
---



# KVM 
KVM是一个基于内核的虚拟机（kernel virtual machine），从Linux内核版本号2.6.20开始已经集成在系统之中。这样要部署虚拟机就不需要额外的软件来支持，效率更高。但是KVM只能虚拟化CPU和内存，对于磁盘、网卡等外设无法模拟，所以需要结合QEMU来模拟其他设备（QEMU虽然也可以模拟CPU和内存，但是效率没有KVM高），也因此KVM其实是QEMU-KVM。

PS：如果虚拟机有安装VirtIO半虚拟化驱动的话能提高虚拟机性能，好在主流Linux系统已经内置了相关驱动，而Windows虚拟机就需要去KVM下载相关驱动了，查询方法：


lsmod | grep virtio
亲测，这个是在虚拟机查询，实体机没有
一、宿主机安装KVM

1、用VMware做实验的话第一步就是开启CPU虚拟化功能
2、检查宿主机CPU虚拟化是否开启、KVM内核是否加载

1
egrep "svm|vmx" /proc/cpuinfo  #查看CPU是否开启虚拟化（Inter是vmx，AMD是svm），如果过滤出的flags有内容就是已开启了
2
lsmod | grep kvm #查看KVM内核是否加载
这个实体机开启查询。
3、使用yum安装KVM相关软件包

```bash
yum install qemu-kvm  libvirt virt-install virt-manager bridge-utils

#qemu-kvm：核心软件包，实现虚拟化
#libvirt：核心软件包，管理KVM的工具，类似VMware也是管理虚拟机的工具
#virt-install：KVM虚拟机命令行管理工具
#virt-manager：KVM虚拟机图形化管理工具，可不装
#bridge-utils：实现网卡桥接的工具
```
4、启动libvirtd服务（如果该服务没启动的话virt-install等工具也无法使用）。该服务启动后系统会新增一块virbr0的网卡，该网卡的地址就是虚拟机使用NAT模式时使用的地址。vmware的NAT也是如此，宿主机一个网卡。

```bash
systemctl enable libvirtd.service
systemctl start libvirtd.service
```
5.5、为KVM创建桥接网络

KVM默认使用NAT模式来分配网络给虚拟机，在使用NAT模式时虚拟机可以访问外网，但外网无法直接访问虚拟机。所以生产环境更多是配置网桥实现桥接模式上网，让虚拟机与宿主机处于一个网段中，各自有一个自己的IP，可以把他们看成是一个局域网中的不同电脑。配置桥接模式有2种方式：

通过命令行配置桥接模式，该命令会修改配置文件实现永久生效


1
virsh iface-bridge eth0 br0  #把eth0网卡绑定到br0中


手动编辑配置文件实现桥接模式：
```
vim /etc/sysconfig/network-scripts/ifcfg-br0    #为br0新建配置文件

DEVICE=br0
TYPE=Bridge  #类型为桥接
BOOTPROTO=static
IPADDR=192.168.1.100    #配置能上网的IP地址，通常就是之前主机的IP
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
ONBOOT=yes

vim /etc/sysconfig/network-scripts/ifcfg-eth0    #将eth0绑定到桥接网络中
DEVICE=eth0
BRIDGE=br0  #这句就是把eth0桥接到br0
ONBOOT=yes
```
也可以通过命令行模式配置网卡，但是一定要把这些命令写到脚本中一起执行，否则网络就断了
#!/bin/bash
brctl addbr br0  #增加一块网卡
brctl addif br0 eth0  #让eth0网卡成为br0网卡的一员
ip addr del dev eth0 192.168.1.100/24  #删掉eth0的IP地址，因为BR0已经有这样的地址了
route add default gw 192.168.1.1  #增加网关

操作完成后查看网络信息会发现eth0已经没有IP了，而新生成的br0网卡有了对应的IP，且是UP状态。也可以通过brctl show命令查看网卡绑定关系
```bash
[root@lykj-dell-manager20 ~]# brctl show
bridge name	bridge id		STP enabled	interfaces
br0		8000.20040fef0dec	no		em1
							vnet0
docker0		8000.0242b5db39e0	no		
virbr0		8000.525400c9d639	yes		virbr0-nic
```
