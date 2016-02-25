title: Shell-case
date: 2015-12-10 02:16:58
categories: Shell
tags: Shell
---

	#!/bin/bash
	#case

	NAME="john"

	case ${NAME} in
		"john" )
			echo "hahahha --- john"
			;;
		"johnx" )
			echo "huhuhuhu ---- johnx"
			;;
	esac

	echo "请输入1-3"
	read INPUT
	case $INPUT in
		1 )
			echo "你输入了1";;
		2 )
			echo "你输入了2";;
		3 )
			echo "你输入了3";;
		* ) # 当以上条件不匹配，则会进入到这里
			echo "你的输出超出了范围";;
	esac

	option="${1}"
	case $option in
		-f ) FILE="${2}"
			echo "file name is $FILE";;
		-d ) DIR="${2}"
			echo "dir name is $DIR";;
		* )
			echo "`basename ${0}`: usage: [-f file] | [-d directory]"
			exit 1;; # 脚本运行的返回值 exit 0 表示成功， exit 1 表示失败
	esac




