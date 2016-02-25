title: centos-安装tsung压力测试工具
date: 2016-02-26 00:20:51
categories: 
- Linux
- Centos7
- Tsung
tags: 
- Linux
- Centos7
- Tsung
---
# Centos 安装Tsung压力测试工具
## Tsung安装包、erlang安装包、Perl ( 生成报告所需模块)、图形库gnuplot等

	yum install gd libpng zlib
## 安装erlang

	wget http://www.erlang.org/download/otp_src_R15B02.tar.gz
	tar xvzf otp_src_R15B02.tar.gz
	cd otp_src_R15B02
	./configure  
	// 这一步如果出现configure: error: /bin/sh '/root/otp_src_R15B02/erts/configure' failed for erts
	yum install ncurses-devel

	make
	make install
	
## 安装gnuplot

	wget http://nchc.dl.sourceforge.net/project/gnuplot/gnuplot/4.4.0/gnuplot-4.4.0.tar.gz
	tar xvzf gnuplot-4.4.0.tar.gz
	cd gnuplot-4.4.0
	./configure
	make
	make install
	
## 下载安装Tsung

	wget http://tsung.erlang-projects.org/dist/tsung-1.4.2.tar.gz  
	tar xvzf tsung-1.4.2.tar.gz
	cd tsung-1.4.2
	./configure
	make && make install
	
## 安装perl Template,用来生成报告模板

	wget http://cpan.org/modules/by-module/Template/Template-Toolkit-2.24.tar.gz  
	tar xvzf Template-Toolkit-2.24.tar.gz
	cd Template-Toolkit-2.24
	perl Makefile.PL
	make
	make test
	make install
	
## 在root目录下创建.tsung目录

	mkdir ~/.tsung
	cd ~/.tsung
	cp /usr/local/share/doc/tsung/examples/http_simple.xml ~/.tsung/tsung.xml
	
## 运行
* 运行，默认执行`~/.tsung/tsung.xml`配置

		tsung start
		
## 进入需要生成图表的log目录

	cd ~/.tsung/log/20141208-1047/
	/usr/local/lib/tsung/bin/tsung_stats.pl
	
	// 如果出现`Error while running gnuplot: 对设备不适当的 ioctl 操作 at /usr/local/lib/tsung/bin/tsung_stats.pl line 223.`
	
	yum -y install gnuplot
* 这是重新启动，进入需要生成图表的log目录

		[root@iZ2326o1rm2Z ~]# cd .tsung/log/20141208-1047/
		[root@iZ2326o1rm2Z 20141208-1047]# ls
		match.log  tsung_controller@iZ2326o1rm2Z.log  tsung.log  tsung.xml
		[root@iZ2326o1rm2Z 20141208-1047]# /usr/local/lib/tsung/bin/tsung_stats.pl 
		creating subdirectory data 
		creating subdirectory gnuplot_scripts 
		creating subdirectory images 
		Use of uninitialized value $interval in numeric ne (!=) at /usr/local/lib/tsung/bin/tsung_stats.pl line 428.
		No data for Session
		No data for Perfs
		No data for Transactions
		No data for Match
		No data for Event
		No data for Async
		No data for Errors
		No data for Users
		No data for Users_Arrival
		No data for Size
		size_rcv is equal to 0 !
		size_sent is equal to 0 !
* 打开report.html就可以查看生成的报表了


