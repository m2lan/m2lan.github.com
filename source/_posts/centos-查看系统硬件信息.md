title: centos-查看系统硬件信息
date: 2016-02-26 00:13:44
categories: 
- Linux
- Centos7
tags: 
- Linux
- Centos7
---
# Centos 查看系统硬件信息
* 查看CPU型号

		cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c

		2  Intel(R) Xeon(R) CPU E5-2430 0 @ 2.20GHz
* 查看物理CPU个数

		cat /proc/cpuinfo | grep physical | uniq -c

		1 physical id	: 0
		1 address sizes	: 46 bits physical, 48 bits virtual
		1 physical id	: 2
		1 address sizes	: 46 bits physical, 48 bits virtual
* 查看CPU运行模式：32位或64位

		getconf LONG_BIT

		64
* 查看CPU是否支持64位系统，如果大于0，则支持64位运算

		cat /proc/cpuinfo | grep flags | grep ' lm ' | wc -l 

		2
* 查看内存信息

		cat /proc/meminfo 

		MemTotal:        1922784 kB
		MemFree:          184468 kB
		Buffers:          198704 kB
		Cached:           630012 kB
		SwapCached:            0 kB
		Active:           997440 kB
		Inactive:         585112 kB
		Active(anon):     753856 kB
		Inactive(anon):      160 kB
		Active(file):     243584 kB
		Inactive(file):   584952 kB
		Unevictable:           0 kB
		Mlocked:               0 kB
		SwapTotal:             0 kB
		SwapFree:              0 kB
		Dirty:                28 kB
		Writeback:             0 kB
		AnonPages:        753960 kB
		Mapped:            30072 kB
		Shmem:               180 kB
		Slab:             124932 kB
		SReclaimable:     102336 kB
		SUnreclaim:        22596 kB
		KernelStack:        3528 kB
		PageTables:         5292 kB
		NFS_Unstable:          0 kB
		Bounce:                0 kB
		WritebackTmp:          0 kB
		CommitLimit:      961392 kB
		Committed_AS:    1604768 kB
		VmallocTotal:   34359738367 kB
		VmallocUsed:       11064 kB
		VmallocChunk:   34359725180 kB
		HardwareCorrupted:     0 kB
		AnonHugePages:    688128 kB
		HugePages_Total:       0
		HugePages_Free:        0
		HugePages_Rsvd:        0
		HugePages_Surp:        0
		Hugepagesize:       2048 kB
		DirectMap4k:        6144 kB
		DirectMap2M:     2095104 kB
* 查看当前系统内核信息

		uname -a

		Linux iZ2326o1rm2Z 2.6.32-431.23.3.el6.x86_64 #1 SMP Thu Jul 31 17:20:51 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux
* 查看当前操作系统内核信息

		cat /etc/issue | grep linux
* 查看服务器型号

		dmidecode | grep "Product Name"

		Product Name: HVM domU
* 查看当前打开的服务

		chkconfig --list
* 查看当前打开的端口

		netstat -an
* 查看磁盘信息

		fdisk -l
		df -h
* 进程监控命令，实时显示系统进程的资源占用情况

		top
* 系统监控命令，重点侧重于虚拟内存，也可以监控CPU、进程、内存分页以及IO的状态信息

		vmstat
* 列举系统中被打开的文件

		lsof -i：36000，显示使用36000端口的进程 
		lsof -u root，显示以root运行的程序 
		lsof -c php-fpm，显示php-fpm进程打开的文件 
		lsof php.ini，显示打开php.ini的进程。
* 查看Linux系统日志

		系统级别的日志文件存放路径为/var/log
		常用的系统日志为/var/log/messages
* 查找文件系统大文件

		可以首先通过df命令查看磁盘分区使用情况，比如df -m；
		然后：
		通过du命令查看具体文件夹的大小，比如du -sh ./*，du -h --max-depth=1|head -10；
		使用ls命令列出文件以及大小，比如ls -lSh；
		另外，也可以通过find命令直接查看特定目录下的文件大小，比如find / -type f -size +10M -exec ls -lrt {} \;


