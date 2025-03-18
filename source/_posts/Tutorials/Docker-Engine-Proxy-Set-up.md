---
title: Docker Engine Proxy Set up
mathjax: false
date: 2024-03-24 09:54:14
tags:
- Docker
- Proxy
category:
- 经验
---

再不记一些解决问题的方法，就有太多东西遗落在我的记忆中了。。。

这个 Post 是关于 Docker Engine 的代理设置，尤其是在 Build Docker Image 的时候。

<!--more-->

# 问题

昨天在使用 Docker Engine[^1] 来 Build 一个部署 React app 的 Image 的时候， `npm install` 非常不稳定，大部分时候会 Time out。我原先以为 Docker 是直接使用 Host 的网络，所以不需要设置代理，后来我在 Dockerfile 中加入了 `curl Google.com` 的命令，发现 Docker 是无法访问外网的。

[^1]: 相较于 Docker Desktop，Docker Engine 是一个更加轻量级的 Docker 环境，它不包含 GUI 界面，只有 CLI，适合在 Linux 环境下使用。

# 解决

## 桥接模式的代理

### 解决思路

如果说 Docker 不直接使用 Host 的网络，那就说明它使用桥接网络（Bridged Network）

> Basically, **bridged networking is a type of network where each container gets its own IP address and is connected to the host machine via a virtual network interface.** The Docker daemon acts as a DHCP server and assigns a dynamic IP address to each container.

因此如果我们在不改变网络模式的情况下，需要打开 Host 代理的 `allow LAN` 选项，然后设置 Image 中的代理环境变量为 Host 的代理地址。

### 修改 Clash 配置以及确认 Proxy Port

我使用的 Proxy Client 是 [`clash-for-linux-backup`](https://github.com/Elegycloud/clash-for-linux-backup), 其配置文件为 `./conf/config.yaml`。

在这份配置文件中，注意开头几行：
```yaml
# HTTP 代理端口
port: 7890

# SOCKS5 代理端口
socks-port: 7891

# Linux 和 macOS 的 redir 代理端口
redir-port: 7892

# 允许局域网的连接
allow-lan: true
```

首先确认 `allow-lan` 选项为 `true`，然后确认 `port` 和 `socks-port` 的值，这两个端口号是我们需要设置的代理端口。

### 设置 Docker Image 的代理环境变量

在 Dockerfile 中加入以下命令：
```Dockerfile
ENV http_proxy http://172.17.0.1:7890
ENV https_proxy http://172.17.0.1:7890
ENV all_proxy=socks5://172.17.0.1:7891
ENV no_proxy localhost, # 这里可以接着加入其他地址，用逗号隔开即可
ENV HTTP_PROXY http://172.17.0.1:7890
ENV HTTPS_PROXY http://172.17.0.1:7890
ENV ALL_PROXY=socks5://172.17.0.1:7891
ENV NO_PROXY localhost, # 这里可以接着加入其他地址，用逗号隔开即可
```
这里面的地址 `172.17.0.1` 是 Docker Engine 的默认网关地址，我们可以通过 `docker network inspect bridge` ，或者 `ip -4 addr show docker0 | grep -Po 'inet \K[\d.]+'` 来查看。

现在在 Build 的过程中，Docker 就能够使用 Host 的代理服务了。

如果一些包管理器不会直接使用环境变量设置的代理地址，就需要我们在 Dockerfile 中手动设置代理地址，比如 `npm`：
```Dockerfile
RUN npm config set proxy $HTTP_PROXY
RUN npm config set https-proxy $HTTPS_PROXY
```

### 自动化进阶处理

以上的手动配置讲解已经大致解释了设置代理的基本原理。根据这个基本原理我们可以通过 Dockerfile 的 `ARG` 来设置自动化的代理设置。便与快速在不同的 Host 上部署。


添加以下代码到未配置代理的 Dockerfile 中：
```Dockerfile
ARG HOST_IP
ARG HTTP_PORT
ARG SOCKS_PORT

ENV http_proxy http://${HOST_IP}:${HTTP_PORT}
ENV https_proxy http://${HOST_IP}:${HTTP_PORT}
ENV all_proxy=http://${HOST_IP}:${SOCKS_PORT}
ENV no_proxy localhost, 
ENV HTTP_PROXY http://${HOST_IP}:${HTTP_PORT}
ENV HTTPS_PROXY http://${HOST_IP}:${HTTP_PORT}
ENV ALL_PROXY=http://${HOST_IP}:${SOCKS_PORT}
ENV NO_PROXY localhost, 
```

在 Biuld 的时候使用以下命令：

```shell
docker build --build-arg HOST_IP=$(ip -4 addr show docker0 | grep -Po 'inet \K[\d.]+') --build-arg HTTP_PORT=$(echo $http_proxy | sed -n 's/.*:\([0-9]\+\).*/\1/p') --build-arg SOCKS_PORT=$(echo $all_proxy | sed -n 's/.*:\([0-9]\+\).*/\1/p') -t your-image-name .
```
其中 `ip -4 addr show docker0 | grep -Po 'inet \K[\d.]+'` 帮我们获取了 Docker Engine 的网关地址，`echo $http_proxy | sed -n 's/.*:\([0-9]\+\).*/\1/p'` 从 Host 的 `http_proxy` 环境变量中提取出 HTTP 代理端口，`echo $all_proxy | sed -n 's/.*:\([0-9]\+\).*/\1/p'` 从 Host 的 `all_proxy` 环境变量中提取出 SOCKS 代理端口。

**请注意，命令的具体写法取决于 Host 代理配置的具体情况。例如你的 all-proxy 使用的是 HTTP 代理，或 FTP 代理。请理解命令的原理再决定在你的环境上要怎么写。**

## Host 模式的代理

### 解决思路

事实上 Docker 只是默认使用桥接网络，它还有别的网络模式。

我们可以通过 `-network host` 参数来使用让 Docker 直接使用 Host 的网络。

因此只需要配置好 Host 的代理服务之后，进行 Build 的时候添加 `-network host` 参数即可。

例如：
```shell
docker build -t my-image --network host .
```