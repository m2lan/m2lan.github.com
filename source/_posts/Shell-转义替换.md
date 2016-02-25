title: Shell-转义替换
date: 2015-12-10 02:17:29
categories: 
- Shell
tags: 
- Shell
---

	#!/bin/bash
	# 转义替换
	# -e 对转义字符进行替换

	echo -e "hello world \n hahahah"

	# 将命令执行的结果保存到变量中
	DATE=`date`
	echo $DATE

	# ${var:-message} 如果var为空或已被删除，则返回message，不会改变var的值
	# ${var:=message} 如果var为空或已被删除，则返回message，并将var的值设置为message
	# ${var:?message} 如果var为空或已被删除，则把message做为错误消息输出，脚本停止运行
	# ${var:+message} 如果var已被定义，则返回message，不会改变var的值

	echo ${var:-"a---hahahahha"}
	echo ${var:="b---hahhahaha"}
	echo ${var}

	unset var
	echo ${var:?"c---hahahahha"} # 程序运行此处将停止

	echo ${var:+"d---hahahhaha"}




