# docker

## 安装

>docker 支持以下版本的 Ubuntu 操作系统：
Ubuntu Hirsute 21.04
Ubuntu Groovy 20.10
Ubuntu Focal 20.04 (LTS)
Ubuntu Bionic 18.04 (LTS)
Docker 可以安装在 64 位的 x86 平台或 ARM 平台上。Ubuntu 发行版中，LTS（Long-Term-Support）长期支持版本，会获得 5 年的升级维护支持，这样的版本会更稳定，因此在生产环境中推荐使用 LTS 版本。

> Docker 支持 64 位版本 CentOS 7/8，并且要求内核版本不低于 3.10。 CentOS 7 满足最低内核的要求，但由于内核版本比较低，部分功能（如 overlay2 存储层驱动）无法使用，并且部分功能可能不太稳定

```shell
#使用脚本安装docker
sudo wget -O get-dcoker.sh 'get.docker.com' && sudo sh get-dcoker.sh --mirror Aliyun
#苏扬的版本
curl -o- https://raw.githubusercontent.com/soulteary/linux-scripts/main/docker-with-mirror.sh | bash
curl -o- https://raw.githubusercontent.com/soulteary/linux-scripts/main/docker-compose.sh | bash

#测试docker是否安装成功
sudo docker run --rm hello-world
sudo docker version #docker版本
sudo docker info #查看相关信息
sudo docker stats #docker状态
```

## docker权限
>默认情况下，docker 命令会使用 Unix socket 与 Docker 引擎通讯。而只有 root 用户和 docker 组的用户才可以访问 Docker 引擎的 Unix socket。出于安全考虑，一般 Linux 系统上不会直接使用 root 用户。因此，更好地做法是将需要使用 docker 的用户加入 docker 用户组。
```shell
#建立 docker 组：
sudo groupadd docker
#将当前用户加入 docker 组：
sudo usermod -aG docker $USER

#将当前用户添加到指定的用户组中，并刷新之(待验证)
sudo gpasswd -a $USER docker 
sudo newgrp docker
sudo chown root:docker /usr/bin/docker*
sudo chown root:docker /usr/bin/containerd*
sudo chown root:docker /usr/bin/runc
sudo chown root:docker /usr/bin/ctr

```


## docker加速
> 国内从 Docker Hub 拉取镜像有时会遇到困难，此时可以配置镜像加速器。国内很多云服务商都提供了国内加速器服务,
由于镜像服务可能出现宕机，建议同时配置多个镜像。各个镜像站测试结果请到 docker-practice/docker-registry-cn-mirror-test 查看。
国内各大云服务商（腾讯云、阿里云、百度云）均提供了 Docker 镜像加速服务，建议根据运行 Docker 的云平台选择对应的镜像加速服务

* 阿里云加速器(点击管理控制台 -> 登录账号(淘宝账号) -> 右侧镜像工具 -> 镜像加速器 -> 复制加速器地址)[https://www.aliyun.com/product/acr?source=5176.11533457&userCode=8lx5zmtu]   
* 网易云加速器[https://sf.163.com/help/documents/56918246390157312]   


## 服务管理 
```shell
sudo systemctl start docker #启动docker
sudo systemctl stop docker #关闭docker
sudo systemctl restart docker #重启docker
sudo systemctl enable docker #设置docker开机自启动
```

## 镜像
>我们都知道，操作系统分为 内核 和 用户空间。对于 Linux 而言，内核启动后，会挂载 root 文件系统为其提供用户空间支持。而 Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:18.04 就包含了完整的一套 Ubuntu 18.04 最小系统的 root 文件系统。
Docker 镜像 是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像 不包含 任何动态数据，其内容在构建之后也不会被改变。

### 下载和删除
```shell
#Docker 镜像仓库地址：地址的格式一般是 <域名/IP>[:端口号]。默认地址是 Docker Hub(docker.io)。
#仓库名：如之前所说，这里的仓库名是两段式名称，即 <用户名>/<软件名>。对于 Docker Hub，如果不给出用户名，则默认为 library，也就是官方镜像。
#docker pull 命令的输出结果最后一行给出了镜像的完整名称

#install image
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签] 
#remove image
docker rmi [imageid] 
```

## 容器
>镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
容器的实质是进程，但与直接在宿主执行的进程不同，容器进程运行于属于自己的独立的 命名空间。因此容器可以拥有自己的 root 文件系统、自己的网络配置、自己的进程空间，甚至自己的用户 ID 空间。容器内的进程是运行在一个隔离的环境里，使用起来，就好像是在一个独立于宿主的系统下操作一样。这种特性使得容器封装的应用比直接在宿主运行更加安全。也因为这种隔离的特性，很多人初学 Docker 时常常会混淆容器和虚拟机。
前面讲过镜像使用的是分层存储，容器也是如此。每一个容器运行时，是以镜像为基础层，在其上创建一个当前容器的存储层，我们可以称这个为容器运行时读写而准备的存储层为 容器存储层。
容器存储层的生存周期和容器一样，容器消亡时，容器存储层也随之消亡。因此，任何保存于容器存储层的信息都会随容器删除而丢失。
按照 Docker 最佳实践的要求，容器不应该向其存储层内写入任何数据，容器存储层要保持无状态化。所有的文件写入操作，都应该使用 数据卷（Volume）、或者 绑定宿主目录，在这些位置的读写会跳过容器存储层，直接对宿主（或网络存储）发生读写，其性能和稳定性更高。
数据卷的生存周期独立于容器，容器消亡，数据卷不会消亡。因此，使用数据卷后，容器删除或者重新运行之后，数据却不会丢失。





```shell
#docker容器操作

docker run -t -i ubuntu:18.04 /bin/bash root@af8bae53bdd3:/#
docker container start [containerid]
docker container stop [containerid]
docker restart [containerid]#重启某个容器

docker container logs #获取容器信息
docker container ls -a #查看所有的容器

docker container rm #删除容器
docker container rm -f #强行删除容器
docker container prune #删除所有未运行的容器
```


### 启动容器
> 启动容器有两种方式，一种是基于镜像新建一个容器并启动，另外一个是将在终止状态（exited）的容器重新启动。 
因为 Docker 的容器实在太轻量级了，很多时候用户都是随时删除和新创建容器。

```shell
# 启动
#-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上     
#-i 则让容器的标准输入保持打开 
docker run -t -i ubuntu:18.04 /bin/bash root@af8bae53bdd3:/#
# 启动一个停止的容器
docker container start
```

#### 守护态运行
> 更多的时候，需要让 Docker 在后台运行而不是直接把执行命令的结果输出在当前宿主机下。此时，可以通过添加 -d 参数来实现。


- 如果不使用 -d 参数运行容器,会把执行结果在前台显示
- 如果使用了 -d 参数运行容器,此时容器会在后台运行并不会把输出的结果 (STDOUT) 打印到宿主机上面(输出结果可以用 docker logs 查看)
- 注： 容器是否会长久运行，是和 docker run 指定的命令有关，和 -d 参数无关。


### 随系统启动
1. 启动时加`restart-always`
```shell
#no 不自动重启容器：(默认value)
#on-failure 容器发生error而退出(容器退出状态不为0)重启容器
#unless-stopped 在容器已经stop掉或Docker stoped/restarted的时候才重启容器
#always 在容器已经stop掉或Docker stoped/restarted的时候才重启容器
docker run -tid -name 容器id -p 端口号 -restart-always -v 挂载路径
```
2. 如果已经过运行的项目
```shell
如果已经启动的项目.则使用update更新：
docker update --restart = always 容器id
```
### 进入容器
> docker exec 后边可以跟多个参数，这里主要说明 -i -t 参数。
只用 -i 参数时，由于没有分配伪终端，界面没有我们熟悉的 Linux 命令提示符，但命令执行结果仍然可以返回。
当 -i -t 参数一起使用时，则可以看到我们熟悉的 Linux 命令提示符。
```shell
docker exec -it 69d1 bash
```

## docker-compose
### 通过脚本安装
```shell
curl -o- https://raw.githubusercontent.com/soulteary/linux-scripts/main/docker-with-mirror.sh | bash
curl -o- https://raw.githubusercontent.com/soulteary/linux-scripts/main/docker-compose.sh | bash
```
### 一般操作
```shell
#启动服务(配置文件名)
docker compose -f docker-compose-gitlab.yaml up -d
#停止服务
docker-compose -f docker-compose.yml stop
#停止并删除服务
docker-compose -f docker-compose.yml down
```
