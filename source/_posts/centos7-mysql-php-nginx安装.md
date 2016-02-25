title: centos7-mysql+php+nginx安装
date: 2016-02-26 00:04:01
categories: 
- Linux
- Centos7
- MySQL
- PHP
- Nginx
tags: 
- Linux
- Centos7
- MySQL
- PHP
- Nginx
---
# Centos7-Mysql+PHP+nginx安装
## 安装nginx

	rpm --import http://nginx.org/keys/nginx_signing.key
	rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
	yum install nginx
	systemctl start nginx
	systemctl enable nginx
	
* nginx 的默认文档要目录是 `/usr/share/nginx/html`，里面有`index.html`，访问`http://ip`入显示`welcome`则表示安装成功

## 安装PHP

	yum install php php-fpm php-mysql
	systemctl start php-fpm
	systemctl enable php-fpm
* 默认安装php的时候会自动安装apache web服务器，所以避免冲突应该禁掉httpd

		systemctl disable httpd
* 配置nginx主机，配置文件在`/etc/nginx/conf.d/default.conf`

		server {
			listen 80;
			server_name localhost;
			root /usr/share/nginx/html;
			index index.php index.html index.htm;
			     
			location / {
			}
			    
			error_page 500 502 503 504 /50x.html;
			location = /50x.html {
			}
			    
			location ~ \.php$ {
			try_files $uri =404;
			fastcgi_pass 127.0.0.1:9000;
			fastcgi_index index.php;
			fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
			include fastcgi_params;
			}
		}
	
## 安装mysql

	wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
	rpm -ivh mysql-community-release-el7-5.noarch.rpm
	yum install mysql-community-server
	service mysqld start
* mysql安装完成后，默认root密码为空

		mysql -uroot
		set password for ‘root’@‘localhost’ = password('mypasswd');
* centos7默认使用的firewall作为防火墙，这里改为iptables防火墙

		systemctl stop firewalld.service
		systemctl disable firewalld.service
* 安装iptables防火墙

		yum install iptables-services
* 修改防火墙配置文件

		vi /etc/sysconfig/iptables

		# Firewall configuration written by system-config-firewall
		# Manual customization of this file is not recommended.
		*filter
		:INPUT ACCEPT [0:0]
		:FORWARD ACCEPT [0:0]
		:OUTPUT ACCEPT [0:0]
		-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
		-A INPUT -p icmp -j ACCEPT
		-A INPUT -i lo -j ACCEPT
		-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
		-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
		-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
		-A INPUT -j REJECT --reject-with icmp-host-prohibited
		-A FORWARD -j REJECT --reject-with icmp-host-prohibited
		COMMIT
* 重启防火墙和开机启动

		systemctl restart iptables.service
		systemctl enable iptables.service


