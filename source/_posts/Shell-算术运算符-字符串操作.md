title: Shell-算术运算符+字符串操作
date: 2015-12-10 02:15:16
categories: Shell
tags: Shell
---

	#!/bin/bash
	#算术运算符

	A=10
	B=$(($A+20))
	echo ${B}

	echo $((10+10))

	# 表达式和运算符之间要存在空格,不能写成2+2
	num=`expr 2 + 2`
	echo $num

	echo $((10/10))
	echo `expr 10 / 10`
	echo $((10*10))

	# 乘法*号前面添加反斜杠才能进行运算
	echo `expr 10 \* 10`
==========

	#!/bin/bash
	#string

	STRING="hello world"
	SUBSTRING="wqpeowqpeowq xxxworldxxxx world"
	STRING2="QQQQQ WWWWWW BBBBBB EEEEEE"

	#字符串替换
	echo ${STRING[@]/world/world!}

	#字符串替换所有
	echo ${SUBSTRING[@]//world/world!}

	#字符串截取 ${STRING:n} 从第N个位置到结尾
	echo ${STRING:2}

	#字符串截取 ${STRING:POS:LEN} 从从第POS位置开始截取LEN长度的字符
	echo ${STRING:2:5}

	#删除字符串中的字符，和空替换 
	echo ${STRING2[@]// WWWWWW}

	#将字符串开头的字符替换
	echo ${STRING2[@]/#QQQQQ/MMMMM}

	#将字符串结尾的字符替换
	echo ${STRING2[@]/%EEEEEE/PPPPP}

	#将字符串提出为shell命令
	echo ${STRING2[@]/%EEEEEE/$(date +%Y-%m-%d)}

