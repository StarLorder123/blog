---
title: Docker使用指南
description: 如何顺畅地、熟练地使用Docker
keywords: 'Docker,使用指导'
top_img: /HowToUseDocker/1-1.jpg
cover: /HowToUseDocker/1-1.jpg
tags:
  - Operation Guide
categories:
  - 代码浪潮
abbrlink: 978e37b0
date: 2024-10-09 22:39:07
---
# 安装
## 在Debian下安装Docker

- 步骤 1：更新系统
- 首先，我们需要确保系统是最新的，以获取最新的软件包和安全更新。

```shell
sudo apt update
sudo apt upgrade
```
- 步骤 2：安装必要的软件包
    - 我们需要安装一些软件包，以便apt可以通过HTTPS使用存储库。

```shell
sudo apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common
```
- 步骤 3：添加Docker的官方GPG密钥
    - 为了验证从Docker下载的软件包的真实性，我们需要添加Docker的官方GPG密钥。

```shell
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
- 步骤 4：添加Docker的APT存储库
    - 现在，我们将添加Docker的APT存储库，以便apt可以从中获取Docker软件包。

```shell
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
- 步骤 5：更新apt存储库
    - 我们需要更新apt存储库，以便它包含Docker存储库中的软件包。

```shell
sudo apt update
```
- 步骤 6：安装Docker Engine
    - 现在，我们可以使用apt安装Docker Engine。

```shell
sudo apt install docker-ce docker-ce-cli containerd.io
```
- 步骤 7：验证安装
    - 让我们验证一下Docker是否成功安装。这将下载一个简单的Docker映像并在容器中运行它。如果一切正常，您将看到一条消息，确认Docker已正确安装。

```shell
sudo docker run hello-world
```
- 步骤 8：（可选）将用户添加到docker组
- 为了避免每次运行Docker命令时都需要使用sudo，您可以将当前用户添加到docker组中。

```shell
sudo usermod -aG docker $USER
```
- 请注意，添加用户到docker组后，需要重新登录才能使更改生效。

# 常用命令
- 查看所有容器

```shell
docker ps -a
```
- 启动一个已停止的容器

```shell
docker start [container-name|container-id]
```
- 停止一个容器

```shell
docker stop [container-name|container-id]
```
- 重启一个运行的容器

```shell
docker restart [container-name|container-id]
```
- 删除一个停止的容器。删除容器需要先停止容器

```shell
docker rm [container-name|container-id]
```
- 查看容器的日志
    - --tail [int] 显示日志的最后n行
    - -f 跟随显示日志

```shell
docker logs [--tail number] [-f] [container-name|container-id]
```