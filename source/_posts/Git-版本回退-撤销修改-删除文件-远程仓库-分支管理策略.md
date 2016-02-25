title: Git-版本回退、撤销修改、删除文件、远程仓库、分支管理策略
date: 2015-12-10 01:39:28
categories: 
- Git
tags: 
- Git
---
## 版本回退
* 如果信息显示过多的话,可以使用`git log --pretty=oneline`

		dfffd94bebdc57c208b5a5c4594b4434bc789226 update a.txt
		9bdf0e56568f64ae6c666cd22f32ed1ca0951383 update a.txt
		2a474cb5f533fbf3bc5fd0b2fe3c856a37a9dd1e add a.txt
* 如果想回退到上个版本,可以使用`git reset --hard HEAD^`,如果想回退前2个版本`git reset --hard HEAD^^`,多个版本添加多个^。如果回退前100个版本,可以使用`git reset --hard HEAD~100`
* 如果想回退到最新版本`git reset –hard`版本号,查看最新版本版本号`git reflog`

		 9bdf0e5 HEAD@{0}: reset: moving to HEAD~2
		 5b95f49 HEAD@{1}: commit: a.txt
		 28cb7d2 HEAD@{2}: commit: a.txt
		 9bdf0e5 HEAD@{3}: reset: moving to HEAD^
		 dfffd94 HEAD@{4}: commit: update a.txt
		 9bdf0e5 HEAD@{5}: commit: update a.txt
		 2a474cb HEAD@{6}: commit (initial): add a.txt
* 执行`git reset --hard 9bdf0e5`
## 撤销修改和删除文件
### 撤销修改
* 在我未提交之前，我发现添加的内容有误，所以我得马上恢复以前的版本，现在我可以有如下几种方法可以做修改：
	* 第一：如果我知道要删掉那些内容的话，直接手动更改去掉那些需要的文件，然后add添加到暂存区，最后commit掉。
	* 第二：我可以按以前的方法直接恢复到上一个版本。使用 `git reset –hard HEAD^`
* 但是现在我不想使用上面的2种方法，我想直接想使用撤销命令该如何操作呢？首先在做撤销之前，我们可以先用`git status`查看下当前的状态。如下所示：

		AndytekiMacBook-Air:demo1 yonglang$ git status
		# On branch master
		# Changes not staged for commit:
		#   (use "git add <file>..." to update what will be committed)
		#   (use "git checkout -- <file>..." to discard changes in working directory)
		#
		#    modified:   a.txt
		#
		no changes added to commit (use "git add" and/or "git commit -a")
* 可以发现，Git会告诉你，`git checkout -- file` 可以丢弃工作区的修改，如下命令：`git checkout -- a.txt`,如下所示：

		AndytekiMacBook-Air:demo1 yonglang$ git checkout -- a.txt
		AndytekiMacBook-Air:demo1 yonglang$ 
		AndytekiMacBook-Air:demo1 yonglang$
		AndytekiMacBook-Air:demo1 yonglang$
		AndytekiMacBook-Air:demo1 yonglang$ cat a.txt
		aaaaaaaaaaaaaaa
		bbbbbbbbbbbbbbb
		ccccccccccccccc
		ddddddddddddddd
* 命令`git checkout -- a.txt`意思就是，把a.txt文件在工作区做的修改全部撤销，这里有2种情况，如下：

		- readme.txt自动修改后，还没有放到暂存区，使用 撤销修改就回到和版本库一模一样的状态。
		- 另外一种是readme.txt已经放入暂存区了，接着又作了修改，撤销修改就回到添加暂存区后的状态。
* 对于第二种情况，我想我们继续做demo来看下，假如现在我对于`readme.txt`添加一行 内容为`6666666666666`，我`git add`增加到暂存区后，接着添加内容`7777777`，我想通过撤销命令让其回到暂存区后的状态。如下所示：

		AndytekiMacBook-Air:demo1 yonglang$ vim a.txt
		AndytekiMacBook-Air:demo1 yonglang$ cat a.txt 
		aaaaaaaaaaaaaaa
		bbbbbbbbbbbbbbb
		ccccccccccccccc
		ddddddddddddddd
		eeeeeeeeeeeeeee
		AndytekiMacBook-Air:demo1 yonglang$ git add a.txt 
		AndytekiMacBook-Air:demo1 yonglang$ vim a.txt 
		AndytekiMacBook-Air:demo1 yonglang$ 
		AndytekiMacBook-Air:demo1 yonglang$ cat a.txt 
		aaaaaaaaaaaaaaa
		bbbbbbbbbbbbbbb
		ccccccccccccccc
		ddddddddddddddd
		eeeeeeeeeeeeeee
		fffffffffffffff
		AndytekiMacBook-Air:demo1 yonglang$ git checkout -- a.txt 
		AndytekiMacBook-Air:demo1 yonglang$ cat a.txt 
		aaaaaaaaaaaaaaa
		bbbbbbbbbbbbbbb
		ccccccccccccccc
		ddddddddddddddd
		eeeeeeeeeeeeeee
### 删除文件
* 假如我现在版本库目录添加一个文件`c.txt`,然后提交。如下：

		AndytekiMacBook-Air:demo1 yonglang$ git add c.txt
		AndytekiMacBook-Air:demo1 yonglang$ git commit -m '添加c.txt'
		[master 2cd7d93] 添加c.txt
		1 file changed, 0 insertions(+), 0 deletions(-)
		create mode 100644 c.txt
		AndytekiMacBook-Air:demo1 yonglang$ rm c.txt
		AndytekiMacBook-Air:demo1 yonglang$ git status
		# On branch master
		# Changes not staged for commit:
		#   (use "git add/rm <file>..." to update what will be committed)
		#   (use "git checkout -- <file>..." to discard changes in working         directory)
		#
		#    deleted:    c.txt
		#
		no changes added to commit (use "git add" and/or "git commit -a")
* 如上：一般情况下，可以直接在文件目录中把文件删了，或者使用如上`rm`命令：`rm c.txt`，如果我想彻底从版本库中删掉了此文件的话，可以再执行`commit`命令 提交掉，现在目录是这样的，

		 AndytekiMacBook-Air:demo1 yonglang$ ls -la
		total 16
		drwxr-xr-x   5 yonglang  staff  170 11  5 18:14 .
		drwxr-xr-x   3 yonglang  staff  102 11  5 17:40 ..
		drwxr-xr-x  13 yonglang  staff  442 11  5 18:14 .git
		-rw-r--r--   1 yonglang  staff   80 11  5 18:09 a.txt
		-rw-r--r--   1 yonglang  staff    6 11  5 17:56 b.txt
* 只要没有`commit`之前，如果我想在版本库中恢复此文件如何操作呢？可以使用如下命令 `git checkout -- c.txt`，如下所示：

		AndytekiMacBook-Air:demo1 yonglang$ git checkout -- c.txt
		AndytekiMacBook-Air:demo1 yonglang$ ls -la
		total 16
		drwxr-xr-x   6 yonglang  staff  204 11  5 18:18 .
		drwxr-xr-x   3 yonglang  staff  102 11  5 17:40 ..
		drwxr-xr-x  13 yonglang  staff  442 11  5 18:18 .git
		-rw-r--r--   1 yonglang  staff   80 11  5 18:09 a.txt
		-rw-r--r--   1 yonglang  staff    6 11  5 17:56 b.txt
		-rw-r--r--   1 yonglang  staff    0 11  5 18:18 c.txt
## 远程仓库
* 在`github`上创建一个`git`库，本地执行`git remote add origin http://xxxx.git`
* 将本地代码推动到远程仓库的master分支`git push -u origin master`，加`-u`首次会和远程`master`分支关联，后面再使用就不用加了。
* 创建本地分支`git checkout -b dev == git branch dev,git checkout dev`,并切换到`dev`分支,这时添加文件`p.txt`后`git add p.txt,git commit -m 'add p.txt in dev'`,切换到`master`分支再看，发现没有`p.txt`
* 合并`dev`分支到`master`分支,`git checkout master`切换到`master`分支后执行`git merge dev`
* 删除分支`git branch -d dev`
* 查看分支`git branch`
* 创建分支`git branch name`
* 切换分支`git checkout name`
* 创建+切换分支`git checkout –b name`
* 合并某分支到当前分支`git merge name`
## 分支管理策略
* 通常合并分支时，`git`一般使用`Fast forward`模式，在这种模式下，删除分支后，会丢掉分支信息，现在我们来使用带参数`–no-ff`来禁用`Fast forward`模式。首先我们来做`demo`演示下:
	* 创建一个`dev`分支。
	* 修改`readme.txt`内容。
	* 添加到暂存区。
	* 切换回主分支`(master)`。
	* 合并`dev`分支，使用命令`git merge –no-ff -m “注释” dev`
	* 查看历史记录

			AndytekiMacBook-Air:git-test yonglang$ git checkout -b dev
			Switched to a new branch 'dev'
			AndytekiMacBook-Air:git-test yonglang$ vim a.txt 
			AndytekiMacBook-Air:git-test yonglang$ git add a.txt 
			AndytekiMacBook-Air:git-test yonglang$ git commit -m 'dev分支添加'
			[dev 3cfba59] dev分支添加
			1 file changed, 1 insertion(+)
			AndytekiMacBook-Air:git-test yonglang$ git checkout master
			Switched to branch 'master'
			AndytekiMacBook-Air:git-test yonglang$ git merge --no-ff -m 'merge with no-ff' dev
			Merge made by the 'recursive' strategy.
			a.txt | 1 +
			1 file changed, 1 insertion(+)
			AndytekiMacBook-Air:git-test yonglang$ git branch -d dev
			Deleted branch dev (was 3cfba59).
			AndytekiMacBook-Air:git-test yonglang$ git branch
			* master
			AndytekiMacBook-Air:git-test yonglang$ git log --graph --pretty=oneline --abbrev-commit
			*   98a6342 merge with no-ff
			|\  
			| * 3cfba59 dev分支添加
			|/  
			* 4c3acaa dev分支添加
			*   913ac13 fix conflicts
			|\  
			| * 5138b0d 在test分支下添加hello world3
			* | 5a63888 在master分支上添加hello world4
			|/  
			* 1beff04 dev分支添加hello world2
			* 33a4fad add a.txt
	* 我们会发现被删除的分支信息还存在。
	* 分支策略：首先master主分支应该是非常稳定的，也就是用来发布新版本，一般情况下不允许在上面干活，干活一般情况下在新建的dev分支上干活，干完后，比如上要发布，或者说dev分支代码稳定后可以合并到主分支master上来。
## bug分支
* 在开发中，会经常碰到`bug`问题，那么有了`bug`就需要修复，在`Git`中，分支是很强大的，每个`bug`都可以通过一个临时分支来修复，修复完成后，合并分支，然后将临时的分支删除掉。
* 比如我在开发中接到一个`404bug`时候，我们可以创建一个`404分支`来修复它，但是，当前的`dev分支`上的工作还没有提交。比如如下：

		AndytekiMacBook-Air:git-test yonglang$ git status
		# On branch dev
		# Changes not staged for commit:
		#   (use "git add <file>..." to update what will be committed)
		#   (use "git checkout -- <file>..." to discard changes in working directory)
		#
		#    modified:   a.txt
		#
		no changes added to commit (use "git add" and/or "git commit -a")
* 并不是我不想提交，而是工作进行到一半时候，我们还无法提交，比如我这个分支`bug`要2天完成，但是我`404bug`需要5个小时内完成。怎么办呢？还好，`Git`还提供了一个`stash`功能，可以把当前工作现场 ”隐藏起来”，等以后恢复现场后继续工作。如下：

		AndytekiMacBook-Air:git-test yonglang$ git stash
		Saved working directory and index state WIP on dev: f4d6d93 merge
		HEAD is now at f4d6d93 merge
		AndytekiMacBook-Air:git-test yonglang$ 
		AndytekiMacBook-Air:git-test yonglang$ 
		AndytekiMacBook-Air:git-test yonglang$ 
		AndytekiMacBook-Air:git-test yonglang$ git status
		# On branch dev
		nothing to commit, working directory clean
* 所以现在我可以通过创建`404分支`来修复`bug`了。首先我们要确定在那个分支上修复`bug`，比如我现在是在主分支`master`上来修复的，现在我要在`master`分支上创建一个临时分支，演示如下：

		AndytekiMacBook-Air:git-test yonglang$ git checkout master
		Switched to branch 'master'
		AndytekiMacBook-Air:git-test yonglang$ git checkout -b 404
		Switched to a new branch '404'
		AndytekiMacBook-Air:git-test yonglang$ vim a.txt 
		AndytekiMacBook-Air:git-test yonglang$ cat a.txt 
		hello world
		hello world2
		hello world4
		hello world3
		hello world4
		hello world5
		hello world9
		hello world6
		hello world404
		AndytekiMacBook-Air:git-test yonglang$ git add a.txt 
		AndytekiMacBook-Air:git-test yonglang$ git commit -m 'add 404'
		[404 d1d5dac] add 404
		1 file changed, 1 insertion(+)
* 修复完成后，切换到`master`分支上，并完成合并，最后删除`404`分支。演示如下：

		 AndytekiMacBook-Air:git-test yonglang$ git checkout master
		Switched to branch 'master'
		AndytekiMacBook-Air:git-test yonglang$ git merge --no-ff -m 'merge 404' 404
		Merge made by the 'recursive' strategy.
		a.txt | 1 +
		1 file changed, 1 insertion(+)
		AndytekiMacBook-Air:git-test yonglang$ cat a.txt 
		hello world
		hello world2
		hello world4
		hello world3
		hello world4
		hello world5
		hello world9
		hello world6
		hello world404
		AndytekiMacBook-Air:git-test yonglang$ git branch -d 404
		Deleted branch 404 (was d1d5dac).
* 现在，我们回到`dev分支`上干活了。

		AndytekiMacBook-Air:git-test yonglang$ git checkout dev
		Switched to branch 'dev'
		AndytekiMacBook-Air:git-test yonglang$ git status
		# On branch dev
		nothing to commit, working directory clean
* 工作区是干净的，那么我们工作现场去哪里呢？我们可以使用命令`git stash list`来查看下。如下：

		AndytekiMacBook-Air:git-test yonglang$ git stash list
		stash@{0}: WIP on dev: f4d6d93 merge
* 工作现场还在，`Git`把`stash`内容存在某个地方了，但是需要恢复一下，可以使用如下2个方法：
	* `git stash apply`恢复，恢复后，`stash`内容并不删除，你需要使用命令`git stash drop`来删除。
	* 另一种方式是使用`git stash pop`,恢复的同时把`stash`内容也删除了。

			AndytekiMacBook-Air:git-test yonglang$ git stash apply
			# On branch dev
			# Changes not staged for commit:
			#   (use "git add <file>..." to update what will be committed)
			#   (use "git checkout -- <file>..." to discard changes in working directory)
			#
			#    modified:   a.txt
			#
			no changes added to commit (use "git add" and/or "git commit -a")
			AndytekiMacBook-Air:git-test yonglang$ cat a.txt 
			hello world
			hello world2
			hello world4
			hello world3
			hello world4
			hello world5
			hello world9
			hello world6
			hello world7
			AndytekiMacBook-Air:git-test yonglang$ git stash list
			stash@{0}: WIP on dev: f4d6d93 merge
			AndytekiMacBook-Air:git-test yonglang$ git stash drop
			Dropped refs/stash@{0} (8f8712bf68de241eca322f99e00a00219accc089)
			AndytekiMacBook-Air:git-test yonglang$ git stash list
			AndytekiMacBook-Air:git-test yonglang$
## 更改远程仓库地址

			git remote set-url origin remote_git_address
## 查看文件每一行的作者是who

			git blame [file_name]
## 检查丢失的提交
* `git fsck --lost-found`这里你可以看到一个丢失的提交。你可以通过`git show [commit_hash]`查看提交的更改或者通过运行`git merge[commit_hash]`命令进行恢复。
* `gitfsck`跟`reflog`命令相比有一个优点。假设你删除了一个远程分支，然后`clone`了这个库。用`fsck`命令你可以找到并且恢复这个删除的远程分支。


