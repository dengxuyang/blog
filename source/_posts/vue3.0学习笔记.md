---
title: vue3.0学习笔记
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
首先这个东西肯定有优点，尤大在一个演讲时候也说过，以后的项目构建会往vite的方向发展，毕竟相对于webpack而言，这个是亲儿子，不过真正开发者使用还是要等他发育一下。
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
### 1. setup
1. vue中的新配置项，值为一个函数
2. setup是所有Composition Api"表演的舞台"
3. 组件中所有要用到的数据、方法等等，均要配置在setup中
4. setup函数有两个返回值：
    * 返回一个对象，对象中的属性或方法在模板中均可使用。
    * 返回一个render函数
5. 注意点：
    1. 尽量不要与vue2.x配置混用
        * vue2.x配置中可以访问到setup中的属性、方法、
        * setup中不行
        * 如果有重名 setup优先
    2. setup不能用async修饰，因为返回值return的是一个promise
### 2. ref函数
1. ref定义一个响应式的数据
2. 语法：`const xxx=ref(value)`
    * 创建一个包含响应式的引用对象
    * js中操作数据xxx.value
    * 模板中读取数据不需要.value 直接 ``<div>{{xxx}}</div>``
3. 备注：
    * 接收数据可以是：基本类型，也可以是对象类型
    * 基本类型的数据：响应式依然是靠`Object.defineProperty`的`get`与`set`完成的。
    * 对象数据类型：内部求助了Vue3.0中的一个新函数--`reactive`函数
### 3. reactive函数
* 作用：定义一个<font color=#ff0000><b>对象类型</b></font>的响应式数据（基本类型别用它，用ref函数）
* 语法：`const 代理对象=reactive(被代理的对象)`接收一个对象或数组,返回一个<font color=#ff0000><b>代理器对象（proxy对象）</b></font>
* reactive定义的响应式是深层次的。
* 内部基于ES6的Proxy实现，通过代理对象操作源对象内部数据都是响应式的。
### 4. vue3.0的响应式原理
* 实现原理：
    * 通过Proxy（代理）：拦截对象中任意属性的变化，包括：属性的添加、属性的删除等。
    * 通过Reflect（反射）：对被代理对象的属性进行操作
```js
new Proxy(data,{
    //拦截读取属性
    get(target,prop){
        return Reflect.get(target,prop)
    },
    //拦截设置或添加新属性
    set(target,prop,value){
        return Reflect.set(target,prop,value)
    },
    deleteProperty(target,prop){
        return Reflect.deleteProperty(target,prop)
    }
})
```
### 5.reactive与ref的对比
* 从定义角度对比：
    * ref定义基本类型数据
    * reactive定义对象或数组类型数据
    * 备注：`ref`也可以用来定义对象或数组类型数据，他内部会自动通过`reactive`转为代理对象
* 从原理角度对比：
    * ref通过`Object.defineproperty()`的get与set来实现响应式（数据劫持）
    * reactive 通过使用Proxy构造函数实现响应式，并通过Reflect操作源对象数据。
* 从使用角度对比：
    * ref定义的数据：操作数据需要`.value`，读取数据时模板中直接读取不需要`.value`。
    * reactive定义的数据：操作和读取均不需要`.value`

### 6.steup的两个注意点
* setup的执行时机
    * 在beforeCreate之前执行一次，this是undefined。
* setup的参数
    * props:值为对象，包含：组件外部传递过来，且组件内部接收了的属性。
    * context：上下文对象
        * attrs：值为对象，包含：组件外部传递过来，但没有在props中声明的属性，相当于`this.$attrs`。
        * slots：收到的插槽内容，相当于`this.slots`。
        * emit：分发自定义事件函数，相当于`this.$emie`。




