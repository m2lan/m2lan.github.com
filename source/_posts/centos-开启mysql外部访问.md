title: centos-开启mysql外部访问
date: 2016-02-26 00:09:43
categories: 
- Linux
- Centos7
- MySQL
tags: 
- Linux
- Centos7
- MySQL
---
# Centos开启mysql外部访问

	grant all privileges on *.* to 'root'@'%' identified by 'password' with grant option;

	iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT //开启3306端口


