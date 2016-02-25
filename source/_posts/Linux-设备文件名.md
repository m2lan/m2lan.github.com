title: Linux-设备文件名
date: 2016-02-25 23:34:08
categories: 
- Linux
tags: 
- Linux
---
# 各硬件设备在Linux中的文件名

|名称|目录|
|-----|------|
|IDE硬盘|/dev/hd[a-d]|
|SCSI/SATA/USB硬盘|/dev/sd[a-p]|
|U盘|/dev/sd[a-p]|
|软驱|/dev/fd[0-1]|
|打印机	|25针:/dev/Ip[0-2],USB:/dev/usb/Ip[0-15]|
|鼠标|USB:/dev/usb/mouse[0-15],PS2:/dev/psaux|
|当前CD ROM/DVD ROM|/dev/cdrom|
|当前鼠标|/dev/mouse|
|磁带机|IDE:/dev/ht0,SCSI:/dev/st0|


