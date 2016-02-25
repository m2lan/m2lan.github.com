title: Mongodb-部署复制集
date: 2015-12-09 23:37:09
categories: Mongodb
tags: Mongodb
---
## 部署3个节点组成的复制集
* 3台IP为
	* 192.168.0.139 主机名：mongoname1
	* 192.168.0.140 主机名：mongoname2
	* 192.168.0.141 主机名：mongoname3
* 修改`/etc/mongodb.conf`，添加`replSet = rs0`(3个节点保持一致)
* 启动`mongodb`
* 在其中一个节点上执行`rs.initiate()`,用来初始化复制集

		rs.initiate("mongoname1")
* 查看当前配置信息

		rs.conf()
* 将其他节点加入复制集

		rs.add("mongoname2");
		rs.add("mongoname3");
* 查看配置

		rs.conf()
* 查看复制集状态

		rs.status()
* 如果在多个节点上执行了rs.initate(),可以做以下操作后重启mongodb

		use local
		db.dropDatabase()


