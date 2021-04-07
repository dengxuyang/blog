---
title: node服务端MVC分层框架
date: 2021-03-26 09:22:59
tags:
categories:
- nodejs
---
通过node+express写一个简单的mvc分层框架，大概思路就是，通过路由分发接口到服务层，封装mysql，用class方式写router和model层。[github地址](https://github.com/dengxuyang/node-express-mvc.git)
## 1.目录结构
* router 用来分发前端传来的接口
* model 数据连接层
* config 连接数据库配置
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
在model文件夹中新建personModel.js
```js
const db = require('../config/db')
    //这里是person表的数据连接层
class personModel {
    //查找所有
    findAll() {
            return db.query('select * from tb_person', [])
        }
        //通过id查找
    findById(id) {
            return db.query('select * from tb_person where id = ?', [id])
        }
        //更新
    update() {
            //TODO
        }
        //删除
    delete() {
            //TODO
        }
        //添加
    create() {
        //TODO
    }
}
module.exports = new personModel()
```

## 4.逻辑层service
在service文件夹中创建personService.js
```js
//引入model
const personModel = require('../model/personModel')

//逻辑层
class PersonService {
    /**
     * 获取全部数据
     * @param {*} req 
     * @param {*} res 
     * @param {*} next 
     */
    async getPerson(req, res, next) {
        const { result } = await personModel.findAll()
        res.json({ err_code: 0, message: result })
    }
    /**
     * 通过id获取数据
     * @param {*} req 
     * @param {*} res 
     * @param {*} next 
     */
    async getPersonById(req, res, next) {
        const id = req.params.id
        const { result } = await personModel.findById(id)
        res.json({ err_code: 0, message: result })
    }
}


module.exports = new PersonService()
```
## 5.编写路由
路由属于数据分发层，会把前端发来的请求分发到service层做处理。在router文件夹中新建路由页person.js。
```js
const express = require('express')
const router = express.Router()
const { getPerson, getPersonById } = require('../service/personService')

class PersonContorller {
    //class 静态资源
    static initRouter() {
        router.get('/', getPerson)
        router.get('/:id', getPersonById)
        return router
    }
}
//把router暴露出去
module.exports = PersonContorller.initRouter()
```
## 6.完成
把router引入到app.js中,这样一个简单的MVC分层服务端框架就搭好了。
```js
const express = require('express')
const  app = express();
//引入路由
const personRouter = require('./router/person')





app.use('/person',personRouter)





//监听9527端口
app.listen(9527);
console.log('服务启动成功,端口:9527');
console.log('地址：http://127.0.0.1:9527');
```