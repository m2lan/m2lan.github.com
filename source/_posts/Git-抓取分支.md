title: Git-抓取分支
date: 2015-12-10 01:31:11
categories: 
- Git
tags: 
- Git
---
* 现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：`$ git checkout -b dev origin/dev`
* 现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

		 $ git commit -m "add /usr/bin/env"
			[dev 291bea8] add /usr/bin/env
			1 file changed, 1 insertion(+)
		 $ git push origin dev
			Counting objects: 5, done.
			Delta compression using up to 4 threads.
			Compressing objects: 100% (2/2), done.
			Writing objects: 100% (3/3), 349 bytes, done.
			Total 3 (delta 0), reused 0 (delta 0)
			To git@github.com:michaelliao/learngit.git
			fc38031..291bea8  dev -> dev
* 你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

		$ git add hello.py
		$ git commit -m "add coding: utf-8"
		  [dev bd6ae48] add coding: utf-8
		   1 file changed, 1 insertion(+)
		  $ git push origin dev
		  To git@github.com:michaelliao/learngit.git
		   ! [rejected]        dev -> dev (non-fast-forward)
		  error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
		  hint: Updates were rejected because the tip of your current branch is behind
		  hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
		  hint: before pushing again.
		  hint: See the 'Note about fast-forwards' in 'git push --help' for details.
* 推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，`Git`已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

		$ git pull
		remote: Counting objects: 5, done.
		remote: Compressing objects: 100% (2/2), done.
		remote: Total 3 (delta 0), reused 3 (delta 0)
		Unpacking objects: 100% (3/3), done.
		From github.com:michaelliao/learngit
		fc38031..291bea8  dev        -> origin/dev
		There is no tracking information for the current branch.
		Please specify which branch you want to merge with.
		See git-pull(1) for details
			git pull <remote> <branch>
		If you wish to set tracking information for this branch you can do so with:
			git branch --set-upstream dev origin/<branch>
* `git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

		$ git branch --set-upstream dev origin/dev
		Branch dev set up to track remote branch dev from origin.
* 再pull：

		$ git pull
		Auto-merging hello.py
		CONFLICT (content): Merge conflict in hello.py
		Automatic merge failed; fix conflicts and then commit the result.
* 这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再`push`：

		$ git commit -m "merge & fix hello.py"
		[dev adca45d] merge & fix hello.py
		$ git push origin dev
		Counting objects: 10, done.
		Delta compression using up to 4 threads.
		Compressing objects: 100% (5/5), done.
		Writing objects: 100% (6/6), 747 bytes, done.
		Total 6 (delta 0), reused 0 (delta 0)
		To git@github.com:michaelliao/learngit.git
		291bea8..adca45d  dev -> dev
* 因此，多人协作的工作模式通常是这样：
	* 首先，可以试图用`git push origin branch-name`推送自己的修改；
	* 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
	* 如果合并有冲突，则解决冲突，并在本地提交；
	* 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！
	* 如果`git pull`提示`“no tracking information”`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。


