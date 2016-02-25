title: centos-升级mysql
date: 2016-02-26 00:18:12
categories: 
- Linux
- Centos7
- MySQL
tags: 
- Linux
- Centos7
- MySQL
---
# Centos 升级mysql
* Centos默认mysql版本是5.1，但是高版本的mysql对性能有很大的提升。并且支持的字符最多有4个字节，支持emoji表情。查看Centos内核版本，下载对应的[yum源](http://dev.mysql.com/downloads/repo/yum/)

		rpm -Uvh http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
		yum -y upgrade mysql mysql_upgrade

		// 配置/etc/my.cnf
		[mysqld]
		datadir=/var/lib/mysql
		socket=/var/lib/mysql/mysql.sock
		user=mysql
		# Disabling symbolic-links is recommended to prevent assorted security risks
		symbolic-links=0
		character-set-server = utf8mb4
		collation-server = utf8mb4_unicode_ci
		init_connect='SET NAMES utf8mb4'
		character-set-client-handshake = FALSE
		[mysql]
		default-character-set=utf8mb4
		[mysqld_safe]
		log-error=/var/log/mysqld.log
		pid-file=/var/run/mysqld/mysqld.pid

		[client]
		default-character-set=utf8mb4
* 启动mysql查看编码

		SHOW VARIABLES WHERE Variable_name LIKE 'character\_set\_%' OR Variable_name LIKE 'collation%';
* 在连接数据看的时候会出现`Cannot load from mysql.proc. The table is probably corrupted`,原因是`mysql.proc`在升级的时候有个字段没有生成成功。在5.1中`mysql.proc`的`comment`字段是`varcahr(64)`,但在5.6中应该是text执行下面的语句修改就可以了。

		ALTER TABLE `proc` MODIFY COLUMN `comment` text CHARACTER SET utf8 COLLATE utf8_bin NOT NULL AFTER `sql_mode`;


