---
title: (入门指南) 在 Google Cloud 上部署 Maven 私服
date: 2020-12-16 14:32:07 +0800
category: Android
tags: [Maven, Nexus, Docker]
excerpt: 在 Google Cloud 上使用 Docker Nexus 容器服务简单快速的部署 Maven 私服
---

### 创建服务器

首先需要注意的一点是，当你创建一个新的项目时，谷歌云默认防火墙策略是不允许有外来流量的，所以我们需要添加一些防火墙规则，开放一些端口，并应用到你的虚拟机实例配置上，这样后续我们才能访问。

这里以 Nexus 服务默认 8081 端口为例，选择我们创建的项目，然后在 `VPC 网络 -> 防火墙` 里添加一个 8081 端口的防火墙规则:

![8081防火墙规则](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main/Nexus-firewall-rule.png)

这里目标可以不用选择 `网络中的所有实例`，只需要填写指定一个目标标记，并在相应的虚拟机实例配置里指定这个目标即可。另外，可以添加默认的 http/https 端口，名称为 default-allow-http/default-allow-https 目标标记为 http-server/https-server，端口可以自己指定，比如常见默认的 80 端口。因为后续配置域名后，我们可以把服务 8081 端口映射到 80 端口，这样访问时就不需要指定端口名了，当然或者你也可以使用 Nginx 反向代理。

![所有防火墙规则](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main//Nexus-firewall-all-rule.png)

在 `Google Cloud Platform -> Compute Engine` 上创建一个虚拟机实例，因为后续我们选取使用 Docker 来部署，所以选择系统版本时，可以先参考 Docker 安装包所[提供的版本](https://docs.docker.com/engine/install/)，这里选取了当前最新的 Ubuntu Groovy 20.10。其他按需配置，再看到防火墙这个部分，当我们勾选上 `允许 HTTP 流量` 及 `允许 HTTPS 流量` 时，就会为我们添加上面配置的默认目标规则：http-server/https-server 的 80/443 端口。然后在网络部分添加上我们创建的 8081 端口规则，输入之前指定的目标标记名 nexus-server 回车即可。

![虚拟机实例防火墙规则配置](https://cdn.jsdelivr.net/gh/zoeywoohoo/image@main//Nexus-new-server.png)

在 `Compute Engine -> 设置 -> 元数据 -> SSH 密钥` 里配置好我们的 SSH 密钥，后续通过 ssh 连接服务器。

> 为了后续域名解析的方便，我们不妨在 `VPC 网络 -> 外部 IP 地址` 里给我们的虚拟机实例设置一个静态的外部 IP 地址。

### 连接服务器

使用 ssh 命令连接服务器:

```bash
$ ssh -i /path/to/your/ssh/key/.ssh/id_rsa_google_cloud [用户名]@[虚拟机实例外部 IP 地址]
```

### 安装 Docker

连上服务器后，我们就可以在终端跟着[官方教程](https://docs.docker.com/engine/install/ubuntu/)来安装 Docker 了。首先更新源及安装一些库以允许使用 HTTPS 服务：

```bash
$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

接着添加 Docker 官方的公钥：

```bash
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

搜索验证一下，会输出如下信息：

```bash
$ sudo apt-key fingerprint 0EBFCD88

pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

添加仓库源地址：

```bash
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

安装 Docker：

```bash
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```

运行一下 Hello World，验证有没有安装成功：

```bash
$ sudo docker run hello-world
```

接着可以添加 docker 用户组授予相应权限，以便后续无需输入 sudo：

```bash
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ newgrp docker
```

### 运行 Nexus 容器服务

我们使用 [Nexus 的 Docker 容器服务](https://hub.docker.com/r/sonatype/nexus3)，直接就可部署在服务器上了。为了数据的持久化存储，我们指定一个 Docker 储存盘符映射到服务器本地文件，并在运行 Nexus 服务时使用这个盘来存储我们的数据。

```bash
$ docker volume create --name nexus-data
$ docker run -d -p 80:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
```

这里我们指定了本机的 80 端口，映射到容器的 8081 端口，运行 Nexus 服务可能需要少许时间，访问我们的服务器 IP 地址，应该就能看见 Nexus 的初始页面了。这时候登录账号应该会提示你登录初始化账号名及密码的地址。

所以需要查看 Nexus 服务容器内的文件，我们需要使用以下 Docker 命令：

```bash
$ docker exec -it nexus bash
```

进入容器内部 bash 后，可以使用 cat 密码文件地址来查看初始密码，登录后要求你更新密码，输入新的密码即可。有域名的话添加一个解析到服务器的 IP 地址。It's all done!
