---
title: Docker学习笔记
tags:
---
1. 概念
- `Docker`: 与传统的虚拟化对比，不需要操作系统层面的的虚拟化，更轻量级。
- 镜像`images`: 镜像文件由若干层组成；当不同的镜像包括相同的层时，本地仅存储了层的一份内容，减小了存储空间。
- 容器`containers`: Docker容器类似与一个轻量级的沙箱，Docker利用容器来运行和隔离应用。容器是从镜像创建的应用运行实例。
2. 镜像操作
- `docker [image] pull NAME[:TAG]`:直接从`Docker Hub`镜像源来下载镜像。