title: centos-安装nginx
date: 2016-02-26 00:17:11
categories: 
- Linux
- Centos7
- Nginx
tags: 
- Linux
- Centos7
- Nginx
---
# Centos 安装Nginx

	yum install gcc-c++

	yum -y install zlib zlib-devel openssl openssl--devel pcre pcre-devel

	wget http://nginx.org/download/nginx-1.6.2.tar.gz

	tar xvzf nginx-1.6.2.tar.gz

	cd ./nginx-1.6.2

	./configure

	make && make install

	// 修改配置防火墙
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

	service iptables restart

	// 启动nginx
	cd /usr/local/nginx/sbin/

	./nginx


