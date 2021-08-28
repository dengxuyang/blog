---
title: vue学习笔记3.0
date: 2021-08-28 20:24:03
tags:
categories:
- vue
# sticky: 9999
---
拥抱vue3 哈哈哈哈哈啊哈哈哈哈哈啊哈哈哈哈哈哈哈 学无止境啊 vue3已经发布一段时间(2020-09-18)了，之前看了看文档，现在系统的整理一下（作为一个不算太合格的前端开发者，有新东西也得适当的拥抱拥抱）。接下来开始拥抱。  
首先就是vue3与vue2相比优点是什么，做出了什么改变呢。
# vue3.0改变
## 1. 性能的提升
* 打包大小减少41%
* 初次渲染快55%，更新渲染快133%
* 内存减少54%
## 2. 源码的升级
* 使用Proxy代替defineProperty实现响应式
* 重写虚拟DOM的实现和tree-shaking
* ......
## 3. 拥抱TypeScript
* vue3.0可以更好的支持TypeScript
## 4. 新的特性
1. Composition API（组合式API）
    * setup配置
    * ref与reative
    * watch与watchEffect
    * provide与inject
    * ...... 
2. 新的内置组件
    * Fragment
    * Teleport
    * Suspense
3. 其它改变
    * 新的生命周期钩子
    * data 选项始终被声明为一个函数
    * 移除keyCode支持作为V-on的修饰符
    * ......
# 一、创建vue3.0工程
## vue-cli搭建
学过vue2的可能在熟悉不过了，vue3脚手架搭建也十分简单，不过在此之前，如果@vue/cli在4.5.0版本之前，得升级一下
```
# 升级vuecli
npm install -g @vue/cli

# 安装脚手架
vue create <项目名>

## 启动
cd <项目>
npm run serve
```
## 使用vite创建
这个玩意儿，大家可能不熟悉，没关系我也不熟悉。哈哈哈，真是听君一席话，如听一席话呀，不过可以先把他创建了，知道有这个东西，以后有时间慢慢研究嘛  
首先这个东西肯定有优点，尤大在一个演讲时候也说过，以后的项目构建会往vite的方向发展，毕竟相对于webpack而言，这个是亲儿子，不够真正开发者使用还是要等他发育一下。
* 什么是vite?---新一代前端构建工具
* 优势如下
    * 开发环境下，无需打包操作，可快速冷启动
    * 轻量快速的热重载（HMR）
    * 真正的按需编译，不在等待整个应用编译完成
  
总结一个字：快  
```
# 创建工程
npm init vite-app <项目名>

# 进入工程目录
cd <项目名>

# 安装依赖
npm i

# 启动
npm run dev
```
## 分析工程结构
这里就只分析vuecli里的结构，vite为什么不分析呢，因为我不会，课里也没讲。哈哈哈哈  
就看看main.js这个入口文件发生了什么改变
```js
// 引入的不再是Vue构造函数，而是一个叫creeateApp的工厂函数
import { createApp } from 'vue'
import App from './App.vue'
// 这样创建比Vue2的vm挂载的东西少。createApp(App)相当于vm mount是挂载
createApp(App).mount('#app')
```
* 值得注意的是vue3中template标签下不用写一个根标签了

# 二、 Composition Api---组合式Api
## 常用的Composition Api





