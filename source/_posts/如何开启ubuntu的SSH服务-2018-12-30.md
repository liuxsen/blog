---
title: 如何开启ubuntu的SSH服务
date: 2018-12-30 01:53:48
tags: [linux]
---


> buntu默认并没有安装ssh服务，如果通过ssh链接ubuntu，需要自己手动安装ssh-server，然而SSH分客户端openssh-client和服务端openssh-server，openssh-client是客户端，openssh-server是服务端，

# 咱们这里讲的是安装SSH的服务端，即：openssh-server。

判断咱们的机器是否安装ssh服务，可以使用如下命令：

```
ssh localhost
ssh: connect to host localhost port 22: Connection refused
```

这个就表示没有还没有安装SSH，

+ 安装命令：sudo apt-get install openssh-server
+ 卸载命令：sudo apt-get remove openssh-server
+ 运行 sudo apt-get install openssh-server 命令之后，确认下是否安装正常

```
ps -e|grep ssh  
1418 ?        00:00:00 sshd 
```

看到sshd那说明ssh-server已经启动了

如果没有则可以这样启动：sudo /etc/init.d/ssh start 或者 service ssh start

>之前因为实验安装SSH，安装又卸载后却提示：E:unable to locate package opensshE:unable to locate package server这个是警告是说明本机没有找到openssh的安装包，之前能安装说明SSH包是存在的，而卸载后却提示找不到SSH包，说明很可能是运行卸载后，直接删除掉了，那么我们现在需要升级下sudo apt-get update，然后再安装试试还是不行。然后网上找了下说是更新了软件源，需要更新并且安装后才能使用 安装更新

**sudo apt-get dist-upgrade**