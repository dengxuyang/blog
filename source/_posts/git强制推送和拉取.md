---
title: git常用命令
date: 2021-01-26 19:44:02
tags:
categories:
- 开发工具
---
当本地或远程仓库被破坏时可强制覆盖文件  
                                              
强制拉取
```
 git fetch --all && git reset --hard origin/master && git pull
```
强制推送
```
git push -f origin master
```
初始化仓库
```
git init
```