# docker

- Docker 项目的目标是实现轻量级的操作系统虚拟化解决方案
- Docker Engine 是一个基于虚拟化技术的轻量级并且功能强大的开源容器引擎管理工具。它可以将不同的 work flow 组合起来构建成你的应用


## 镜像
- 一个没有任何父镜像的镜像，谓之基础镜像
- 所有镜像都是通过一个 64 位十六进制字符串 （内部是一个 256 bit 的值）来标识的。 为简化使用，前 12 个字符可以组成一个短ID，可以在命令行中使用。短ID还是有一定的 碰撞机率，所以服务器总是返回长ID。

## 获取镜像
docker pull 仓库名（centos，仓库后可加tag标记不同版本的镜像，如centos:7.2.115）

## 列出本地镜像
docker images

## 运行镜像
docker run -t -i ImageID /bin/bash

## 提交新镜像
docker commit -m '说明' -a '指定更新的用户信息' ContainerID 目标仓库/目标镜像

## Dockerfile
- 编写`Dockerfile`
```
$ mkdir sinatra
$ cd sinatra
$ touch Dockerfile

# This is a comment
FROM ubuntu:14.04
MAINTAINER Docker Newbee <newbee@docker.com>
RUN touch test
```
- 使用`docker build`来生成镜像：sudo docker build -t="ouruser/sinatra:v1" .

## 上传镜像
docker push ouruser/sinatra

## 导出镜像到本地
docker save -o sinatra.v1.tar ouruser/sinatra:v1

## 导入本地镜像
docker load --input sinatra.v1.tar


## 移除本地镜像
docker rmi ImageID

## 移除本地容器
docker rm ContainerID


## 查看日志
docker logs -f CONTAINER_ID
docker logs -f -t --since="2018-02-08" --tail=100 CONTAINER_ID


