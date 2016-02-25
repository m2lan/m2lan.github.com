title: Shell-for-while
date: 2015-12-10 02:19:59
categories: Shell
tags: Shell
---

	#!/bin/bash
	# for 

	arr=(111 2222 3333 4444 5555)
	num=5
	count=5

	# for--in--
	for m in ${arr[@]}; do
		echo $m
	done

	echo "------------------------------------------"

	for n in $( ls /usr/local ); do
		echo $n
	done

	echo "------------------------------------------"

	# while
	while [[ $num -gt 0 ]]; do
		echo "num: ${num}"
		num=$(($num - 1))
	done

	num=5

	echo "------------------------------------------"
	# while --- continue
	while [[ $num -gt 0 ]]; do
		num=$(($num - 1))
		if [[ $num -eq 3 ]]; then
			continue
		fi
		echo "num: ${num}"
	done

	echo "------------------------------------------"
	# while --- break
	while [[ $count -gt 0 ]]; do
		count=$(($count - 1))
		if [[ $count -eq 3 ]]; then
			break;
		fi
		echo "count: ${count}"
	done


