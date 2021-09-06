---
title: git常用命令
date: 2021-01-26 19:44:02
tags:
categories:
- 开发工具
---
当本地或远程仓库被破坏时可强制覆盖文件  
                                              
## 1.强制拉取
```
 git fetch --all && git reset --hard origin/master && git pull
```
## 2.强制推送
```
git push -f origin master
```
## git初始化项目

### 1.初始化仓库
```
git init
```
### 2.remote
```
git remote add origin 仓库地址
```
### 3.从远程分支拉取master分支并与本地master分支合并
```
git pull origin master:master
```
### 4.提交本地分支到远程分支
```
git push -u origin master
```
### 5.将现有项目添加并提交上传
```
git add -A
git commit -m ''
git push --set-upstream origin master
```
### 6.更新远程分支列表
git remote update origin --prune

git remote update origin -p