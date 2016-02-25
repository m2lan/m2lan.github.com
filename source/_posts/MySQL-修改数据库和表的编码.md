title: MySQL-修改数据库和表的编码
date: 2015-12-09 23:23:13
categories: MySQL
tags: MySQL
---
* 查看数据库编码 

		show variables like 'char%';

* 修改表编码 

		ALTER TABLE 'table' DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci

* 修改表字段编码 

		ALTER TABLE 'test' CHANGE 'name' 'name' VARCHAR( 10 ) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL;

* 查看表编码 
	
		show create table mytable

* 修改mysql默认编码

		[mysqld]和[client]后添加character_set_server=utf8


