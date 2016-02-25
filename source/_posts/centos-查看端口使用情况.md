title: centos-查看端口使用情况
date: 2016-02-26 00:12:23
categories: 
- Linux
- Centos7
tags: 
- Linux
- Centos7
---
# Centos 查看端口使用情况
* 通过测试感觉tsung每次进行压测,没进一个用户需要占用本机一个端口，所以系统默认的端口打开是从32768到65535,所以这里就只能进2W多个用户，于是修改默认打开端口数。 
* 查看端口

		sysctl net.ipv4.ip_local_port_range
* 修改该值

		echo 1024 65535 > /proc/sys/net/ipv4/ip_local_port_range  

		||

		sudo sysctl -w net.ipv4.ip_local_port_range="1024 64000"

		||

		如果想一直生效，需要修改 /etc/sysctl.conf文件，加入net.ipv4.ip_local_port_range = 1024 65535,修改完成执行sysctl -p。


