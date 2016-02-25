title: Tigase-配置
date: 2015-12-10 00:09:25
categories: 
- XMPP
- Tigase
tags: 
- XMPP
- Tigase
---
# 创建默认的配置文件,包含在配置组件包括：会话管理器，客户端 - 服务器连接管理器和服务器到服务器的连接管理器。

	config-type=--gen-config-def
# 配置管理员账号
	--admins=admin@192.168.0.50
# 配置域名
	--virt-hosts = 192.168.0.50
# 调试输出信息
	--debug=server,message-archive,muc
# 使用的数据库
	--user-db=mysql
	--user-db-uri=jdbc:mysql://127.0.0.1:3306/mytigase?user=root&password=feng&useUnicode=true&characterEncoding=UTF-8&autoCreateUser=true
# 加载插件
	--sm-plugins=-starttls,message-archive-xep-0136,offlinemessage
# 最大的离线消息配置
	sess-man/plugins-conf/amp/store-limit=1000
# 默认房间是锁住的，其他用户就无法进入该房间
	muc/muc-lock-new-room[B]=false
# 消息过滤
	muc/message-filter-enabled[B]=false
# 允许聊天状态
	muc/muc-allow-chat-states[B]=true
# 默认情况下,tigase设置取决于最大可用内存为tigase服务器进程队列的大小
	--max-queue-size = 10000
# 加载组件
	--comp-name-1 = muc
	--comp-class-1 = tigase.muc.MUCComponent
	--comp-name-2=message-archive
	--comp-class-2=tigase.archive.MessageArchiveComponent
	--comp-name-3 = pubsub
	--comp-class-3 = tigase.pubsub.PubSubComponent
# 存储用户的聊天记录
	message-archive/archive-repo-uri=jdbc:mysql://127.0.0.1:3306/mytigase?user=root&password=feng&useUnicode=true&characterEncoding=UTF-8&autoCreateUser=true
	sess-man/plugins-conf/message-archive-xep-0136/component-jid=message-archive@192.168.0.50
	sess-man/plugins-conf/message-archive-xep-0136/default-store-method=body
	sess-man/plugins-conf/message-archive-xep-0136/required-store-method=body
	sess-man/plugins-conf/message-archive-xep-0136/auto=true
	cl-comp/compress-stream[B]=false
	message-router/components/msg-receivers/s2s.active[B]=false
	c2s/net-buffer-limit[I]=4194304
* [详细参考官方配置](http://docs.tigase.org/tigase-server/snapshot/Properties_Guide/webhelp/)

