---
title: python scrapy框架学习
date: 2021-02-23 17:20:27
tags: python
categories:
- python
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
## 爬虫
### 1.新建爬虫项目
首先新建个文件夹执行
```
scrapy startproject mySpider
```
这样一个scrapy的框架就建好了

### 2.创建一个爬虫
* 首先需要在items.py文件里创建写一个存放抓取数据的容器，就是item
```py
import scrapy

class ItcastItem(scrapy.Item):
   name = scrapy.Field()
   title = scrapy.Field()
   info = scrapy.Field()
```
* 新建一个python文件，如果是2.x的版本，需要注意编码在头部加上
``` py
# -*- coding: utf-8 -*-
```
我按照菜鸟教程抓取的传智播客老师的信息，代码是直接搬运的[原地址](https://www.runoob.com/w3cnote/scrapy-detail.html)，代码如下
```py
# -*- coding: utf-8 -*-
import scrapy
from testspider.items import ItcastItem

# 以下三行是在 Python2.x版本中解决乱码问题，Python3.x 版本的可以去掉
import sys
reload(sys)
sys.setdefaultencoding("utf-8")

class Opp2Spider(scrapy.Spider):
    name = 'itcast'
    allowed_domains = ['itcast.com']
    start_urls = ("http://www.itcast.cn/channel/teacher.shtml",)

    def parse(self, response):
        #open("teacher.html","wb").write(response.body).close()

        # 存放老师信息的集合
        items = []

        for each in response.xpath("//div[@class='li_txt']"):
            # 将我们得到的数据封装到一个 `ItcastItem` 对象
            item = ItcastItem()
            #extract()方法返回的都是unicode字符串
            name = each.xpath("h3/text()").extract()
            title = each.xpath("h4/text()").extract()
            info = each.xpath("p/text()").extract()

            #xpath返回的是包含一个元素的列表
            item['name'] = name[0]
            item['title'] = title[0]
            item['info'] = info[0]

            items.append(item)

        # 直接返回最后数据
        return items
```
* 保存数据  
在命令行中运行
```
scrapy crawl itcast -o teachers.json
```
这样一个简单的爬虫就写好了
#### 持续更新中。。。。