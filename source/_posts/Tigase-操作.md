title: Tigase-操作
date: 2015-12-10 00:14:17
categories: 
- XMPP
- Tigase
tags: 
- XMPP
- Tigase
---
### Tigase操作
____

#### 安装

* 在[Tigase-server](https://projects.tigase.org/projects/tigase-server/repository)下载`Tigase`源码
* `git clone https://repository.tigase.org/git/tigase-server.git`默认下载的时最新版本
* 如果要下载其他版本，在本机创建目录，在目录下
	* `git init`
	* `git remote add origin https://repository.tigase.org/git/tigase-server.git`
	* 如果要下载比如说`5.2.0`,`git pull origin tigase-server-5.2.0`
* 由于是源码要在`eclipse`中运行，运行`mvn eclipse:eclipse`先转成`eclipse`项目
* 打开`eclipse`导入`tigase`项目
* 下载下来后进入，修改`etc/tigase.conf`，修改`JAVA_HOME`
* 修改`etc/init.properties`,详细配置参数请见:[config](http://golearns.org/topic/view/28)
* 这时就可以启动项目了，入口文件是`XMPPServer.java`

#### 数据库的创建和导入

* 进入`tigase`的目录，在命令行运行`./scripts/db-create-mysql.sh admin admin mytigase root root localhost`
	* 后面参数根据自己实际情况而定
	* 第一、二个参数，如`admin admin`分别为`tigase`的用户名、密码，默认会拥有`root`权限
	* 第三个参数，如`mytigase`为所要创建的数据库
	* 第四、五个参数,分别为`mysql`数据库的用户名、密码
	* 第六个参数，如`localhost`为数据库连接地址，本地则为`localhost`
	
#### 项目运行

* 两种方式运行
	* 如果是在`eclipse`中测试环境则通过启动入口文件`XMPPServer.java`运行
	* 如果是部署运行，则需要先进行打包后再运行
		* 进入`tigase`目录,如果之前打包过，先运行`mvn clean`，然后`mvn -DskipTest package`
		* 进入`tigase`目录下的`target`目录，拷贝`tigase-server.jar`到`tigase`目录下的`jar`目录下
		* 执行`./scripts/tigase.sh start etc/tigase.conf`
		* 可用命令`start restart stop`

#### 插件编写

* 插件编写需要继承`XMPPProcessor`父类方法并实现`XMPPProcessorIfc`接口
* 所实现方法有

		public String id() {
		
			// 设置加载插件时的名称
			return "test";
		}
		
		@Override
		public String[][] supElementNamePaths() {
		
			// 所接收的消息类型
			return { { "message" }, { "presence" }};
		}
		
		@Override
		public String[] supNamespaces() {
		
			// 命名空间
			return { "jabber:client", "jabber:client" };
		}
		
		public void process(Packet packet, XMPPResourceConnection session,
			NonAuthUserRepository repo, Queue<Packet> results,
			Map<String, Object> settings) throws XMPPException {
			
			// packet 客户端发送过来的数据包和服务端返回的数据包
			// session 标注用户状态
			// repo 数据源
			// results 将消息加入消息队列并返回给客户端
			// settings 得到配置信息
		}
		
* 编写好插件后，在`init.properties`中添加插件，如`--sm-plugins=test,test2`多个插件用逗号隔开

#### MUC配置参数

* `muc#roomconfig_anonymity`(匿名房间的级别)
* `muc#maxhistoryfetch`(历史消息最大记录)
* `muc#roomconfig_membersonly`(房间仅对成员开放)
* `muc#roomconfig_moderatedroom`(房间是适度的)
* `muc#roomconfig_passwordprotectedroom`(需要密码才能进入房间)
* `muc#roomconfig_persistentroom`(房间是持久的)
* `muc#roomconfig_publicroom`(列出目录中的房间)
* `muc#roomconfig_roomdesc`(房间描述)
* `muc#roomconfig_roomname`(房间名)
* `muc#roomconfig_roomsecret`(密码)

----
### xmpp-error-code
***

### 302

	重定向 
	尽管HTTP规定中包含八种不同代码来表示重定向，Jabber只用了其中一个（用来代替所有的重定向错误）。不过Jabber代码302是为以后的功能预留的，目前还没有用到 
	
### 400

	坏请求 
	Jabber代码400用来通知Jabber客户端，一个请求因为其糟糕的语法不能被识别。例如，当一个Jabber客户端发送一个的订阅请求给它自己活发送一条没有包含“to”属性的消息，Jabber代码400就会产生。 
	
### 401 

	未授权的 
	Jabber代码401用来通知Jabber客户端它们提供的是错误的认证信息，如，在登陆一个Jabber服务器时使用一个错误的密码，或未知的用户名。 
	
### 402

	所需的费用 
	Jabber代码402为未来使用进行保留，目前还不用到。 
	
### 403

	禁止 
	Jabber代码403被Jabber服务器用来通知Jabber客户端该客户端的请求可以识别，但服务器拒绝执行。目前只用在注册过程中的密码存储失败。 
	
### 404

	没有找到 
	Jabber代码404用来表明Jabber服务器找不到任何与JabberID匹配的内容，该JabberID是一个Jabber客户端发送消息的目的地。如，一个用户打算向一个不存在的JabberID发送一条消息。如果接受者的Jabber服务器无法到达，将发送一个来自500级数的错误代码。 
	
### 405

	不允许的 
	Jabber代码405用在不允许操作被’from’地址标识的JabberID。例如，它可能产生在，一个非管理员用户试图在服务器上发送一条管理员级别的消息，或者一个用户试图发送一台Jabber服务器的时间或版本，或者发送一个不同的JabberID的vCard。 
	
### 406

	不被接受的 
	Jabber代码406用于服务器因为某些理由不接受一个包。例如，这个可能发生在，一个Jabber客户端试图使用jabber:iq:private在服务器上存储信息，但当前的用于存储的名字空间用”jabber:”开头（在Jabber里是一个被存的XML开头）。另一种可能产生406错误的情况是当一个Jabber客户端试图用一个空密码注册到一台Jabber服务器上。 
	
### 407

	必须注册 
	Jabber代码407当前不被使用 
	
### 408

	注册超时 
	当一个Jabber客户端不能在服务器准备好的时间内发起一个请求时，Jabber服务器生成Jabber代码 408。这个代码当前只用于Jabber会话管理器使用的零度认证模式中。 
	
### 409

	冲突
	
### 500

	服务器内部错误 
	当一台Jabber服务器遇到一种预期外的条件，该条件阻止服务器处理来自Jabber客户端的包，这是将用到Jabber代码500。现在，唯一会引发500错误代码的时间是当一个Jabber客户端试图通过服务器认证，而该认证因为某些原因没有被处理（如无法保存密码）。 
	
### 501

	不可执行 
	当服务器不支持Jabber客户端请求的功能，使用Jabber代码501。例如，该代码只当Jabber客户端发送一个认证请求，而该认证请求不包含服务器配置中定义的任何一种认证方式时，服务器发送Jabber代码501。这个代码还被用于，当一个Jabber客户端试图注册一个不允许注册的服务器。 
	
### 502

	远程服务器错误 
	当因为无法到达远程服务器导致转发一个包失败时，使用Jabber代码502。该代码发送的特殊例子包括一个远程服务器的连接的失败，无法获取远程服务器的主机名，以及远程服务器错误导致的外部时间过期。 
	
### 503

	服务无法 
	当一个Jabber客户端请求一个服务，而Jabber服务器通常由于一些临时原因无法提供该服务时，使获得用Jabber代码503。例如，一个Jabber客户端试图发送一条消息给另一个用户，该用户不在线，但它的服务器不提供离线存储服务，服务器将返回一个503错误代码给发送消息的JabberID。当为vcard-temp和jabber:iq:private名字空间设置信息时，出现通过xdb进行数据存储的写入错误，也使用该代码.
	
### 504

	远程服务器超时 
	Jabber代码504用于下列情况:试图连接一台服务器发生超时，错误的服务器名。 
	
### 510

	连接失败 
	Jabber代码510
	
***
* [tigase-muc](https://projects.tigase.org/projects/tigase-muc):群聊
* [tigase-pubsub](https://projects.tigase.org/projects/tigase-pubsub):消息订阅
* [message-archiving](https://projects.tigase.org/projects/message-archiving):消息归档

***
### init.properties

* [S] - String
* [s] - String Array
* [B] - Boolean
* [b] - Boolean Array
* [L] - long
* [l] - long Array
* [I] - Integer
* [i] - integer

### 处理支持管理员操作XEP-0133

* 将src/main/groovy/tigase/admin拷贝到scripts/下,XEP-0133协议由groovy完成。

### 执行自定义SQL

* 参考CounterDataLogger.java

### Tigase监控

* [tigase-monitor](https://projects.tigase.org/projects/tigase-monitor)

