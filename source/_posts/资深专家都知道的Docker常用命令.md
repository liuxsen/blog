---
title: 资深专家都知道的 Docker 常用命令
date: 2019-04-04 10:15:19
tags: [docker]
---

![](/images/2019-04-04-10-24-35.png)

开发人员一直在努力提高 Docker 的使用率和性能，命令也在不停变化。Docker 命令经常被弃用，或被替换为更新且更有效的命令，本文总结了近年来资深专家最常用的命令列表并给出部分使用方法。

目前，Docker 共有 13 个管理命令和 41 个通用命令，以下是常用 Docker 命令列表：

- docker help—检查最新 Docker 可用命令；
- docker attach—将本地输入、输出、错误流附加到正在运行的容器；
- docker commit—从当前更改的容器状态创建新镜像；
- docker exec—在活动或正在运行的容器中运行命令；
- docker history—显示镜像历史记录；
- docker info—显示系统范围信息；
- docker inspect—查找有关 docker 容器和镜像的系统级信息；
- docker login—登录到本地注册表或 Docker Hub；
- docker pull—从本地注册表或 Docker Hub 中提取镜像或存储库；
- docker ps—列出容器的各种属性；
- docker restart—停止并启动容器；
- docker rm—移除容器；
- docker rmi—删除镜像；
- docker run—在隔离容器中运行命令；
- docker search—在 Docker Hub 中搜索镜像；
- docker start—启动已停止的容器；
- docker stop—停止运行容器；
- docker version—提供 docker 版本信息;

## 查找 Docker 版本和系统信息

无论是在本地还是云端，我们都需要检查 Docker 版本和系统信息，可以使用以下命令找到 Docker 版本：

```sh
> docker version
Client:
 Version:      18.03.1-ce
 API version:  1.37
 Go version:   go1.9.5
 Git commit:   9ee9f40
 Built:        Thu Apr 26 07:13:02 2018
 OS/Arch:      darwin/amd64
 Experimental: false
 Orchestrator: swarm

Server:
 Engine:
  Version:      18.03.1-ce
  API version:  1.37 (minimum version 1.12)
  Go version:   go1.9.5
  Git commit:   9ee9f40
  Built:        Thu Apr 26 07:22:38 2018
  OS/Arch:      linux/amd64
  Experimental: false
```

docker info

```sh
> docker info
Containers: 24
 Running: 2
 Paused: 0
 Stopped: 22
Images: 16
Server Version: 18.03.1-ce
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file logentries splunk syslog
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: 773c489c9c1b21a6d78b5c538cd395416ec50f88
runc version: 4fc53a81fb7c994640722ac585fa9ca548971871
init version: 949e6fa
Security Options:
 seccomp
  Profile: default
Kernel Version: 4.9.87-linuxkit-aufs
Operating System: Docker for Mac
OSType: linux
Architecture: x86_64
CPUs: 2
Total Memory: 1.952GiB
Name: linuxkit-025000000001
ID: EHVR:7DDR:U27I:5GBT:7LQZ:4H2I:F22X:QRTN:52K2:TU7P:H4P5:WUFF
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): true
 File Descriptors: 37
 Goroutines: 53
 System Time: 2019-04-04T02:33:52.270407335Z
 EventsListeners: 2
HTTP Proxy: docker.for.mac.http.internal:3128
HTTPS Proxy: docker.for.mac.http.internal:3129
Registry: https://index.docker.io/v1/
Labels:
Experimental: false
Insecure Registries:
 127.0.0.0/8
Registry Mirrors:
 http://968ada1b.m.daocloud.io/
Live Restore Enabled: false
```

> 该命令将显示部分设备信息，比如服务器版本、存储驱动程序、内核版本、操作系统、总内存等。当尝试为当前 Docker 安装启动新资源或尝试找出系统级资源分配问题时，这些信息非常有用，这也是检查运行、停止容器数量及下载镜像数量的快速方法。

搜索下载 Docker 镜像
可以使用 docker search 命令在 Docker Hub 搜索已有镜像：

![](/images/2019-04-04-10-37-03.png)

以上对 ubuntu 的搜索显示可用镜像、描述和官方状态等信息。如果要下载 ubuntu 镜像，可以使用 docker pull 命令：
![](/images/2019-04-04-10-37-36.png)

docker images 命令可详细列出拥有镜像：

![](/images/2019-04-04-10-38-22.png)

假设下载 NGINX 镜像，可以运行 docker pull 命令：

```sh
$ docker pull nginx
```

## 运行镜像的 Docker 容器

假设想在 docker 上运行 NGINX 服务器，可运行以下命令：

```sh
$ docker run -p 8080:80 nginx
```

如果已用 run 命令从 Docker Hub 提取 nginx 映像创建 NGINX 容器。 -p 8080:80 会告诉 Docker 将 localhost 端口 8080 映射到 Docker 容器端口 80，应该能够从 http:// localhost:8080 访问 NGINX 服务器。

NGINX 容器已附加到命令行。如果退出命令行，容器将会停止，可使用 detach（’- d）启动 NGINX 容器，这样即使退出命令行也可继续运行。

```sh
docker run -p 8080:80 -d nginx
```

以上命令将以分离模式启动容器并返回命令行。
![](/images/2019-04-04-14-13-10.png)

## 使用 docker ps 命令列出 Docker 容器

docker ps 命令允许查找正在运行的所有容器：
![](/images/2019-04-04-14-13-59.png)

如上显示容器的各种属性，可以看到是从 nginx 镜像创建并显示端口转发信息。CONTAINER ID 和 NAMES 属性需要特别提及，可以使用上述两大属性唯一标识容器。二者都可自动生成，但也可在容器创建过程中为容器命名。
创建一个名为 my_nginx 的 NGINX 容器：

```sh
$ docker run --name my_nginx -p 8888:80 -d nginx
```

再次列出所有 Docker 容器：

![](/images/2019-04-04-14-15-40.png)

请注意，新容器的名称为“my_nginx”。 处理大量容器时，可以使用命名约定，这可以更好得组织容器。
docker ps 命令仅显示正在运行的容器。如果对上述情况使用 docker info 命令：
![](/images/2019-04-04-14-16-35.png)

可以看到有两个容器正在运行，如果一个暂停或已停止，则不会仅使用 docker ps 命令查看这些容器，必须使用 all（ - a）选项：

```sh
docker ps -a
```

## 启动，停止，重启和杀死容器

假设要停止’my_nginx’容器，可使用 CONTAINER ID 或 NAME。

```sh
$ docker stop my_nginx
my_nginx
```

![](/images/2019-04-04-14-18-23.png)

docker start 容器

```sh
$ docker start my_nginx
my_nginx
```

![](/images/2019-04-04-14-19-46.png)

如果由于某些问题而需重启容器，则可使用 restart 命令，这比单独停止和启动容器速度更快：

```sh
$ docker restart my_nginx
```

可以像进程一样终止 docker 容器，比如终止 my_nginx 容器：

```sh
$ docker kill my_nginx
```

## Docker Exec Bash 和 Docker SSH

如果需要与 shell 进行交互以创建服务或解决问题，可以使用 docker exec 命令创建交互式 shell。比如，用 bash shell 从 ubuntu 映像启动一个容器：

```sh
$ docker run --name my_ubuntu -it ubuntu:latest bash
root@a19c770b8621:/#
```

![](/images/2019-04-04-14-23-42.png)

新开一个 shell 窗口，

![](/images/2019-04-04-14-26-29.png)

使用 docker exec 向容器发出命令。例如，可以直接用命令提示符在 my_ubuntu 容器上运行 ls 命令：

![](/images/2019-04-04-14-27-38.png)

## 以分离模式启动容器并使用 docker attach

> 以上示例用附加模式启动 ubuntu 容器，我们也可在分离模式下启动：

```
$ docker run -it -d --name my_ubuntu_2 ubuntu:latest bash
75b28b7208359137b3e1dc2843387918e28b4c6c4860ef0cdeac79c205f5cbc4
```

![](/images/2019-04-04-14-28-53.png)

使用 docker attach 命令获取 docker exec bash-like 效果：
![](/images/2019-04-04-14-29-22.png)

## 检查 Docker 镜像历史

Docker 社区镜像是分层创建的，可使用 Docker history 命令查看镜像创建方式。先看看有哪些镜像

查看 nginx 镜像历史：

![](/images/2019-04-04-14-30-24.png)

Docker 检查容器

> 可以使用 docker inspect 命令查找有关系统的信息，运行 docker ps 命令列出 Docker 容器：

![](/images/2019-04-04-14-32-53.png)

该命令将以 JSON 格式提供大量信息，以下是查找容器 IP 地址的技巧：
![](/images/2019-04-04-14-33-26.png)

## 使用 docker cp 将文件从本地复制到容器

NGINX 容器在端口 8080 上运行。因此，如果转到 http：// localhost：8080，将看到以下内容：

```sh
"Welcome to nginx!"
```

![](/images/2019-04-04-14-39-15.png)

使用带有 ls 的 docker exec 命令检查 NGINX 容器中具有 index.html 的文件夹：
![](/images/2019-04-04-14-40-34.png)

使用创建的本地文件覆盖容器 index.html 文件：

```sh
docker cp index.html my_nginx:/usr/share/nginx/html/
```

![](/images/2019-04-04-14-42-13.png)

可使用 docker cp 命令在本地计算机和创建容器间移动文件，此方法可用于覆盖配置文件。

## 创建 Docker 镜像

如果想复制更多“Hello World”容器，必须将当前容器保存为镜像。
首先，停止容器：
![](/images/2019-04-04-14-45-06.png)

使用 docker commit 命令创建新镜像：
![](/images/2019-04-04-14-46-31.png)

![](/images/2019-04-04-14-47-09.png)

如果现在检查，将看到新镜像：
![](/images/2019-04-04-14-49-53.png)

可以使用此镜像并启动新 Docker 容器，新创建容器将具有“Hello world”页面，而不是“Welcome NGINX”页面。示例如下：

![](/images/2019-04-04-14-51-55.png)

## 删除 Docker 容器和镜像

Docker 容器和镜像会占用硬盘空间，因此最好定期清理。首先停止所有容器，然后列出所有容器，使用以下 Docker 命令执行操作：

Docker 容器和镜像会占用硬盘空间，因此最好定期清理。首先停止所有容器，然后列出所有容器，使用以下 Docker 命令执行操作：

docker ps -a

docker rm container01 container02

可使用 NAMES 代替 CONTAINER ID

可使用 docker rmi 命令和 IMAGE ID 删除 docker 镜像：
