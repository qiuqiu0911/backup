---
title: Docker环境下部署MySQL
tags:
  - Docker
date: 2020-05-01 08:27:05
---

1. #### 拉取镜像
``` shell
 docker pull mysql:5.7.29
```
2. #### 创建本地目录
```shell
mkdir -p ~/dev/mysql/{log,data,conf}
```
3. #### 创建容器，建立目录映射并启动
```shell
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root \
-v ~/dev/mysql/log:/var/log/mysql -v ~/dev/mysql/data:/var/lib/mysql \
-v ~/dev/mysql/conf:/etc/mysql mysql:5.7.29
```