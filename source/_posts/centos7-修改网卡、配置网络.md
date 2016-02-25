title: centos7-修改网卡、配置网络
date: 2016-02-25 23:59:46
categories: 
- Linux
- Centos7
tags: 
- Linux
- Centos7
---
# Centos7-修改网卡、配置网络
## 我是在vmware中安装的
* 网络适配器调为桥接模式
* 修改`/etc/sysconfig/network-scripts/ifcfg-xxxx`里面的`Name`和`Dev`为`eth0`
![x](http://7xj3i5.com1.z0.glb.clouddn.com/8C6D5903-85B6-44BD-811C-5F0ADDD9E356.png)  

* 修改`ifcfg-xxxx`重命名为`ifcfg-eth0`
![y](http://7xj3i5.com1.z0.glb.clouddn.com/F6764A8C-88B6-4D39-8C37-DE856EBED293.png)

* 禁用可预测的命名规则，编辑`/etc/default/grub`为
![z](http://7xj3i5.com1.z0.glb.clouddn.com/4F5BA1BF-4495-4FBE-92BF-EE93DBFED7CE.png)

* 执行`grub2-mkconfig -o /boot/grub2/grub.cfg`
* 修改`/etc/resolv.conf`
![m](http://7xj3i5.com1.z0.glb.clouddn.com/70E8BAA9-7F78-4167-B3D7-BDD5951B6EEE.png)

* 重启network`systemctl restart network`

