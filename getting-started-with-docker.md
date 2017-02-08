---
title: getting-started-with-docker
date: 2017-02-07 15:33:44
tags:
---


### SECTION 1
#关于Docker

一夜之间，Docker俨然成为开发，打包，部署应用的一种既成标准。为DevOps提供了便捷快速启动轻量级虚拟机环境-容器，容器包含了应用本身与之所依赖的一切，
这种轻量级的虚拟机易于测试，也方便运维人员的部署发布。

Docker让自动化的基础设施，应用隔离，保持一致性，提高资源利用都变的更加简单。

与广受欢迎的源码控制工具Git相似，Docker也有其协作性，开发者和运维人员都可以通过 [Docker Hub](https://hub.docker.com/) 分享镜像。

Docker是开源的解决方案，不仅仅可以运行在Linux上，也可以在 Windows 和 Mac 使用轻量级Linux发行版或者VirtualBox来运行。Docker也复杂的应用为提供了众多的管理与编排工具。


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


### SECTION 3

# 从零开始

### 安装 Docker

对于Mac和Windows用户来说，安装可能有些麻烦。
需要下载 Docker Toolbox 在 https://www.docker.com/toolbox。
此安装包包含Docker Client, Docker Machine, Compose (Mac only) 和 VirtualBox。

因为Docker基于Linux容器技术，所以不能够直接运行在Mac 和 Windows上，VirtualBox使用一个小型Linux内核容器来运行Docker服务。
