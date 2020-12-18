## Docker核心概念

### 1、镜像

定义:镜像是一个只读的文件和文件夹组合;其中包含了容器运行时所需要的所有基础文件和配置信息,是容器运行的基础;

如果希望获取镜像,有两种方式可以获取:    
1)、自己创建镜像:一般情况下一个镜像是基于一个基础镜像构建的,我们可以在基础镜像上添加自定义的内容;   
2)、从功能镜像仓库拉取已经制作好的镜像:一些常用的软件或系统都会有官方已经制作好的镜像可供直接使用(nginx,centos,mysql);

### 2、容器

定义:容器是镜像的运行实体;镜像是静态的只读文件,而容器带有运行时需要的可写文件层,且容器中进行处于运行状态;容器自身生命周期有创建、运行、停止、暂停及删除等状态;

容器本质上是主机上运行的一个进程,不过容器有自己的命名空间隔离和资源限制;也就是说容器内部无法看到主机上的进程、环境变量、网络等信息,这也是容器与主机上直接运行的进程的本质区别;

### 3、仓库

定义:仓库是用于存储和分发Docker镜像的介质;镜像仓库存在公共镜像仓库集私有镜像仓库;

Docker官方的公共镜像仓库为[DockerHub](https://hub.docker.com/),在DockerHub上不仅存在官方制作的镜像,还有很多组织或者个人开发的镜像供免费使用;



## Docker架构

技术背景:容器技术一开始不是仅有Docker参与其中,比如CoreOS的rkt、lxc等也有在做,后来Docker一枝独秀成为容器技术的事实标准,但是业界对于容器的标准技术标准并没有确定;事实上不仅仅有容器标准之争,还存在编排技术之争,其中三大主力分别是Docker Swarm、Kubernetes和Mesos,最终胜出的就是现在常听说的Kubernetes了,也正是在这种情况下,OCI应运而生;

OCI(Open Container Initiative)开放容器标准,是一个轻量级、开放的治理结构;OCI组织在Linux基金会的支持下,于2015年6月份正式注册成立;基金会主要围绕工业化容器的标准和镜像运行时指定一个开放的容器标准;目前主要有两个标准文档:**容器运行时标准**(runtime spec)和**容器镜像标准**(image spec);

Docker整体架构采用c/s模式,主要由客户端和服务端组成;客户端负责发送操作指定,服务端负责接收和处理指令;客户端和服务端相互之间通信有多种方式,既可以在同一台机器上通过UNIX套接字通信,也可以通过网络进行远程通信;

## Docker客户端

Docker客户端是一种泛称,可以作为Docker客户端与服务端交互的可以是使用最多的docker命令,也可以是通过rest api交互,还可以通过提供的多种语言的sdk进行交互(目前存在go/java/python/php等语言)；

## Docker服务端

Docker服务端是Docker所有后台服务的统称,其中dockerd是一个非常重要的后台管理进程,主要负责响应和处理来自Docker客户端的请求,然后将客户端的请求转化为Docker的具体操作;   
比如针对镜像、容器、网络和挂载卷等对象的操作和管理;

随着Docker的发展,服务端经历过多次重构,刚开始服务端组件全部集成在dockerd二进制里;不过从1.11版本开始,dockerd分离出独立二进制,而容器也不再直接由dockerd启动,而是集成了containerd、runC等多个组件;

## Docker重要组件

测试环境使用的docker版本信息

```docker
Version:           18.09.2
API version:       1.39
Go version:        go1.10.8
Git commit:        6247962
Built:             Sun Feb 10 04:12:39 2019
OS/Arch:           darwin/amd64
Experimental:      false
```

当前版本下与docker存在至关重要的组件:runC和containerd

* runC是Docker官方按照OCI容器运行时标准的一个实现,通俗来说runC是一个用来运行容器的轻量级工具,是真正用来运行容器的;
* containerd是Docker服务端的核心组件,从原本的dockerd中剥离出来,它的诞生遵循OCI标准,是容器标准化后的产物;containerd通过container-shim启动并管理runC,可以认为containerd真正管理了容器的生命周期;

## Docker各组件之间的关系

当dockerd启动的时候,containerd就随之启动了,dockerd与containerd一直存在;当执行docker run命令时,containerd会创建containerd-shim充当中间进程,然后启动容器的真正进程;


### 更多信息链接

<https://hub.docker.com/>   
<https://docs.docker.com/get-started/>
