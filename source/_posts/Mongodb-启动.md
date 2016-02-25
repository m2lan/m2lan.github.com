title: Mongodb-启动
date: 2015-12-09 23:26:15
categories: Mongodb
tags: Mongodb
---
## Mac下安装PHP的mongodb扩展

* `XAMPP/bin`

		sudo pecl install mongo

* `php.ini`添加

		extension=mongo.so

## 启动
* 创建`/data/db`目录`sudo mkdir -p /data/db`
* 设置`/data/db`目录权限`sudo chown -R 你的系统登录用户名 /data`
* 终端执行`mongod`
* 输入`mongo`便可连结到默认的数据库

