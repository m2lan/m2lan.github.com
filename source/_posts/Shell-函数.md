title: Shell-函数
date: 2015-12-10 02:18:03
categories: 
- Shell
tags: 
- Shell
---

	#!/bin/bash
	# function --
	# 函数返回值只能是整数，用来表示函数执行是否成功，0表示执行成功，其他表示失败
	# 如果返回非整数，则error
	# 函数名前面的function可加可不加
	# 函数可以使用return，return的返回值可以通过$?来接收
	# 
	# 也可以对函数进行删除，使用unset，后面加上.f
	# 如要删除function1函数：unset .f function1
	# 

	# 无参函数
	function function1 {
		echo function1.
	}

	# 带参函数
	function function2 {
		echo $1
	}

	# 多个参数
	function function3 {
		echo $1 --- $2
	}

	function4() {
		echo "function4"

		return $((10 + 20))
	}

	# 函数嵌套调用
	function5() {
		echo "function5"

		function1
	}

	# 函数调用
	function1

	# 外部参数传入
	function2 $1
	function3 "hello " "world!"
	function4

	# 用来接收function4的返回值
	echo $?

	function5

