title: Shell-数组
date: 2015-12-10 02:18:38
categories: 
- Shell
tags: 
- Shell
---

	#!/bin/bash
	# 数组 不支持多维数组
	# 通过index访问数组元素 arr[1]

	arr=(
		xxxx
		yyyy
		wwww
		"hello world"
	)

	# 使用下标定义
	arr1[0]=aaaa
	arr1[1]=bbb
	arr1[2]=cccc
	arr1[5]=dddd

	# 获取arr1所有值
	echo ${arr1[@]}

	# 获取arr1数组中值
	echo ${arr1[0]}
	echo ${arr1[1]}

