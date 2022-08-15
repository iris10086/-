# 基本语法

## v-on

绑定事件

所绑定的事件，需要写在vue的methods方法中

```html
<button v-on:click="sayHi">
    hello
</button>
```

## v-if

判断vue对象中的属性选择是否显示

```html
<span v-if="judge===true">Yes</span>
<span v-if="judge===false">No</span>
```

## v-for

遍历对象/数组的值

语法同python的for

```html
<li v-for="item in items">
    {{item.msg}}
</li>
```

## v-bind

给组件绑定参数

```html
<h1 v-bind:title="hello">
    hello vue
</h1>
```

# 双向绑定

## v-model

实现双向绑定

```html
<input type="text" v-model="msg">
<div>{{msg}}</div>
```



# Axios异步请求

推荐使用Axios包进行异步请求的发送

## 钩子函数/生命周期函数

mounted:在渲染数据之前执行。

​				一般用于发送初始化数据请求。

​				只执行一次	

created......等

# 计算属性（vue亮点之一）

在创建对象时，使用computed在methods同级位置创建。

写法和methods一样。里面装方法。

computed与methods中的方法名可以重复，优先执行methods中的方法。

## 理解

计算属性的逻辑。

计算属性看作一个属性，值为返回值。

将计算属性中的方法返回值看作一个属性。缓存在内存中。

在该方法相关的值没有改变时，调用计算属性不会执行计算过程。会从缓存中获取数据。

若缓存失效。则会重新调用方法。更新缓存

# 组件

可理解为自定义标签。

​	将多个标签封装为一个组件，使用标签的形式进行使用。

使用Vue.component创建组件

```javascript
Vue.component("haha",{
    props: ['hello'], // 接受的参数。父向子传递
    data:{},
    template: "<div>{{hello}}</div>" // 标签集
});
```

## 插槽slot

动态的将组件拔插到另外一个组件的特定位置。

插槽的传值与函数传递

## 组件传值

通过bind绑定数据的形式传递值

```html
<haha>
    <!--        插槽对应-->
    <haha_title slot='title' :title="title"></haha_title>
    <haha_item slot='item' v-for="item in items" :item="item"></haha_item>
</haha>
```

## 组件传递函数

组件使用了传递进来的数据。

但是组件中的方法无法访问数据本身。

组件自身无法实现对数据的修改。

需要在组件外部实现修改数据的方法。再传递进入组件，实现组件调用。

具体步骤

​	外部实现数据操作函数

```javascript
methods: {
    removeItem: function(index){
        this.items.splice(index,1);
    }
}
```

​	再插槽插入位置，为插入的组件绑定自定义函数

```html
<haha_item slot='item' v-for="(item,index) in items"
           :item="item" :index="index"
            v-on:remove="removeItem(index)">
</haha_item>
```

​	在组件中调用自定义函数

```javascript
template: "<li>{{item}} <button @click='removeItem(index)'>删除</button></li>", // 绑定click时间，调用remove方法
methods: {
    removeItem: function (index){
        this.$emit("remove",index) // 调用自定义事件
    }
}
```



# Vue项目

模块化开发组件。通过引入使用组件。

## vue-router

### 使用

安装router

> cnpm install vue-router -s 

为router创建一个文件夹。

创建index.js文件配置router

```javascript
import Vue from 'vue'; // 引入Vue
import router from "vue-router"; // 引入router依赖

// 引入组件
import main from "../view/main";
import login from "../components/login";
// 注册router插件
Vue.use(router); 

// 创建router
export default new router({
  routes: [ // 配置路由
    {
      path: "/main",
      name: "首页",
      component: main
    },{
      path: "/login",
      name: "登录",
      component: login
    }
  ]
});
```

注册router

```javascript
import Vue from 'vue'
import App from './App'

import router from "./router" // 引入配置好的router

Vue.use(router) // 注册router

new Vue({
  el: '#app',
  router
})
```

### router-link

相当于a标签。用于路由

### router-view

将路由获取的组件放到router-view位置

## 路由的钩子函数

类似过滤器，拦截器

beforRouteEnter

beforRouteLeave

```javascript
beforRouteEnter:(to,from,next)=>{
	next();// 放行

},
beforRouteLeave:()=>{
    
}
```

> to 去向信息
>
> from 来源信息
>
> next() 放行
>
> next(Path)： 跳转其他页面
>
> next(false): 拒绝跳转，返回from页面
>
> next((vm)=>{})  仅在beforRouteEnter使用。vm是组件实例，可以使用当前组件的方法进行初始化。

