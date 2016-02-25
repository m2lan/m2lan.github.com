title: kafka
date: 2016-02-25 23:01:53
categories: [Apache, Kafka]
tags: [Apache, Kafka]
---
# kafka
> kafka是一个分布式、分区、复制提交日志服务。它提供了一个消息传送系统的功能。
> 通过zookeeper实现集群
> kafka集群由多个kafka实例组成，每个实例称为broker

## install

	wget http://apache.fayea.com/kafka/0.9.0.0/kafka_2.11-0.9.0.0.tgz
	tar xzf kafka_2.11-0.9.0.0.tgz
	export KAFKA_HOME=/root/kafka_2.11-0.9.0.0
	cd kafka_2.11-0.9.0.0
## 启动zookeeper

	bin/zookeeper-server-start.sh config/zookeeper.properties
## 启动kafka

	bin/kafka-server-start.sh config/server.properties
## 创建一个topic
* 创建一个单分区、单副本的`test`话题

		bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
* 获取`topic`列表

		bin/kafka-topics.sh --list --zookeeper localhost:2181
* 当发布到一个不存在的`topics`时，可以配置自动创建而不是手动创建
## 发送消息 
* 运行生产者客户端，然后发送一些消息

		bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
## 启动一个消费者
* 运行消费者客户端，接收消息

		bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
## 设置多个brokers集群
* 为每个brokers创建一个配置文件

		cp config/server.properties config/server-1.properties
		cp config/server.properties config/server-2.properties
* 配置文件内容
	* `config/server-1.properties`

			broker.id=1
			port=9093
			log.dir=/tmp/kafka-logs-1
	* `config/server-2.properties`

			broker.id=2
			port=9094
			log.dir=/tmp/kafka-logs-2
	> `broker.id`是集群节点唯一名称

* 启动两个新节点

		bin/kafka-server-start.sh config/server-1.properties &
		bin/kafka-server-start.sh config/server-2.properties &
* 创建有3个副本的新`topics`

		bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 3 --partitions 1 --topic my-topic
* 在集群环境，查看`broker`都做了什么

		bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-topic
		
		Topic:my-topic	PartitionCount:1 ReplicationFactor:3 Configs:
			Topic: my-topic	Partition: 0 Leader: 2 Replicas: 2,0,1 Isr: 2,0,1
> `leader`负责节点所有读取和写入指定分区
> `replicas`复制此分区节点列表日志
> `isr`集中同步副本

* 查看单复制集下的`topics`

		bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic test
		
		Topic:test	PartitionCount:1 ReplicationFactor:1 Configs:
			Topic: test	Partition: 0 Leader: 0 Replicas: 0 Isr: 0
* 给集群中的`topics`发送一些消息

		bin/kafka-console-producer.sh --broker-list localhost:9092 --topic my-topic
* 接收消息

		bin/kafka-console-consumer.sh --zookeeper localhost:2181 --from-beginning --topic my-topic
* 测试容错性，杀掉`server-1.properties`

		ps aux|grep server-1.properties
		kill -9 PID
* 查看

		bin/kafka-topics.sh --describe --zookeeper localhost:2181 --topic my-topic
		
		Topic:my-topic	PartitionCount:1 ReplicationFactor:3 Configs:
			Topic: my-topic	Partition: 0 Leader: 2 Replicas: 2,0,1 Isr: 2,0
			
## 使用kafka连接导入/导出数据
* 使用`kafka`的`connect`工具导入/导出数据
	* 首先需要建立一些数据来测试

			echo -e "foo\nbar" > test.txt
			
			bin/connect-standalone.sh config/connect-standalone.properties config/connect-file-source.properties config/connect-file-sink.properties
	* 使用之前启动默认的本地集群配置并创建两个连接，第一个是源连接器，它从输入文本读取消息，并各自产生的一个输出文件中的一行。
		* 连接器开始读取`test.txt`产生一个`topics`为`connect-test`
		* 接收端连接器应该开始从`topic`读取消息`connect-test`,并写入到`test.sink.txt`
		* 查看`topics`列表

				[root@keep-haproxy134 kafka_2.11-0.9.0.0]# bin/kafka-topics.sh --list --zookeeper localhost:2181
				__consumer_offsets
				connect-test
				my-topic
				test
				test2
		* 启动一个消费者控制台查看`topic`为`connect-test`的数据

				[root@keep-haproxy134 kafka_2.11-0.9.0.0]# bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic connect-test --from-beginning
				{"schema":{"type":"string","optional":false},"payload":"foo"}
				{"schema":{"type":"string","optional":false},"payload":"bar"}
		* 重新打开一个终端，给`test.txt`中追加数据，再看消费者控制台的数据

				echo "Hello world" >> test.txt
		* 消费者控制台数据为

				[root@keep-haproxy134 kafka_2.11-0.9.0.0]# bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic connect-test --from-beginning
				{"schema":{"type":"string","optional":false},"payload":"foo"}
				{"schema":{"type":"string","optional":false},"payload":"bar"}
				{"schema":{"type":"string","optional":false},"payload":"Hello world"}
				
## API
* 生产者API

		<dependency>
		    <groupId>org.apache.kafka</groupId>
		    <artifactId>kafka-clients</artifactId>
		    <version>0.9.0.0</version>
		</dependency>
	参考: [生产者doc](http://kafka.apache.org/090/javadoc/index.html?org/apache/kafka/clients/producer/KafkaProducer.html)
* 消费者API

		<dependency>
		    <groupId>org.apache.kafka</groupId>
		    <artifactId>kafka-clients</artifactId>
		    <version>0.9.0.0</version>
		</dependency>
	参考: [消费者doc](http://kafka.apache.org/090/javadoc/index.html?org/apache/kafka/clients/consumer/KafkaConsumer.html)

## error
* `org.apache.kafka.common.errors.TimeoutException: Batch Expired`修改`config/server.properties`文件的`advertised.host.name=IP地址`

## server.properties配置项

	broker.id=0
	listeners=PLAINTEXT://:9092
	advertised.host.name=192.168.1.211
	num.network.threads=3
	
	num.io.threads=8
	socket.send.buffer.bytes=102400
	socket.receive.buffer.bytes=102400
	socket.request.max.bytes=104857600
	log.dirs=/tmp/kafka-logs
	num.partitions=1
	num.recovery.threads.per.data.dir=1
	log.retention.hours=168
	log.segment.bytes=1073741824
	log.retention.check.interval.ms=300000
	log.cleaner.enable=false
	zookeeper.connect=localhost:2181
	zookeeper.connection.timeout.ms=6000
	auto.create.topics.enable=true
	default.replication.factor=3


