title: Shell-ifelse
date: 2015-12-10 02:19:16
categories: 
- Shell
tags: 
- Shell
---

	#!/bin/bash
	#if--else

	NAME="john"

	# 注意if条件两边的空格
	if [ ${NAME} == "john" ]; then
		echo "hahhahahha"
	else
		echo "huhuhuhuhu"
	fi

	if [ ${NAME} == "john" ]; then
		echo "hahhahahha"
	elif [ ${NAME} == "johnx" ]; then
		echo "huhuhuhuhu"
	else
		echo "xxxxxxxxxx"
	fi

	# 数值类型比较
	# $a -lt $b    $a < $b
	# $a -gt $b    $a > $b
	# $a -le $b    $a <= $b
	# $a -ge $b    $a >= $b
	# $a -eq $b    $a is equal to $b
	# $a -ne $b    $a is not equal to $b

	# 字符串类型比较
	# "$a" = "$b"     $a 和 $b 相等
	# "$a" == "$b"    $a 和 $b 相等
	# "$a" != "$b"    $a 和 $b 不相等
	# -z "$a"         $a 是空的


