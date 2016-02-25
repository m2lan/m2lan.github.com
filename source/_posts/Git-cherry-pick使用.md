title: Git-cherry-pick使用
date: 2015-12-10 01:29:15
categories: 
- Git
tags: 
- Git
---
* `git cherry-pick`用于把另一个本地分支的`commit`修改应用到当前分支。
* 在本地 `master` 分支上做了一个`commit( 38361a68138140827b31b72f8bbfd88b3705d77a )`,如何把它放到 本地 `old_cc` 分支上？
* 办法之一： 使用 `cherry-pick`. 根据git 文档： `Apply the changes introduced by some existing commits`
* 就是对已经存在的`commit`进行`apply`(可以理解为再次提交)
* 简单用法：`git cherry-pick <commit id>`
* 例如：

		$ git checkout old_cc
		$ git cherry-pick 38361a68


