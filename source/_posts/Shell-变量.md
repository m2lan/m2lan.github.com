title: Shell-变量
date: 2015-12-10 02:11:37
categories: 
- Shell
tags: 
- Shell
---

	#!/bin/bash

	# 变量 if---else---fi
	# 变量赋值不能有空格
	# USERNAME = "zhangsan" error
	# 使用定义过的变量 前面加上美元符$
	USERNAME="zhangsan"

	# 重新定义变量
	# USERNAME="hahhahaha"
	# 
	# 变量可以定义为只读的 如下：
	# 
	# URL="http://www.baidu.com"
	# readonly URL
	# URL="http://www.google.com.hk" --- error 
	# echo $URL
	# 
	# 删除变量 使用unset，不能删除只读变量
	# unset USERNAME
	# echo ${USERNAME} --- 输出为空
	# 
	if [ $USERNAME == "zhangsan" ]; then
		echo "ok $USERNAME"
		echo ${USERNAME}  # 返回变量值，更可观
		echo $USERNAME    # 返回变量值
	else
		echo "buok"
	fi

	# 特殊变量
	# $ 表示当前shell的进程ID，即PID
	# 
	echo $$
	echo $?


| 变量 | 含义 |
|---|----|
|$0 | 当前脚本文件名|
|$n | 传递给脚本或者函数的参数，n表示一个数字，如：第一个参数$1,第二个参数$2|
|$# | 传递给脚本的参数个数|
|$* | 传递给脚本或者函数的所有参数|
|$@ | 传递给脚本或者函数的所有参数|
|$? | 上个命令的退出状态，或函数的返回值|
|$$ | 当前shell的进程ID，即PID|


