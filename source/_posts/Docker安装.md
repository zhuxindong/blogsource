---
abbrlink: ''
categories: []
date: '2022-02-05 15:20:53'
tags:
- Docker
title: Docker安装等
updated: Mon, 10 Apr 2023 09:22:12 GMT
---
Docker安装

# 一、docker 安装

## 1.windows

1. 安装／升级Docker客户端
   推荐安装1.10.0以上版本的Docker客户端，参考文档 docker-ce
2. 配置镜像加速器
   针对Docker客户端版本大于 1.10.0 的用户

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://kgrlk8c1.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 2.ubuntu

一键安装

```shell
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```

ps:网络不好的话可以考虑阿里云镜像

```shell
curl -fsSL [https://get.docker.com](https://get.docker.com) | bash -s docker --mirror Aliyun
```

配置阿里云加速

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://kgrlk8c1.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
