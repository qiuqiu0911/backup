---
title: linux静态路由配置
date: 2019-07-14 23:42:02
tags: 
 - 运维
 - linux
---
1. 如果要添加一条静态路由，命令为: 
`route add -net 192.56.76.0 netmask 255.255.255.0 dev eth0`
2. 在`/etc/sysconfig/static-routes`文件中添加格式为：
`any net 192.56.76.0 netmask 255.255.255.0 dev eth0`
3. 删除一条静态路由，命令为: 
`route del -net 192.56.76.0 netmask 255.255.255.0 dev eth0`