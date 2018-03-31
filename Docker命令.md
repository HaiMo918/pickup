# Docker

## Docker Container

### Docker Hello World

	docker run ubuntu:latest /bin/echo "Hello world"
	
各个参数解析：

* __docker__: Docker 的二进制执行文件。

* __run__:与前面的 docker 组合来运行一个容器。

* __ubuntu__:15.10指定要运行的镜像，Docker首先从本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库 Docker Hub 下载公共镜像。

* __/bin/echo "Hello world"__: 在启动的容器里执行的命令

以上命令完整的意思可以解释为：Docker 以 ubuntu15.10 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello world"，然后输出结果。

### 运行交互式容器

	docker run -i -t ubuntu:latest /bin/bash
	
各个参数解析：

* __-t__:在新容器内指定一个伪终端或终端。

* __-i__:允许你对容器内的标准输入 (STDIN) 进行交互。

### 启动容器（后台模式）

	docker run -d ubuntu:latest /bin/sh -c "while true; do echo hello world; sleep 1; done"
	
### 查看运行容器

	docker ps
	docker ps -as
	
### 停止容器

	docker stop container_name/container_id
	
### 运行一个web应用

	docker pull training/webapp
	docker run -d -P training/webapp python app.py
	
参数说明:

* __-d__:让容器在后台运行。

* __-P__:将容器内部使用的网络端口随机映射到主机的高端口。

查看正在运行的容器

```
docker ps
```
 
以下端口信息

	PORTS
	0.0.0.0:32768->5000/tcp
	
Docker 开放了 5000 端口（默认 Python Flask 端口）映射到主机端口 32768 上。

可以用地浏览器访问127.0.0.1:32768

也可以指定 -p 标识来绑定指定端口

	docker run -d -p 本机映射端口:容器映射端口 training/webapp python app.py
	
* 上面的例子中，默认都是绑定 tcp 端口，如果要绑定 UDP 端口，可以在端口后面加上 /udp。
	
使用docker port可以查看容器端口的映射情况
 
 	docker port contianer_id/container_name

### 查看web应用程序日志

	docker logs -f contianer_id/contianer_name
	
参数说明:
	
* __-f__:让 dokcer logs 像使用 tail -f 一样来输出容器内部的标准输出

### 查看web应用容器的进程

	docker top container_id/container_name
	
### 查看容器底层状态

	docker inspect container_id/container_name
	
### 启动容器

对于停止的容器
	
	docker start container_id/container_name
	
对于启动的容器

	docker restart container_id/container_name
	
### 移除容器

	docker rm container_id/container_name
	
* 删除容器时，容器必须是停止状态

## Docker Images

### 列出镜像列表

	docker images
	
### 获取一个新的镜像

	docker pull image:tag
	
### 查找镜像

	docker search image
	
### 创建镜像

当我们从docker镜像仓库中下载的镜像不能满足我们的需求时，我们可以通过以下两种方式对镜像进行更改。

1. 从已经创建的容器中更新镜像，并且提交这个镜像
2. 使用 Dockerfile 指令来创建一个新的镜像

#### 更新镜像

变更镜像

	docerk run -t -i ubuntu:latest /bin/bash
	apt-get update
	exit

通过命令 docker commit来提交容器副本。

	docker commit -m="comment" -a="author" container_id/container_name newbuntu:v2
	
#### 构建镜像

我们使用命令 docker build ， 从零开始来创建一个新的镜像。为此，我们需要创建一个 Dockerfile 文件，其中包含一组指令来告诉 Docker 如何构建我们的镜像。

Dockerfile:

	FROM    centos:6.7
	MAINTAINER      Fisher "fisher@sudops.com"

	RUN     /bin/echo 'root:123456' |chpasswd
	RUN     useradd runoob
	RUN     /bin/echo 'runoob:123456' |chpasswd
	RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
	EXPOSE  22
	EXPOSE  80
	CMD     /usr/sbin/sshd -D
	
通过Dockerfile构建镜像

	docker build -t centos:6.7 .
	
参数说明：

* __-t__ ：指定要创建的目标镜像名

* __.__ ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径

Docker Client会默认发送Dockerfile同级目录下的所有文件到Dockerdaemon中。

解决办法有两种：

1. 使用.dockerignore文件，设置黑名单，该文件包含的目录不会被发送到Docker daemon中

2. 将Dockerfile迁移后其他目录中执行。

#### 设置标签镜像

使用 docker tag 命令，为镜像添加一个新的标签。

	docker tag container_id/container_name centos:dev