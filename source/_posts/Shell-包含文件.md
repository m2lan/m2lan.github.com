title: Shell-包含文件
date: 2015-12-10 02:16:28
categories: Shell
tags: Shell
---

	# shell 中包含外部脚本，可以使用
	# 1. . filename
	# 2. source filename
	# 现在新建一个脚本test.sh，内容如下

	USERNAME="john"
	PASSWORD="123456"

	# 再新建一个include.sh，内容如下

	#!/bin/bash

	. ./test.sh

	echo $USERNAME  # 输出john

