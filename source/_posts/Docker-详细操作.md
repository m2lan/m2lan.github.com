title: Docker-详细操作
date: 2015-12-09 23:48:47
categories: 
- Docker
tags: 
- Docker
---
## 获取镜像

	docker pull ubuntu:14.04
	相当于docker pull registry.hub.docker.com/ubuntu:14.04
	可以从国内镜像网站下载docker pull dl.dockerpool.com:5000/ubuntu:14.04
## 列出本地已有的镜像

	docker images
## 创建镜像，利用Dockerfile来创建镜像,新建一个目录和一个Dockerfile

	$ mkdir sinatra
	$ cd sinatra
	$ touch Dockerfile
* Dockerfile 基本的语法是
	* 编写完成Dockerfile后可以使用docker build来生成镜像

			docker build -t="ouruser/sinatra:v2" .
	* 使用`#`来注释
	* `FROM`指令告诉`Docker`使用哪个镜像作为基础
	* 接着是维护者的信息
	* `RUN`开头的指令会在创建中运行，比如安装一个软件包，在这里使用`apt-get`来安装了一些软件
	* 其中`-t`标记来添加`tag`，指定新的镜像的用户信息。 “.” 是`Dockerfile`所在的路径（当前目录），也可以替换为一个具体的`Dockerfile`的路径。
	* `docker run -t -i ouruser/sinatra:v2 /bin/bash`运行
	* `Dockerfile`文件内容

			# This is a comment
			  FROM ubuntu:14.04
			  MAINTAINER Docker Newbee <newbee@docker.com>
			  RUN apt-get -qq update
			  RUN apt-get -qqy install ruby ruby-dev
			  RUN gem install sinatra
* 如果要导出镜像到本地文件，可以使用`docker save`命令。

		docker save -o ubuntu_14.04.tar ubuntu:14.04
* 载入镜像,可以使用`docker load`从导出的本地文件中再导入到本地镜像库，例如

		docker load --input ubuntu_14.04.tar
		或
		docker load < ubuntu_14.04.tar
* 输出`hello world`

		docker run ubuntu:14.04 /bin/echo 'hello world'
* 启动一个`bash`终端，允许用户进行交互

		docker run -t -i ubuntu:14.04 /bin/bash
* 让`Docker`容器在后台以守护状态形式运行，可以通过通过`-d`参数来实现

		docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
	* `docker ps`返回容器信息，包括`NAMES`
	* 获取容器的输出信息，可以通过`docker logs`命令

			docker logs NAMES
* 获取终止状态的容器`docker ps -a`,终止状态的容器可以通过`docker start`命令来重新启动，`docker restart`命令会将一个运行态的容器终止，然后再重新启动。`docker restart NAMES`。
* 在使用`-d`参数时，容器启动后会进入后台。 某些时候需要进入容器进行操作，有很多种方法，包括使用`docker attach`命令或`nsenter`工具等。
	* 如果出现`2014/11/10 17:46:08 You cannot attach to a stopped container, start it first`,执行`docker start berserk_fermat`
	* attach

			docker run -idt ubuntu:14.04
			[root@MyCloudServer ~]# docker run -idt ubuntu:12.04
			e13112150bd52dbb12d7fbbe0e24e786ba86e97a2e337db654d0ec144d6b88f4
			[root@MyCloudServer ~]# docker ps -a
			CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                     PORTS               NAMES
			e13112150bd5        ubuntu:12.04        "/bin/bash"            15 seconds ago      Up 14 seconds                                  berserk_fermat
			005c6adf29a4        ubuntu:12.04        "/bin/echo 'hello wo   3 minutes ago       Exited (0) 3 minutes ago                       lonely_leakey
			f4d208e3e8e6        ubuntu:12.04        "/bin/bash"            37 minutes ago      Up 4 minutes                                   jovial_rosalind
			[root@MyCloudServer ~]# docker attach berserk_fermat
			root@e13112150bd5:/# ls
			bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  selinux  srv  sys  tmp  usr  var
			root@e13112150bd5:/# pwd
			/
			root@e13112150bd5:/# exit
			exit
## 导出和导入容器
### 导出容器
* 如果要导出本地某个容器，可以使用`docker export`命令.

		[root@MyCloudServer ~]# docker ps -a
		CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                      PORTS               NAMES
		e13112150bd5        ubuntu:12.04        "/bin/bash"            13 minutes ago      Exited (0) 11 minutes ago                       berserk_fermat
		005c6adf29a4        ubuntu:12.04        "/bin/echo 'hello wo   17 minutes ago      Exited (0) 16 minutes ago                       lonely_leakey
		f4d208e3e8e6        ubuntu:12.04        "/bin/bash"            50 minutes ago      Up 17 minutes                                   jovial_rosalind
		[root@MyCloudServer ~]# docker export e13112150bd5>ubuntu.tar
### 导入容器快照
* 可以使用`docker import`从容器快照文件中再导入为镜像，例如

		$ cat ubuntu.tar | sudo docker import - test/buntu:v1.0
		$ sudo docker images
		REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
		test/ubuntu         v1.0                9d37a6082e97        About a minute ago   171.3 MB
## 删除镜像
* `docker ps -a`列出所有依赖`Image`的`container`
* `docker rm (CONTAINER ID前4位)`执行删除容器操作
* `docker images`列出所有镜像
* `docker rmi (image id前4位)`
* 如果要删除一个运行中的容器，可以添加`-f`参数。`Docker`会发送`SIGKILL`信号给容器。


