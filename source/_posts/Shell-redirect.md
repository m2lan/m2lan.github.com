title: Shell-redirect
date: 2015-12-10 02:20:35
categories: Shell
tags: Shell
---

	#!/bin/bash
	# 输出重定向
	#  将输出到终端的结果重定向到文件中

	# 加上双引号 可以对输出结果换行
	# > 会将已存在的结果覆盖
	# 如果不想覆盖 则使用 >> 如果文件存在，结果将会追加
	echo "$(ls -la)" > file

	echo "$(who)" >> file1


	# 输入结果重定向
	cat < file
	wc -l < file

	# 文档嵌入
	wc -l <<EOF
		hahahhdfdslfjdslfjsdlfjsdlfsd
		werjewlrjewlrjewlrjewlrjwer
		werjwelrjwlejrewljrew
	EOF


