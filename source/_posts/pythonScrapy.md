---
title: python scrapy框架学习
date: 2021-02-23 17:20:27
tags: python
---
## 安装
首先我是在Windows平台，进行学习，所以安装和之后发生的问题和解决都是基于Windows，之后可能会在linux，但不是现在，主要记录学习的过程，总结经验。好，接下来，干！
### 1. 安装python
就直接莽官网<http://python.org/download>下载2.7版本，这里我按照scrapy文档上添加环境变量，发生了一点问题，安装后查看版本号没有反应，后手动解决，执行如下命令行。
```
set PATH=C:\Python27;%PATH%
```
* Python 2.7.9 + 或 Python 3.4+ 以上版本都自带 pip 工具无需手动安装
### 2. 安装pywin32
安装地址<https://github.com/mhammond/pywin32/releases>找到自己的系统版本和对应的python版本。
### 3. 安装Scrapy
```
pip install Scrapy
```
### 4.爬虫
首先新建个文件夹执行
```
scrapy startproject mySpider
```
这样一个scrapy的框架就建好了


