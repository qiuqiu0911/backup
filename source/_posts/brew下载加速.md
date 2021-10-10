---
title: brew下载加速
date: 2020-01-06 21:57:09
tags:
- macOS
---
**Homebrew**是一款Mac OS平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。使用`brew`安装软件时, 可以将brew的仓库访问地址改成`alibaba`的地址来加速

1. #### 将brew访问的仓库地址改成aliyun镜像
- 替换brew.git 仓库地址
```shell
cd "$(brew --repo)" 
git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git 
```
- 替换homebrew-core.git仓库地址
```shell
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core" 
git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git 
```
- 替换homebrew-bottles访问地址
```shell
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.zshrc
```

2. #### 如需换回原来官方的地址，同样修改以上三处
- 替换brew.git 仓库地址
```shell
cd "$(brew --repo)" 
git remote set-url origin https://github.com/Homebrew/brew.git
```
- 替换homebrew-core.git仓库地址
```shell
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```
- 替换homebrew-bottles访问地址
`vi ~/.zshrc` 删除`HOMEBREW_BOTTLE_DOMAIN`所在的行