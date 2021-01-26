---
title: vue学习笔记
date: 2021-01-23 21:00:03
tags:
categories:
- vue
sticky: 9999
---

“在我看来，春天里一棵小草生长，它没有什么目的。风起时一匹公马发情，它也没有什么目的。草长马发情，绝非表演给什么人看的，这就是存在本身。我要抱着草长马发情的伟大真诚去做一切事，而不是在人前羞羞答答地表演。在我看来，人都是为了要表演，失去了自己的存在。”   -----王小波

## 0.1. 插值操作的语法和指令 
1. mustache (使用双大括号进行插值操作) {{可以调用函数、表达式等}} **[语法]**  
2. v-onece：只渲染一次数据，数据在改变时视图不会改变，后不接表达式。
3. v-html：解析并插入html
```html
<!-- url指一个含html标签的变量 -->
<div v-html="url"></div>
```
4. v-text：将文本放在DOM上进行展示,接受一个string类型。
```html
<div v-text="message"></div>
```
5. v-pre: 将标签中的元素，不做任何解析，原封不动的显示出来。  
6. v-cloak：“斗篷”  在vue没加载之前，存在v-cloak属性，加载后消失
```html
<style>
[v-cloak]{
  display:none;
}
</style>
<div v-cloak></div>
```
## 0.2. 绑定属性 
1.  v-bind：可以绑定任何属性，常用动态绑定如src，href。语法糖：：  
2. :class:{类名:boolean,类名:boolean}动态绑定class,当boolean为false的时候，DOM中不存该class **[对象语法]**
```html
<!-- 括号是不能省略的 -->
<div :class="getClass()"></div>
<script>
method:{
  getclass(){
     return {类名:boolean,类名:boolean}
  }
}
</script>
```
* :class:['active','line'] **[数组语法]**，一般用于服务器返回的数据，不会经常使用。 

 ```html
 <!-- 练习 点击列表 改变列表class -->
 <template>
  <div>
    <ul>
    <!--index==active  遍历的index与active 进行比较 数据驱动视图 -->
      <li
        :key="index"
        :class="{line:index==active}"
        @click="changeColor(index)"
        v-for="(m,index) in movies"
      >{{m}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      movies: ["vue", "react", "angular"],
      active: 0,
    };
  },
  methods: {
    changeColor(index) {
      this.active = index;
    },
  },
};
</script>

<style>
.line {
  color: red;
}
</style>
 ``` 
3. :style:   
**[对象语法]**  :style="{key(style属性名，可以使用驼峰标识 如font-size可以写成fontSize):value(属性值，不写'' vue会吧属性值解析成变量)}"  
**[数组语法]** :style="[style1,style2]" **style1与style2为{属性名:属性值}，{属性名：属性值}**
## 0.3. 计算属性 computed
**某些情况我们可能会对数据进行转化或者合并到一起，这个时候就需要用到计算属性**  
* 按照属性的方式给函数命名 如 
```js
fullName：function(){
return fristName + lastName
}  
```

**创建vue时可以在option中传入**  
* el： 
* data： 
* methods：
* computed：
* 生命周期函数
* 计算属性的getter和setter 
**每个计算属性中都包含getter和setter** 
```js
computed:{
  fullname:{
    //使用get方法计算值
    get:function(){
     return 'abc'
    },
    //在给计算值赋值是调用get方法 一般时不使用 set方法是有参数的
    set:function(newValue){
      
    }
  }
}
```
* 计算属性的缓存
计算属性是有缓存的，会比methods性能更高，因为methods会多次调用，而在变量没有改变时computed只会调用一次。
## 0.4. ES6语法（补充） js作者 Brendan Eich
let：块级作用域 当变量需要改变时
* 变量作用域：变量在什么范围内是可用的 var是没有块级作用域的
```js
//无块级作用域，打印出的是第btn.length个按钮（错误的方式）
for(var i=0; i<btn.length; i++){
   btn[i].addEventListener.('click',function(){
     console.log('点击第'+i+'按钮')
   })
}
//闭包可以解决该问题 
for(var i=0; i<btn.length; i++){
  (function(num){
  btn[i].addEventListener.('click',function(){
     console.log('点击第'+ num +'按钮')
   })
  })(i)
 
}
 //ES6 let 
  for(let i=0; i<btn.length; i++)
   //相当于下面的大括号有自己的作用域
  {
   btn[i].addEventListener.('click',function()
   {
     console.log('点击第'+i+'按钮')
   })
   }
```
const：将变量修饰成常量，不可以赋值。建议：优先使用const，只有需要改变某一标识符时才使用let。
* 一旦给const 修饰的常量，不能进行修改。
* 在使用const时，一定要进行赋值。
* 常量的含义是指向的对象不能修改，但是可以改变内部对象的属性。
```js
//不能赋值是因为对象内存地址不能改变
const obj ={
  name:'deng',
  age:24,
  height:186
}
//这样赋值是不会改变对象的内存地址的
obj.name = 'xuyang'
```
对象字面量的增强写法：
* 字面量：obj={} 这个括号就是字面量
```js
//es6中这样赋值
const  name='deng';
const age=24;
const height=186;
const obj ={
  name,
  age,
  height
}
```
函数的增强写法
```js
obj={
  // ES6中这样写就可以
  run(){

  },
  eat(){

  },
  name:'deng'
}
```
## 0.5. v-on指令
基本使用：v-on:click="handleAdd"
语法糖：@click="handleAdd"
* 在vue中如果@click="handleAdd"没有参数，而方法需要一个参数，默认传event对象。如果想使用event和其他参数，可以用$event。
* 修饰符：
1. .stop: @click.stop="btnclick" 阻止冒泡事件。  
2. .prevent:阻止默认事件。
3. 键盘键的点击 @keyup.enter。
4. .navtive 组件的点击事件。
5. v-once 只能点击一次。
## 0.6. 条件判断
1. v-if：接受Boolean类型的值，控制页面渲染。
2. v-else：结合v-if当if条件不满足时，显示v-else。
```html
<template>
  <div>
    <!-- 用户登录切换的案例 -->
    <!-- tip:当切换时用户输入的内容不会清空 因为vue先进行一个虚拟DOM的渲染，出于性能考虑，会尽量复用已经存在的元素，不会重新创建。加个 key  标识-->
    <div>
      <label for="username" v-if="uesrname">
        账号:
        <input id="username" placeholder="请输入账号" type="text" />
      </label>
      <label for="e_mail" v-else>
        邮箱:
        <input id="e_mail" placeholder="请输入邮箱" type="text" />
      </label>
      <el-button type="primary" round size="mini" @click="uesrname = !uesrname">切换</el-button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      uesrname: true,
    };
  },
};
</script>

<style>
[v-cloak] {
  display: none;
}
.line {
  color: red;
}
</style>
```
3. v-show:控制DOM节点是否显示；  
* 与v-if的区别：当条件为false时，包含v-if的元素不会存在在DOM中，是创建销毁的过程，而v-show是添加行内style的display，当频繁切换时建议使用v-show。
## 0.7. 循环遍历
* key主要的作用是更高效的更新虚拟dom，但得保证key的唯一性
1. 遍历数组
```html
<!--   knowledge: ["vue", "react", "angular"], -->
    <ul>
      <li v-for="(item,index) in knowledge" :key="item">{{index+1}}.{{item}}</li>
    </ul>
```
2. 遍历对象
```html
<!--    
        objknowledge: {
        fristName: "deng",
        lastName: "xuyang",
        score: 99,
      } -->
    <ul>
      <li v-for="(value,key) in objknowledge" :key="value">{{key}}:{{value}}</li>
    </ul>
```
3. 哪些方法是响应式的：
* push() //向最后一位添加 push(...num)是可变参数，把多个参数变为数组
* pop() //从最后一位删除
* shift() //删除数组中的第一个元素
* unshift() //在数组最前面添加元素
* splice() //splice(位置start，操作几个元素，'加入元素')
* sort()//排序
* reverse()//反转
4. 通过索引值修改数据，是不会响应的  
解决：Vue.set(要修改的对象，索引值，修改后的值)
## 0.8. 案例：
```html
<template>
  <div>
    <div v-if="books.length>0">
      <table>
        <thead>
          <tr>
            <th></th>
            <th>书籍名称</th>
            <th>出版日期</th>
            <th>价格</th>
            <th>购买数量</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item,index) in books" :key="item.id">
            <td>{{index+1}}</td>
            <td>{{item.name}}</td>
            <td>{{item.date}}</td>
            <!-- 使用过滤器 显示最终价格样式 -->
            <td>{{item.price | showprice}}</td>
            <td>
              <el-button type="primary" size="mini" @click="increment(index)">+</el-button>
              {{item.count}}
              <el-button
                type="primary"
                size="mini"
                :disabled="item.count<1"
                @click="decrement(index)"
              >-</el-button>
            </td>
            <td>
              <el-button type="danger" size="mini" @click="hangleremove(index)">移除</el-button>
            </td>
          </tr>
        </tbody>
      </table>
      <h2>总价:{{totalPrice | showprice}}</h2>
    </div>

    <div v-else>
      <h2>暂无书籍</h2>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      books: [
        { id: 1, name: "代码大全", price: 90, date: "2019-02", count: 0 },
        { id: 2, name: "UNIX编程", price: 80, date: "2019-03", count: 0 },
        { id: 3, name: "算法导论", price: 75, date: "2019-05", count: 0 },
        { id: 4, name: "编程珠玑", price: 100, date: "2019-06", count: 0 },
      ],
    };
  },
  methods: {
    increment(index) {
      this.books[index].count++;
    },
    decrement(index) {
      this.books[index].count--;
    },
    hangleremove(index) {
      this.books.splice(index, 1);
    },
  },
  computed: {
    totalPrice() {
      let totalPrice = 0;
      //1.普通的for循环
      // for (let i = 0; i < this.books.length; i++) {
      //   totalPrice += this.books[i].price * this.books[i].count;
      // }
      //2.for...in
      // for (const i in this.books) {
      //   totalPrice += this.books[i].price * this.books[i].count;
      // }
      //3.for...of
      // for (const item of this.books) {
      //   totalPrice += item.price * item.count;
      // }
      //reduce
      totalPrice = this.books.reduce(
        (prevValue, n) => prevValue + n.price * n.count,
        0
      );
      return totalPrice;
    },
  },
  //过滤器
  filters: {
    showprice(price) {
      return "￥" + price.toFixed(2);
    },
  },
};
</script>

<style>
table {
  border: 1px solid gray;
  border-collapse: collapse;
}
td {
  border: 1px solid gray;
  border-collapse: collapse;
  width: 200px;
  height: 50px;
}
th {
  border: 1px solid gray;
  border-collapse: collapse;
  background-color: lightgray;
  width: 100px;
}
</style>
```
* 编程范式：命令式编程/声明式编程
* 编程范式：面向对象编程（第一公民：对象）/函数式编程（第一公民：函数）
* 高阶函数 filter/map/reduce
1. filter：必须返回Boolean值，当返回一个true时，函数内部会将这次回调的n加入数组中，当返回false是，函数内部会支佛那个过滤掉n。
2. map:映射操作
3. reduce:对数组中所有的内容进行汇总
```js
//高阶函数的使用
      //filter
      const nums = [1, 5, 50, 89, 100, 200];
      let newNums = nums.filter((n) => {
        return n <= 100;
      });
      //map
      let new2Nums = newNums.map((n) => {
        return n * 2;
      });
      //reduce prevalue为上一次return的值 0 为初始值
      let total = new2Nums.reduce((prevalue, n) => {
        return prevalue + n;
      }, 0);
      console.log(total);
      //高阶写法
       const nums = [1, 5, 50, 89, 100, 200];
      let total = nums
        .filter((n) => n < 100)
        .map((n) => n * 2)
        .reduce((prevvalue, n) => prevvalue + n);
      console.log(total);
```
## 0.9. v-model使用
* 原理：v-model其实是一个语法糖，本质是动态绑定input的:value属性和@input="things = $event.target.value"事件实现双向绑定
1. radio:互斥绑定name，如果绑定想相同的v-model，name可以省略。对应Boolean值
2. checkbox：单选对应Boolean；多选对应数组。
3. select：单选对应srring类型，多选对应数组类型。
* 修饰符
1. .lazy：在失去焦点或者enter时候才会更新。
2. .number：转换成number。
3. .trim：去空格.
## 0.10. 组件化
* 常规组件化
1. 将一个完成的页面分成很多组件。
2. 每个组件都用于实现页面的一个功能模块。
3. 而每一个组件都可以进行细分。
* Vue组件化
1. 他提供了一种抽象，让我们开发一个个独立可复用的小组件来构造我们的应用。
2. 任何的应用都会被抽象成一颗组件树。
* 组件使用步骤
1. 创建组件构造器。调用 Vue.extend(),传入template作为组件模板
2. 注册组件。调用 Vue.component()，传入使用时的名称和构造器
3. 使用组件。
* 全局组件和局部组件
 1. 全局组件可以在多个vue实例中引用，通过Vue.component()注册。
 2. 局部组件在某个vue实例下添加component：{
   组件名：构造器
 }属性
 * 父组件和子组件
 1. 组件和组件之间存在层级关系，在父组件构造器中可以注册子组件
 2. 父子组件错误用法：以子标签的形式在vue实例中使用
 * 组件语法糖 内部调用 Vue.extend()
 ```js
    //全局组件
    vue.component(cpn,{
    template:`
    组件内容
    `
    })
    //局部组件
    components：{
      cpn：{
    template:`
    组件内容
    `
    }
    }
 ```
* 组件模板分离
1. 通过script标签，类型为text/x-template，通过注册时cpn:'#id'绑定。
2. 通过template标签，直接把内容写进去。
* 为什么组件的data是一个方法，需要返回一个新的对象指向不同的内存地址。
* 父子组件通信
1. 通过props向子组件传递数据。properties（属性）
```html
<template>
<div>
<cpn :cmsg="msg"></cpn>
</div>
</template>
<script>
const cpn={
  template:``,
  //传数组
  props:['cmsg'],
  //传对象
  props:{
  //类型限制
  cmsg:String,
  //提供默认值及必传值 
  cmsg:{
     cmsg:String,
     default:'默认值'
     required:true
  }
  //默认值如果是个对象或者数组 必须为一个函数
   cmsg:{
     cmsg:Array,
     default(){
       return []
     }
     required:true
  }
  }
}
<script/>
```
* props中的驼峰标识 用- 入 cInfo等同于 c-info
2. 通过事件像父组件发送消息。自定义事件 $emit(发射) Events（事件）
```js
components: {
    cpn: {
      props: ["cmsg"],
      template: `
      <div>
       <p>{{cmsg}}这是子组件</p>
      <button v-for="item in books" @click="hanleclick(item)">{{item.name}}</button>
      </div>
      `,
      data() {
        return {
          books: [
            { id: 1, name: "代码大全", price: 90, date: "2019-02", count: 0 },
            { id: 2, name: "UNIX编程", price: 80, date: "2019-03", count: 0 },
            { id: 3, name: "算法导论", price: 75, date: "2019-05", count: 0 },
            { id: 4, name: "编程珠玑", price: 100, date: "2019-06", count: 0 },
          ],
        };
      },
      methods: {
        hanleclick(item) {
          this.$emit("itemClick",item)
         // console.log(item);
        },
      },
    },
  },
```
3. 父子组件的访问方式
* 父组件访问子组件：$children或$refs reference(引用)。
* 子组件访问父组件：$parent。
## 0.11. 插槽slot 
1. 插槽的基本使用
```html
<template>
  <div>
    <cpn :cmsg="msg" ref="ccpn">
      <div class="slotcolor">插槽</div>
    </cpn>
    <cpn :cmsg="msg" ref="ccpn">
      <button>按钮</button>
    </cpn>
    <cpn></cpn>
    <button @click="showmessage">按钮</button>
  </div>
</template>


<script>
export default {
  data() {
    return {
      msg: "这是父组件传来的值",
    };
  },
  methods: {
    showmessage() {
     // console.log(this.$children[0].name);
      this.$refs.ccpn.hanleclick();
      this.msg = this.$children[0].name;
      console.log(this.$refs.ccpn.name);
    },
  },
  computed: {},

  components: {
    cpn: {
      props: ["cmsg"],
      template: `
      <div>
       <p>{{cmsg}}这是子组件</p>
       <button @click="btnClick">子组件按钮</button>
       <slot><p>插槽默认值</p></slot>
      </div>
      `,
      data() {
        return {
          name: "我是子组件的name",
        };
      },
      methods: {
        hanleclick() {
          // this.$emit("itemClick",item)
          // console.log(item);
          console.log("父组件访问子组件");
        },
        btnClick(){
          //访问父组件
          console.log(this.$parent);
           console.log(this.$root);
        }
      },
    },
  },
  //过滤器
  filters: {},
};
</script>

<style>
.slotcolor {
color: rgb(231, 21, 21);
}
</style>
```
2. 具名插槽
```html
<template>
  <div>
    <cpn>
      <span slot="center">标题</span>
    </cpn>
  </div>
</template>


<script>
export default {
  data() {
    return {
      msg: "这是父组件传来的值",
    };
  },
  methods: {
    showmessage() {
     // console.log(this.$children[0].name);
      this.$refs.ccpn.hanleclick();
      this.msg = this.$children[0].name;
      console.log(this.$refs.ccpn.name);
    },
  },
  computed: {},

  components: {
    cpn: {
      props: ["cmsg"],
      template: `
      <div>
       <slot name="left"><span>左边</span></slot>
       <slot name="center"><span>中间</span></slot>
       <slot name="right"><span>右边</span></slot>
      </div>
      `,
      data() {
        return {
          name: "我是子组件的name",
        };
      },
      methods: {
        hanleclick() {
          // this.$emit("itemClick",item)
          // console.log(item);
          console.log("父组件访问子组件");
        },
        btnClick(){
          //访问父组件
          console.log(this.$parent);
           console.log(this.$root);
        }
      },
    },
  },
  //过滤器
  filters: {},
};
</script>

<style>
table {
  border: 1px solid gray;
  border-collapse: collapse;
}
td {
  border: 1px solid gray;
  border-collapse: collapse;
  width: 200px;
  height: 50px;
}
th {
  border: 1px solid gray;
  border-collapse: collapse;
  background-color: lightgray;
  width: 100px;
}
.slotcolor {
color: rgb(231, 21, 21);
}
</style>
```
3. 作用域插槽
* 编译作用域：组件有自己的作用域，当使用组件时，不会找子组件的属性，会在当前组件中查找，父模板父模板编译，子模板子模板编译。
* 父组件替换插槽的标签，但是内容由子组件提供
## 0.12.模块化开发
* 模块化： 是具有特定功能的一个对象（ 广义理解 ）
* 模块定义的流程：
1. 定义模块（对象）
2. 导出模块
3. 引用模块
好处：可以存储多个独立的功能块，复用性高
1. 导出：
```js
//方式一
let height=1.88;
let age  = "20"
function sum(num1,num2){
  return num1+num2
}
class person {
  run(){
  console.log('跑')
  }
}
export {
  height,age,sum,person
}
//方式二
export let height=1.88;
export let age  = "20"
export function sum(num1,num2){
return num1+num2
}
//三 不命名的导出，其他人导入是可以命名的 但只能有一个
export default height
//* commonjs 
    module.exports = {
    add,
    mul
}
```
2. 导入：
```js
import {height,age,sun,person} form './aaa.js'
import * as aaa form './aaa.js'
//export default 导出是可以自己命名的
import xmheighe form './aaa.js'
const c=new person();
c.run()
//* commonjs 
const {add,mul} = require('./mathutils')
```
## 0.13.webpack

* webpack是模块打包工具，更加强调模块化开发管理而文件压缩、预处理等是它附带的功能。工厂
* gulp更强调前端模块自动化，模块化不是他的核心。工厂流水线
1. webpack安装 15 p93 了解知识回顾
## 0.14 vue cli
* runtime-compiler template->ast->render->vdom->UI
* runtime-only >render->vdom->UI 省掉了把template解析成 ast的过程
## 0.15. vue 路由
* 后端路由：有服务器处理url和页面之间的映射关系
* 前后端分离：后端只提供数据，不负责任何阶段的内容
* 前端渲染：浏览器网页中的大部分内容，都是由前端写的js代码在浏览器中执行，最终渲染出来的网页。
* SPA （single page web application） 单页面富应用
* 前端路由管理url和页面的映射关系
* 改变URL不发生刷新
1. url hash
2. history.pushState 入栈和出栈的阶段 replaceState是不能返回的
3. history.back()、history.forward和history.go(-1)
```js
//请求接口封装
import axios from 'axios'
export function request(config) {
    const instance = axios.create({
        baseURL: '你的接口地址',
        timeout: 5000
    })
    //请求拦截
    instance.interceptors.request.use(config => {
        return config
    }, error => {
    })
    //响应拦截
    instance.interceptors.response.use(res => {
        // console.log(res);
        return res.data
    }, err => {
        // console.log(err);
    })
    return instance(config)

    // .then(res => {
    //     resolve(res)
    // }).catch(err => {
    //     reject(err)
    // })
}
```
## vue响应式原理
1. Vue内部是如何监听数据改变的  
* Object.defineProperty 监听对象属性的改变
2. vue如何知道要通知哪些人，界面发生刷新
* 发布订阅者模式

```js
<script>
const obj={
  name:'哈哈哈',
  message:'dxy'
}
Object.keys(obj).forEach(key=>{
  let value = obj[key]
  Object.defineProperty(obj,key,{
    set(newValue){
      value= newValue
    },
    get(){
      return value
    }
  })
})
</script>
```
