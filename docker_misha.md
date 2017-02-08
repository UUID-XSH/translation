为了查看运行的容器，你可以使用`ps`命令：

	docker ps

`ps`命令能够列出所有正在运行的进程，以及创建它的镜像的名称，正在运行的指令，软件监听的端口和容器名称。

	CONTAINER ID    IMAGE     COMMAND      
	30645f307114    est-mysql “/entrypoint.sh mysql”	PORTS       NAMES
	3306/tcp    serene_brahmagupta从上面的执行进程结果来看，容器名称为serene_brahmagupta。这是一个自动生成的名字，其维护便利性受到质疑。因此明确的指定容器名称被认为是一种更好的方式，通常使用`--name`选项在容器启动的时候提供其名称。

	docker run --name my-est-mysql -e MYSQL_ROOT_ 
	↳PASSWORD=root+1 -d est-mysql你会注意到ps命令的输出内容中该容器监听端口3306，但是这并不意味着你能够在本地运用MySQL的命令行或者MySQL的工作台直接和数据库进行交互了，因为该端口只能在运行它的安全的docker环境中才能够使用。为了使得该端口在该环境外部可用，你需要使用`-p`选项将端口映射出去。

	docker run --name my-est-mysql -e MYSQL_ROOT_ 
	↳PASSWORD=root+1 -p 3306:3306 -d est-mysql
	
现在你可以连接到MySQL正在监听的端口了，但是你仍然需要知道你要连接的IP地址。为了明确IP你可以使用`docker- machine ip`命令去查询。

	docker-machine ip default
使用default作为machine的名称，默认的machine安装自Docker Toolbox，你将会获得运行该容器的machine的IP地址。利用这个IP地址，你就能够利用本地MySQL命令行去链接MySQL了。

	mysql -h 192.168.99.100 -u root -proot+1##暂停和启动容器
现在你有一个运行着的容器了，接下来你可以使用Docker的`stop`命令和容器名称去暂停它。

	docker stop my-est-mysql
容器的全部状态都记录在磁盘上，如果你想由关闭状态再次开启它，你可以使用start命令。
	
	docker start my-est-mysql
##标记镜像
现在你有一个执行过且被验证过的镜像，在上传到镜像仓库之前，建议使用用户名，镜像名和版本号作为其标签。你可以使用Docker的`tag`命令来实现这一点。

	docker tag est-mysql javajudd/est-mysql:1.0
##提交镜像到仓库
最后，你已经准备好将你的镜像提交到Docker Hub供世界范围内的开发者使用或者提交到私有仓库供你的团队成员内部使用。
首先，你需要前往https://hub.docker.com/ 注册一个免费的账号，接着利用`login`命令登入。

	docker login 
接着输入用户名，密码和你注册时候的电子邮箱。
然后就可以利用`push`命令推送镜像了，同时指定你的用户名，镜像名和版本号。

	docker push javajudd/est-mysql:1.0
不久你就会收到一封邮件，说明镜像已经成功提交到仓库了。这时如果你从网页登录Docker Hub，你会看到这个新的仓库。
##其他常用命令
###查询容器
你已经看到ps命令能够查看正在运行的容器，但是如果要无视状态查看全部的容器呢，这时候就需要添加 `-a`选项了。

	docker ps -a
列出全部容器之后，你就能够确定启动或者移除哪一个。
###移除容器
当你使用完一个容器之后，相比于让它留在那里占据磁盘空间，不如将其移除，你可以利用`rm`命令。

	docker rm my-est-mysql
###移除镜像
你已经了解如何列出所有本地缓存的镜像，这些镜像会占据很大的空间，几MB到几百MB不等，因此你会需要使用`rmi`命令去清除不需要的镜像。

	docker rmi est-mysql
在创建镜像的DEBUG过程中，通常会产生大量的不理想或者标记为<none>的未命名镜像。你可以使用下面的命令来删除这些虚悬的镜像。

	docker rmi $(docker images -q -f dangling=true)
###查询端口
查看容器暴露的端口也是常用的需求，例如端口3306常用来链接MySQL数据库或者端口80常用来链接网站服务器。`port`命令能够用来展示暴露的端口号。

	docker port my-est-mysql
###查询进程
如果你想查看容器中正在运行的进程情况，你可以使用`top`命令（和Linux中的top命令相似）

	docker top my-est-mysql
###执行命令
你能够通过`exec`命令在一个运行的容器内部执行指令。例如为了列出硬盘驱动器根目录，你可以使用下面的命令。

	docker exec my-est-mysql ls /
如果你想在容器中进行root认证，仍然可以使用等效的`exec`命令去开启使用bash shell的权限。同时，因为Docker client和Docker daemon之间的通信都是加密的，所以安全性也得到了保证。

	docker exec -it my-est-mysql bash
###启动容器
`run`命令是最复杂且最有特色的Docker命令。它能够用来管理网络配置，系统资源（例如容量，CPU，文件系统），配置安全等。访问https://docs.docker.com/reference/run/查看完整的可用选项。
##DOCKERFILE
前文已经介绍，Dockerfile是首选的创建Docker镜像的方式。它包括一些说明，例如安装和配置软件的Linux命令。`build`命令能够查阅你的路径或者类似GitHub仓库的URL下的Dockerfile。和Dockerfile一起，任何同一文件夹和子文件夹下的内容都会被视为构造过程的一部分。如果你希望将执行脚本或者对于部署有用的文件一同构造进去的时候会非常有用。
###HOT TIP
如果你希望在包含的路径中排除部分文件或者目录，可以使用`.dockerignore file`来实现。

##说明

