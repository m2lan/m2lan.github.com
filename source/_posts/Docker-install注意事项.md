title: Docker-install注意事项
date: 2015-12-09 23:41:55
categories: Docker
tags: Docker
---
* 当安装一个不存在的package时`yum -y install docker-io`,使用

		yum install epel-release
* 当出现以下错误时

		Loaded plugins: fastestmirror
		Loading mirror speeds from cached hostfile
		Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again
* 运行`yum update --disablerepo=epel\*`
* 这时再运行`yum -y install docker-io`
* 现在，它已经安装好了，让我们来启动 Docker 守护进程`service docker install`
* 如果我们需要开机自启动，如下：`chkconfig docker on`
* 现在我们确认 Docker 是否正常工作，首先我们需要获取最新的 CentOS 镜像`docker pull centos-latest`
* 下一步，我们要确保通过如下命令可以看到镜像：`docker images centos`
* 它将会输出如下的信息：

		[root@MyCloudServer ~]# docker images centos
		  REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
		  centos              latest              ae0c2d0bdc10        6 days ago          224 M
* 运行简单的`bash shell`来测试这个镜像`docker run -i -t centos /bin/bash`
* 如果正常，你会获得一个简单的`bash`提示，输入`exit`退出。


