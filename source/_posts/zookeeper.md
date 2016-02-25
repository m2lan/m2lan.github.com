title: zookeeper
date: 2016-02-25 23:03:14
categories: 
- Apache
- Zookeeper
tags: 
- Apache
- Zookeeper
---
# zookeeper
## 安装

		wget http://apache.fayea.com/zookeeper/zookeeper-3.4.8/zookeeper-3.4.8.tar.gz
		tar xvf zookeeper-3.4.8.tar.gz
		cd zookeeper-3.4.8
		cd conf
		cp zoo_sample.cfg zoo.cfg
		
		// zoo.cfg配置
		tickTime=2000
		initLimit=10
		syncLimit=5
		dataDir=/tmp/zookeeper
		clientPort=2181
		autopurge.purgeInterval=1
		server.1=localhost:2888:3888
## zoo.cfg配置参数
* tickTime
> zookeeper服务器之间或客户端与服务器之间维持心跳的时间间隔，每隔tickTime时间就会发一个心跳包
* initLimit
> 配置zookeeper接受客户端初始化连接最长多少个心跳间隔时间,initLimit * tickTime = 20s,超过20s zookeeper服务器还没收到客户端返回信息，表示客户端连接失败
* syncLimit
> 配置项标识Leader与Follower之间发送消息,请求和应答时长,最长不能超过多少个tickTime的时间长度,总的时间长度就是5*2000=10秒；
* dataDir
> Zookeeper保存数据的目录
* clientPort
> 客户端连接zookeeper服务器的端口
* server
	
		server.1=192.168.1.212:2887:3887
		server.2=192.168.1.213:2888:3888
		server.3=192.168.1.214:2889:3889
> server.A=B：C：D：
> A是一个数字,表示这个是第几号服务器,B是这个服务器的ip地址
> C第一个端口用来集群成员的信息交换,表示的是这个服务器与集群中的Leader服务器交换信息的端口
> D是在leader挂掉时专门用来进行选举leader所用
* 集群模式下需要在`dataDir`目录下创建`myid`文件，文件内容为`A`的值

## 启动

		bin/zkServer.sh start
	
##查看状态

		bin/zkServer.sh status
		-- 192.168.1.212状态 --
		[root@keep-haproxy134 zookeeper-3.4.8]# ./bin/zkServer.sh status
		ZooKeeper JMX enabled by default
		Using config: /root/zookeeper-3.4.8/bin/../conf/zoo.cfg
		Mode: follower
		
		--192.168.1.213状态 --
		[root@keep-haproxy134 zookeeper-3.4.8]# ./bin/zkServer.sh status
		ZooKeeper JMX enabled by default
		Using config: /root/zookeeper-3.4.8/bin/../conf/zoo.cfg
		Mode: leader
		
		-- 192.168.1.214状态 --
		[root@keep-haproxy134 zookeeper-3.4.8]# ./bin/zkServer.sh status
		ZooKeeper JMX enabled by default
		Using config: /root/zookeeper-3.4.8/bin/../conf/zoo.cfg
		Mode: follower


