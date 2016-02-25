title: keepalived+haproxy高可用负载均衡
date: 2016-02-25 23:45:11
categories: 
- Linux
- Keepalived
- Haproxy
tags: 
- Linux
- Keepalived
- Haproxy
---
# 基于Keepalived+haproxy实现高可用负载均衡
## Centos7集群配置，采用Keepalived+haproxy来做。两台负载均衡服务器做主、备切换.3台web服务器

	* VIP：192.168.0.185
	* h1：192.168.0.137
	* h2：192.168.0.138
	* web1：192.168.0.130
	* web2：192.168.0.131
	* web3：192.168.0.132

## h1安装keepalived
* 安装

		yum -y install keepalived
* 配置h1的keepalived

		vim /etc/keepalived/keepalived.conf

		! Configuration File for keepalived
		      global_defs {
		          notification_email {
		              xxx@xx.com
		          }
		      notification_email_from keepalived@localhost
		          smtp_server 127.0.0.1
		          smtp_connect_timeout 30
		          router_id LVS_DEVEL
		      }
		   
		      vrrp_script chk_haproxy {
		         script "killall -0 haproxy"
		         interval 2
		         weight 2
		      }
		      
		      vrrp_instance VI_1 {
		          state MASTER
		          interface eth0
		          virtual_router_id 85
		          priority 100
		          advert_int 1
		          nopreempt
		          authentication {
		             auth_type PASS
		             auth_pass 1111
		          }
		          track_script {
		             chk_haproxy
		          }
		          virtual_ipaddress {
		             192.168.0.185/24 dev eth0
		          }
		       } 
* 配置h2的keepalived

		vim /etc/keepalived/keepalived.conf

		! Configuration File for keepalived
		      global_defs {
		          notification_email {
		              xxx@xx.com
		          }
		      notification_email_from keepalived@localhost
		          smtp_server 127.0.0.1
		          smtp_connect_timeout 30
		          router_id LVS_DEVEL
		      }
		   
		      vrrp_script chk_haproxy {
		         script "killall -0 haproxy"
		         interval 2
		         weight 2
		      }
		      
		      vrrp_instance VI_1 {
		          state BACKUP
		          interface eth0
		          virtual_router_id 85
		          priority 90
		          advert_int 1
		          authentication {
		             auth_type PASS
		             auth_pass 1111
		          }
		          track_script {
		             chk_haproxy
		          }
		          virtual_ipaddress {
		             192.168.0.185/24 dev eth0
		          }
		       }
		  
## 安装haproxy
* 安装

		yum -y install haproxy
* 配置haproxy，两台机器的配置都是一样的，所以这里只提供一份

		global
		log         127.0.0.1 local2

		chroot      /var/lib/haproxy
		pidfile     /var/run/haproxy.pid
		maxconn     4000
		user        haproxy
		group       haproxy
		daemon

		# turn on stats unix socket
		stats socket /var/lib/haproxy/stats

		#---------------------------------------------------------------------
		# common defaults that all the 'listen' and 'backend' sections will
		# use if not designated in their block
		#---------------------------------------------------------------------
		defaults
		mode                    http
		log                     global
		option                  httplog
		option                  dontlognull
		option http-server-close
		#option forwardfor       except 127.0.0.0/8
		option                  redispatch
		retries                 3
		timeout http-request    10s
		timeout queue           1m
		timeout connect         10s
		timeout client          1m
		timeout server          1m
		timeout http-keep-alive 10s
		timeout check           10s
		maxconn                 3000

		# 用来监听状态，访问地址http://server:8080/admin?stats
		listen statistics
		mode http
		bind *:8080
		stats enable
		stats auth admin:admin
		stats uri /admin?stats
		stats hide-version
		stats admin if TRUE
		stats refresh 5s
		acl allow src 192.168.0.0

		# tcp监听
		listen tigase_5222
		mode tcp
		bind *:5222
		balance leastconn
		option tcplog
		option redispatch
		option log-health-checks
		timeout connect 1s
		timeout queue 5s
		timeout server 3600s
		timeout client 3600s
		maxconn 5000
		server webserver1 192.168.0.130:5222 inter 3000 rise 1 fall 1 check maxconn 5000
		server webserver2 192.168.0.131:5222 inter 3000 rise 1 fall 1 check maxconn 5000
		server webserver3 192.168.0.132:5222 inter 3000 rise 1 fall 1 check maxconn 5000

		listen tigase_5280
		mode tcp
		bind *:5280
		balance source
		option tcplog
		option redispatch
		option log-health-checks
		timeout connect 1s
		timeout queue 5s
		timeout server 3600s
		timeout client 3600s
		maxconn 5000
		server webserver1 192.168.0.130:5280 inter 3000 rise 1 fall 1 check maxconn 5000
		server webserver2 192.168.0.131:5280 inter 3000 rise 1 fall 1 check maxconn 5000
		server webserver3 192.168.0.132:5280 inter 3000 rise 1 fall 1 check maxconn 5000

		# http连接
		listen apache_httpd
		mode http
		bind *:80
		balance roundrobin
		option httplog
		server webserver1 192.168.0.130:80 inter 3000 rise 1 fall 1 check maxconn 5000
		server webserver2 192.168.0.131:80 inter 3000 rise 1 fall 1 check maxconn 5000
		server webserver3 192.168.0.132:80 inter 3000 rise 1 fall 1 check maxconn 5000
		
## 启动
* 启动haproxy和keepalived，首先先启动h1的haproxy和keepalived，然后启动h2的

		service haproxy start
		service keepalived

## 查看状态
* 查看VIP状态，当h1正常运行的时候

		[root@newkeep1 keepalived]# ip a|grep eth0
		2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
		    inet 192.168.0.137/24 brd 192.168.0.255 scope global eth0
		    inet 192.168.0.185/24 scope global secondary eth0
* 手动关闭h1的keepalived, 在h2上查看

		[root@newkeep1 keepalived]# ip a|grep eth0
		2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
		    inet 192.168.0.138/24 brd 192.168.0.255 scope global eth0
		    inet 192.168.0.185/24 scope global secondary eth0
* 再去查看h1的时候，发现VIP已经飘走了。当重新启动h1的时候，会恢复主机状态。
* 注意：如果启动的时候发现两台服务器都监听了VIP。则需要关闭防火墙。


