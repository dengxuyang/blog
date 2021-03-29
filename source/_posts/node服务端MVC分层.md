---
title: node服务端MVC分层框架
date: 2021-03-26 09:22:59
tags:
categories:
- nodejs
---
通过node+express写一个简单的mvc分层框架，大概思路就是，通过路由分发接口到服务层，封装mysql，用class方式写router和model层。
## 1.目录结构
* router 用来分发前端传来的接口
* model 数据连接层
* cinfig 连接数据库配置
* service 业务逻辑层
* app.js 项目入口
## 2.项目初始化
1. 项目初始化
```
npm init
```
2. 安装express
```
npm install express
```
3. 安装mysql
```
npn install mysql
```
## 3.封装mysql
在config文件夹中新建db.js文件
```js
class DB {
    constructor() {
        this.mysql = require('mysql')
    }
    query(sql, params) {
        return new Promise((resolve, reject) => {
            //创建连接
            const connection = this.mysql.createConnection({
                    host: 'localhost',
                    user: 'root',
                    password: '123456',
                    database: 'test'
                })
                //2.开启连接
            connection.connect(err => {
                    if (err) {
                        console.log('连接数据库失败');
                        reject(err)
                    }
                    console.log('连接数据库成功');
                })
                //3.执行sql查询语句
            connection.query(sql, params, (err, result, fileds) => {
                    if (err) {
                        console.log('数据库查询失败');
                        reject(err)
                    }
                    resolve({
                        result,
                        fileds
                    })
                })
                //4.关闭连接
            connection.end(err => {
                if (err) {
                    console.log('关闭数据库失败');
                    reject(err)
                }
                console.log('关闭数据库成功');
            })
        })
    }
}
module.exports = new DB()
```
## 4.用class编写model层
在model文件夹中新建personModel
