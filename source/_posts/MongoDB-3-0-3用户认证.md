title: MongoDB-3.0.3用户认证
date: 2015-12-09 23:32:12
categories: 
- Mongodb
tags: 
- Mongodb
---
## 在Mongodb3.0.3中创建User进行权限验证

* 启动mongodb，切换db为admin

		use admin
		db.system.version.find()
		db.system.version.update({"_id": "authSchema"}, {"$set": {"currentVersion": 3}})
* 为数据库创建用户，这里使用的mydb数据库

		use mydb
		db.createUser({"user": "zhangsan", "pwd": "123456", roles: [{role: "dbOwner", db: "mydb"}]})
* 在mongodb.conf中添加authorization: enabled

		security:
		  authorization: enabled
* 完毕！重启mongodb验证。
* 参考:
	1. [配置文件操作](http://docs.mongodb.org/manual/reference/configuration-options/)
	2. [权限](http://docs.mongodb.org/manual/reference/built-in-roles/)


