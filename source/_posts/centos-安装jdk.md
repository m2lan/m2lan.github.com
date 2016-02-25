title: centos-安装jdk
date: 2016-02-26 00:11:10
categories: 
- Linux
- Centos7
- JDK
tags: 
- Linux
- Centos7
- JDK
---
# Centos安装JDK

	wget http://download.oracle.com/otn-pub/java/jdk/7u3-b04/jdk-7u3-linux-x64.rpm(当出现秒下时候，出现HTTP request sent, awaiting response... 302 Moved Temporarily错误，通过chrom抓包，获取重定向后的链接地址)

	运行rpm -ivh jdk-7u3-linux-x64.rpm

	修改profile文件，vi /etc/profile,在文件最后加入

	  export JAVA_HOME=/usr/java/jdk1.7.0_03
	  export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	  export PATH=$PATH:$JAVA_HOME/bin

