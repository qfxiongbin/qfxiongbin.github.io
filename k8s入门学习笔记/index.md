# Kubernetes入门实战课学习笔记01


<!--more-->

现在 Kubernetes 已经没有了实际意义上的竞争对手，它的地位就如同 Linux 一样，成为了事实上的云原生操作系统，是构建现代应用的基石。

## Kubernetes技术栈的四个特点
* 技术新
* 领域广
* 实现杂
* 方向深

学习Kubernetes最好的方式 **建立一个全局观和大局观，了解全貌，再选择自己感谢兴趣的方向去研究。**

## 课程选择的版本 
2022年发布的Kubernetes 1.23.3

## 学习地图
![](/images/k8smap.webp)

## 学习环境

课程中推荐使用VirtualBox，我目前还有一台阿里云的云服务器，就不费劲本地弄虚拟机了，直接在云服务器上面开始了。
写blog笔记和实际操作练习都是在vscode中进行。操作云服务器用的是Remote-SSH插件。

## Docker

学习这门课之前，我已经有一定的docker 方面的基础了。安装docker相关的步骤直接跳过。这是我云服务器上面之前安装的docker版本信息

```
[root@iZ8vb53kklhrg4j6sviqtpZ ~]# docker version
Client:
 Version:         1.13.1
 API version:     1.26
 Package version: docker-1.13.1-208.git7d71120.el7_9.x86_64
 Go version:      go1.10.3
 Git commit:      7d71120/1.13.1
 Built:           Mon Jun  7 15:36:09 2021
 OS/Arch:         linux/amd64

Server:
 Version:         1.13.1
 API version:     1.26 (minimum version 1.12)
 Package version: docker-1.13.1-208.git7d71120.el7_9.x86_64
 Go version:      go1.10.3
 Git commit:      7d71120/1.13.1
 Built:           Mon Jun  7 15:36:09 2021
 OS/Arch:         linux/amd64
 Experimental:    false

```

### docker 使用

* docker ps
列出系统中运行的容器，与Linux中的 ps命令类似。

* 拉取image
拉取busybox image
```
docker pull busybox

[root@iZ8vb53kklhrg4j6sviqtpZ ~]# docker pull busybox
Using default tag: latest
Trying to pull repository docker.io/library/busybox ... 
latest: Pulling from docker.io/library/busybox
729ce43e2c91: Pull complete 
Digest: sha256:ad9bd57a3a57cc95515c537b89aaa69d83a6df54c4050fcf2b41ad367bec0cd5
Status: Downloaded newer image for docker.io/busybox:latest

```

* 列出本机所存储的所有镜像

```
docker images

[root@iZ8vb53kklhrg4j6sviqtpZ ~]# docker images
REPOSITORY                                          TAG                 IMAGE ID            CREATED             SIZE
docker.io/busybox                                   latest              2bd29714875d        4 days ago          1.24 MB
registry.cn-shenzhen.aliyuncs.com/star7th/showdoc   latest              ffe481c085e9        9 months ago        602 MB
star7th/showdoc                                     latest              ffe481c085e9        9 months ago        602 MB
docker.io/rabbitmq                                  3-management        c91dff94df48        9 months ago        253 MB
docker.io/rabbitmq                                  3.9-management      c91dff94df48        9 months ago        253 MB
docker.io/rabbitmq                                  latest              c10b0f4fd126        9 months ago        220 MB
docker.io/jenkinsci/jenkins                         lts                 d7c5abfe8477        3 years ago         703 MB
registry.aliyuncs.com/helowin/oracle_11g            latest              3fa112fd3642        6 years ago         6.85 GB
```

* 通过 echo输出 hello world

```
[root@iZ8vb53kklhrg4j6sviqtpZ ~]# docker run busybox echo hello wrold
hello wrold
```
### docker 架构

![](/images/dockerar.webp)

上面的docker 相关的使用都是客户端中的行为，这些命令会与Docker Engine的后台服务 Docker daemon通信，客户端通过 build、pull、run等命令给Docker daemon发送请求，“大管家” Docker daemon负责从远端拉取镜像、本地存储镜像、从镜像生成容器和管理容器等功能。

C/S架构实现了功能分离，利于分布式应用，客户端与服务端进行了解耦。
