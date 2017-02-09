# 目录

- [关于Docker](#关于Docker)
- [Docker 架构](#Docker 架构)
- [起航](#起航)
- [其他常用命令](#其他常用命令)
- [DOCKER集群](#DOCKER集群)

### SECTION 1
# 关于Docker

一夜之间，Docker俨然成为开发，打包，部署应用的一种既成标准。为DevOps提供了便捷快速启动轻量级虚拟机环境-容器，容器包含了应用本身与之所依赖的一切，
这种轻量级的虚拟机易于测试，也方便运维人员的部署发布。

Docker让自动化的基础设施，应用隔离，保持一致性，提高资源利用都变的更加简单。

与广受欢迎的源码控制工具Git相似，Docker也有其协作性，开发者和运维人员都可以通过 [Docker Hub](https://hub.docker.com/) 分享镜像。

Docker是开源的解决方案，不仅仅可以运行在Linux上，也可以在 Windows 和 Mac 使用轻量级Linux发行版或者VirtualBox来运行。Docker也复杂的应用为提供了众多的管理与编排工具。

---
### SECTION 2
# Docker 架构

Docker使用client-server的架构，使用远程API在Linux容器之上来管理，创建Docker容器。Docker容器使用Docker镜像创建。容器和镜像的关系非常像 面向对象语言中  Class 和 Object的概念。

![docker architecture](https://dzone.com/storage/temp/576507-docker1.png)

- Docker Images(镜像)
创建容器的模版，包含了安装步骤和运行所必须的软件。
- Docker Container(容器)
像是小型的虚拟机，从镜像中创建而来。
- Docker Client(客户端)
命令行工具，或者一系列和Docker守护进程交互的[API](https://docs.docker.com/reference/api/docker_remote_api)
- Docker Host(主机)
运行Docker daemon的物理机或者是虚拟机，包含已缓存的镜像已经在运行的容器。
- Docker Registry(镜像仓库)
用来创建docker容器的镜像仓库，[Docker Hub](https://hub.docker.com) 是最受欢迎的Docker镜像仓库。
- Docker Machine(集群)
用来管理多个Docker主机的工具，可以运行在本地的VirtualBox中也可以在各种云平台中。

---

### SECTION 3

# 起航
### 安装 Docker

对于Mac和Windows用户来说，安装已经不能再简单了。
需要下载 Docker Toolbox 在 https://www.docker.com/toolbox。
此安装包包含Docker Client, Docker Machine, Compose (Mac only) 和 VirtualBox。

因为Docker基于Linux容器技术，所以不能够直接运行在Mac 和 Windows上，VirtualBox使用一个小型Linux内核来运行Docker服务。

于此同时，在Linux安装Docker可能就是没有这么容易，为了安装Docker在Linux上，我们必须要安装一些依赖。从 https://docs.docker.com/installation 文中我们可以查看详细，对于某些发行版可能在包管理中已经有Docker,对于其他的发行版通用的方法是：

```
curl -sSL https://get.docker.com/ | sh
```

在Linux上安装Docker-Machine需要root权限，使用以下命令：

```
curl -L https://github.com/docker/machine/releases/download/v0.4.0/docker-machine_linux-amd64 > /usr/local/bin/docker-machine
chmod +x /usr/local/bin/docker-machine
```

截止至此文发布，Docker-Machine仍在beta中不建议在生产环节中使用。


### 运行容器

Docker已经安装好，我们开始运行容器，如果你没有必要的镜像来创建容器，Docker会从Hub仓库中下载镜像，然后组建并运行。  
为了运行最简单的 hello-world 容器，来证明我们的环境已经安装完毕，我们执行以下命令：

```
docker run hello-world
```
最终这个命令将打印一系列消息在标准输出上。

### 典型工作流

Docker对于创建镜像，推送镜像，发布镜像，运行容器有一套典型的工作流。

![Workflow](https://dzone.com/storage/temp/576516-docker2.png)

Docker构建镜像的典型方式是从Dockerfile，Dockerfile是一系列指令包含如何配置容器或者从Docker仓库中拉取镜像。当Docker 镜像在你的Docker环境中，那你就可以运行容器了，运行容器创建运行时操作系统，软件，和一些配置。比如你可以Debian操作系统上运行Mysql 5.5. 并且为你的应用创建指定的数据库，用户和表结构。这些可运行的容器能够象一个虚拟机一样启动和关闭。如果你手动的配置或者手动安装软件，容器能够通过提交的方式创建一个新的镜像再下一次使用。当然，你也可以通过Docker Hub 仓库分享你的镜像。

### 从Docker仓库拉取镜像


你可以通过访问 https://hub.docker.com 查找是否已经你所需要的镜像。仓库内有许多已经被认证的官方镜像，比如 MySQL, Node.js, Java, Nginx, WordPress 当然也有数以万记个人用户创建的镜像，如果你找到你需要的镜像，那就将之拉取下来。

```
docker pull mysql
```
如果你本地还没有缓存这些镜像，Docker会自动从Hub上下载当前最新版本并缓存之，如果你想要指定某个特殊版本，可以使用以下命令：
```
docker pull mysql:5.5.45
```
如果你并不想立即运行镜像，那这个命令将比Run节约一步，并且会在后台自动下载镜像。


### 从Dockerfile构建一个镜像


如果你没有找到任何你需要或者信任的镜像在DockerHub上，你可以通过创建Dokcerfile构建自己的镜像。Dockerfiles可以从已有的镜像中继续编写，从而添加你需要的软件或者自定义配置。

下面就是一个简单的 Dokcerfile 例子：

```
FROM mysql:5.5.45
RUN echo America/New_York | tee /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata
```

这个Dockerfile展示了如何从官方mysql仓库（指定的  5.5.45 版本）创建一个 时区为 America/New_York的新镜像。

关于 Dockerfile 详细的介绍，我们在后面再谈。

为了在目录内构建我们的镜像，我们需要执行：

```
docker build .
```
这个命令将创建一个未命名的镜像，我们可以通过下面这个命令查看我们的镜像列表：

```
docker images
```
这个命令列出我们本地所有缓存的镜像，包括我们自己创建的那个。


```
REPOSITORY  TAG     IMAGE ID      VIRTUAL SIZE
<none>      <none>  4b9b8b27fb42  214.4 MB
mysql       5.5.45  0da0b10c6fd8  213.5 MB
```

如上所示，我们build命令创建一个 REPOSITORY 和  TAG 都是 none 的镜像，这种方式并不推荐，更建议使用下面这种方式，使用 -t 为镜像打上标签：

```
docker build –t est-mysql .

REPOSITORY   TAG     IMAGE ID      VIRTUAL SIZE
est-mysql    latest  4b9b8b27fb42  214.4 MB
mysql        5.5.45  0da0b10c6fd8  213.5 MB
```
这是用还有另外一种创建镜像的方式，我们可以通过一个已经存在的镜像，通过Bash命令安装我们的自己的软件或者更改配置，最后使用Docker Commit为正在运行的容器创建一个新的镜像。不过这种方式并非是最佳实践，因为不能够重复而且文档也是一种自解释。

# 运行镜像

为了运行这个镜像，我们首先需要的在本地找到此镜像的缓存或者从Docker Hub找到。通常 Docker镜像总会需要一些额外的环境配置，我们通过 -e 的方式传入，而一切需要象守护进程需要长期运行的需要 -d 选项。 
为了运行我们 est-mysql 镜像，我们需要配置 Mysql root用户密码，我们可以从  Docker Hub 中Mysql文档里查看到具体的。

```
docker run -e +1 -d est-mysql
```
查看运行中的容器，我们使用ps命令：
```
docker ps
```
ps命令将列出所有运行的镜像，包括从什么镜像创建，哪些命令在运行，哪些端口正在被监听，还有这个容器的名字。
	CONTAINER ID    IMAGE     COMMAND      
	30645f307114    est-mysql “/entrypoint.sh mysql”
	PORTS       NAMES
	3306/tcp    serene_brahmagupta
从上面的执行进程结果来看，容器名称为serene_brahmagupta。这是一个自动生成的名字，其维护便利性受到质疑。因此明确的指定容器名称被认为是一种更好的方式，通常使用`--name`选项在容器启动的时候提供其名称。

	docker run --name my-est-mysql -e MYSQL_ROOT_ PASSWORD=root+1 -d est-mysql

你会注意到ps命令的输出内容中该容器监听端口3306，但是这并不意味着你能够在本地运用MySQL的命令行或者MySQL的工作台直接和数据库进行交互了，因为该端口只能在运行它的安全的docker环境中才能够使用。为了使得该端口在该环境外部可用，你需要使用`-p`选项将端口映射出去。

	docker run --name my-est-mysql -e MYSQL_ROOT_ PASSWORD=root+1 -p 3306:3306 -d est-mysql
	
现在你可以连接到MySQL正在监听的端口了，但是你仍然需要知道你要连接的IP地址。为了明确IP你可以使用`docker- machine ip`命令去查询。

	docker-machine ip default
使用default作为machine的名称，默认的machine安装自Docker Toolbox，你将会获得运行该容器的machine的IP地址。利用这个IP地址，你就能够利用本地MySQL命令行去链接MySQL了。

	mysql -h 192.168.99.100 -u root -proot+1
### 暂停和启动容器
现在你有一个运行着的容器了，接下来你可以使用Docker的`stop`命令和容器名称去暂停它。

	docker stop my-est-mysql
容器的全部状态都记录在磁盘上，如果你想由关闭状态再次开启它，你可以使用start命令。
	
	docker start my-est-mysql

### 标记镜像
现在你有一个执行过且被验证过的镜像，在上传到镜像仓库之前，建议使用用户名，镜像名和版本号作为其标签。你可以使用Docker的`tag`命令来实现这一点。

	docker tag est-mysql javajudd/est-mysql:1.0

### 提交镜像到仓库
最后，你已经准备好将你的镜像提交到Docker Hub供世界范围内的开发者使用或者提交到私有仓库供你的团队成员内部使用。
首先，你需要前往https://hub.docker.com/ 注册一个免费的账号，接着利用`login`命令登入。

	docker login 
接着输入用户名，密码和你注册时候的电子邮箱。
然后就可以利用`push`命令推送镜像了，同时指定你的用户名，镜像名和版本号。

	docker push javajudd/est-mysql:1.0
不久你就会收到一封邮件，说明镜像已经成功提交到仓库了。这时如果你从网页登录Docker Hub，你会看到这个新的仓库。

---
### SECTION 4
# 其他常用命令
### 查询容器
你已经看到ps命令能够查看正在运行的容器，但是如果要无视状态查看全部的容器呢，这时候就需要添加 `-a`选项了。

	docker ps -a
列出全部容器之后，你就能够确定启动或者移除哪一个。
### 移除容器
当你使用完一个容器之后，相比于让它留在那里占据磁盘空间，不如将其移除，你可以利用`rm`命令。

	docker rm my-est-mysql
### 移除镜像
你已经了解如何列出所有本地缓存的镜像，这些镜像会占据很大的空间，几MB到几百MB不等，因此你会需要使用`rmi`命令去清除不需要的镜像。

	docker rmi est-mysql
在创建镜像的DEBUG过程中，通常会产生大量的不理想或者标记为<none>的未命名镜像。你可以使用下面的命令来删除这些虚悬的镜像。

	docker rmi $(docker images -q -f dangling=true)
### 查询端口
查看容器暴露的端口也是常用的需求，例如端口3306常用来链接MySQL数据库或者端口80常用来链接网站服务器。`port`命令能够用来展示暴露的端口号。

	docker port my-est-mysql
### 查询进程
如果你想查看容器中正在运行的进程情况，你可以使用`top`命令（和Linux中的top命令相似）

	docker top my-est-mysql
### 执行命令
你能够通过`exec`命令在一个运行的容器内部执行指令。例如为了列出硬盘驱动器根目录，你可以使用下面的命令。

	docker exec my-est-mysql ls /
如果你想在容器中进行root认证，仍然可以使用等效的`exec`命令去开启使用bash shell的权限。同时，因为Docker client和Docker daemon之间的通信都是加密的，所以安全性也得到了保证。

	docker exec -it my-est-mysql bash
### 启动容器
`run`命令是最复杂且最有特色的Docker命令。它能够用来管理网络配置，系统资源（例如容量，CPU，文件系统），配置安全等。访问https://docs.docker.com/reference/run/查看完整的可用选项。

## DOCKERFILE
前文已经介绍，Dockerfile是首选的创建Docker镜像的方式。它包括一些说明，例如安装和配置软件的Linux命令。`build`命令能够查阅你的路径或者类似GitHub仓库的URL下的Dockerfile。和Dockerfile一起，任何同一文件夹和子文件夹下的内容都会被视为构造过程的一部分。如果你希望将执行脚本或者对于部署有用的文件一同构造进去的时候会非常有用。
### HOT TIP
如果你希望在包含的路径中排除部分文件或者目录，可以使用`.dockerignore file`来实现。

## 指令
指令会按照Dockerfile中的顺序被执行。Dockerfile同样可以包含以字符#开头的行命令，下表包含了一些可用的命令：

命令 | 描述
--- | ---
FROM | 这条指令必须位于Dockerfile的第一行，它指定了基础镜像
MAINTAINER |  指定了镜像作者的信息
RUN |  执行一条linux命令用于配置和安装
ENTRYPOINT |  启动容器的脚本或应用(最后执行)，它可以让容器变成可执行的应用
CMD |  以JSON数组的格式为ENTRYPOINT提供默认参数
LABEL |  镜像元信息，以键值对的形式
ENV |  设置环境变量
COPY |  将文件拷贝至容器内
ADD |  COPY的替代选择
WORKDIR |  为RUN,ENTRYPOINT,CMD,COPY,和（或）ADD指令设置工作目录
EXPOSR |  容器将要监听的端口
VOLUME |  创建一个挂载点
USER |  运行RUN，CMD，和（或）ENTRYPOINT指令的用户

## DOCKERFILE 示例
这是MySQL 5.5官方版的一个示例Dockerfile,使用了许多可用的命令，你可以在 https://github.com/docker-library/mysql/blob/5836bc9af9deb67b68c32bebad09a0f7513da36e/5.5/找到它。

```bash
FROM debian:jessie

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN groupadd -r mysql && useradd -r -g mysql mysql

RUN mkdir /docker-entrypoint-initdb.d

# FATAL ERROR: please install the following Perl modules before executing /usr/local/mysql/scripts/mysql_install_db:
# File::Basename
# File::Copy
# Sys::Hostname
# Data::Dumper
RUN apt-get update && apt-get install -y perl --no-install-recommends && rm -rf /var/lib/apt/lists/*

# mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory
RUN apt-get update && apt-get install -y libaio1 && rm -rf /var/lib/apt/lists/*

# gpg: key 5072E1F5: public key "MySQL Release Engineering <mysql-build@oss.oracle.com>" imported
RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys A4A9406876FCBD3C456770C88C718D3B5072E1F5

ENV MYSQL_MAJOR 5.5
ENV MYSQL_VERSION 5.5.45

# note: we're pulling the *.asc file from mysql.he.net instead of dev.mysql.com because the official mirror 404s that file for whatever reason - maybe it's at a different path?
RUN apt-get update && apt-get install -y curl --no-install-recommends && rm -rf /var/lib/apt/lists/* \
	&& curl -SL "http://dev.mysql.com/get/Downloads/MySQL-$MYSQL_MAJOR/mysql-$MYSQL_VERSION-linux2.6-x86_64.tar.gz" -o mysql.tar.gz \
	&& curl -SL "http://mysql.he.net/Downloads/MySQL-$MYSQL_MAJOR/mysql-$MYSQL_VERSION-linux2.6-x86_64.tar.gz.asc" -o mysql.tar.gz.asc \
	&& apt-get purge -y --auto-remove curl \
	&& gpg --verify mysql.tar.gz.asc \
	&& mkdir /usr/local/mysql \
	&& tar -xzf mysql.tar.gz -C /usr/local/mysql --strip-components=1 \
	&& rm mysql.tar.gz* \
	&& rm -rf /usr/local/mysql/mysql-test /usr/local/mysql/sql-bench \
	&& rm -rf /usr/local/mysql/bin/*-debug /usr/local/mysql/bin/*_embedded \
	&& find /usr/local/mysql -type f -name "*.a" -delete \
	&& apt-get update && apt-get install -y binutils && rm -rf /var/lib/apt/lists/* \
	&& { find /usr/local/mysql -type f -executable -exec strip --strip-all '{}' + || true; } \
	&& apt-get purge -y --auto-remove binutils
ENV PATH $PATH:/usr/local/mysql/bin:/usr/local/mysql/scripts

# replicate some of the way the APT package configuration works
# this is only for 5.5 since it doesn't have an APT repo, and will go away when 5.5 does
RUN mkdir -p /etc/mysql/conf.d \
	&& { \
		echo '[mysqld]'; \
		echo 'skip-host-cache'; \
		echo 'skip-name-resolve'; \
		echo 'user = mysql'; \
		echo 'datadir = /var/lib/mysql'; \
		echo '!includedir /etc/mysql/conf.d/'; \
	} > /etc/mysql/my.cnf

VOLUME /var/lib/mysql

COPY docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

EXPOSE 3306
CMD ["mysqld"]

```
这个示例的Dockerfile会产生以下动作：

* 继承一个已存在的名叫debian:jessie的Debian镜像
* 使用RUN指令去配置镜像：增加用户组，创建目录以及使用Debian的包管理工具apt-get安装必要的软件
* 运行gpg对PGP进行加密
* 使用ENV指令去设置该镜像所代表的MYSQL的最高和最低版本
* 运行一长行命令去安装和配置系统，后面紧接着另外一个环境变量去设置系统PATH
* 使用RUN指令去创建配置文件
* 使用VOLUME命令去映射一个文件系统
* 使用COPY命令去复制和重命名一个脚本，它将在容器启动的时候运行。ENTRYPOINT指定了该脚本。
* 使用EXPOSE指令声明端口3306作为MYSQL的标准端口
* 使用CMD指令去指定容器启动时传给ENTRYPOINT的命令行参数，字符串mysqld

---

### SECTION 5

# DOCKER集群

Docker Machine是另外一个命令行工具用以管理一个或多个本地或远程服务器。本地机器通常运行在不同的VirtualBox实例中。远程机器通常运行在诸如Amazon Web Services (AWS), Digital Ocean, or Microsoft Azure提供的云服务器中。

### 创建本地 Machine
安装Docker Toolbox的同时，你将会得到一个名叫"default"的默认Docker Machine。它很容易上手，但在一定程度上，你需要多个 Machine分割你将要运行的不同容器。你可以使用``docker-machine create``命令：
```bash
docker-machine create -d virtualbox qa
```
它将使用一个名叫qa的VirtualBox镜像去新建一个本地Machine

### 列出 MACHINES
如果你需要查看你所配置的所有machine，可以使用``docker-machine ls``命令：
```bash
docker-machine ls
```

### 启动和停止 Machine
Docker Machines可以用``docker-machine start``启动：
```bash
docker-machine start qa
```
一旦machine启动起来了，你需要去配置docker命令行，以便和docker守护进程进行交互。你可以使用``docker-machine env``命令进行此动作并使用eval对其求值。
```bash
docker-machine env qa
eval “$(docker-machine env qa)”
```
使用``docker-machine stop``去停止machine。

### HOT TIP
docker-machine start 和 stop命令实际上会启动或停止VirtualBox VMs。如果你开启了VirtualBox管理器，你可以在运行上述命令的时候看到VM的变化。


> 原文 [Getting Started With Docker](https://dzone.com/refcardz/getting-started-with-docker-1)

**译者：** 
- [misha913loki](https://github.com/misha913loki)
- [renlulu](https://github.com/renlulu) 
- [yannxia](https://github.com/yannxia)