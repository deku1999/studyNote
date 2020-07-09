# 前端发展阶段

## 后端渲染阶段

<img src="C:\Users\22140\Desktop\web-study\note\assets\img\image-20200709153213998.png" alt="image-20200709153213998"  />

## 前后端分离，前端渲染阶段

![image-20200709153249099](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200709153249099.png)

## SPA(单页面富应用)页面阶段

- 由前端路由来进行管理各个界面，当url改变时前端路由会自动的去选择展示哪些组件，每个url有其各自的对应关系

- SPA最主要的特点就是在前后端分离的基础上，加了一层前端路由，也就是前端来维护一套路由规则
- 前端路由的核心就是改变URL，但是页面不进行整体的刷新

# vue-router安装配置

- 在src新建router文件夹，并新建index.js文件

```javascript
//router/index.js
import Vue from 'vue'
import VueRouter from 'vue-router'
const Home = () => import('components/home/home.vue')	//路由懒加载，当路由被访问的时候再加载，															提高加载速率
....		//懒加载导入其他组件

//这段代码不用硬记，是为了解决路由重复点击报错的问题
const originalPush = VueRouter.prototype.push		
   VueRouter.prototype.push = function push(location) {
   return originalPush.call(this, location).catch(err => err)
}

Vue.use(VueRouter)		//安装vue-router
const routes = [
    //配置映射关系
    {
      //配置默认路径
        path: '/',
        redirect: '/home'
    },
    {
     	path: '/home',
        component: Home
    },{...},{...}
]
const router = new  VueRouter({
    routes,
    mode: 'history'		//将默认的哈希路径模式改为history模式
})
export default router //导出路由对象
//main.js
import router from './router/index'
import Vue from 'vue'
new Vue({
    ...,
    router	//挂载路由对象
})
```

# router显示

- router显示都是通过router-view标签

## 通过router-link跳转

```html
//app.vue
<template>
    <router-link to="/home">首页</router-link>	//to的值必须要跟路由配置时的路径一样
    <router-link to="/about">关于</router-link>
    <router-view></router-view>
</template>
```

- router-link除了to，还有一些其他属性

```html
//tag属性，指定router-link会被渲染成什么组件，默认好像是a标签。下面代码则是渲染成li
<router-link tag="li" to="/home"></router-link>
```

```html
//replace：不会留下历史记录，所以指定replace的情况下，后退键不能返回到上一个页面中
```

```html
//active-class：当router-link对应的路由匹配成功时，会自动给当前元素设置一个router-link-active的class，设置active-class可以修改默认名称
```

## 通过代码跳转

```html
//app.vue
<template>
	<button @click="toHome">首页</button>
    <button @click="toAbout">关于</button>
    <router-view></router-view>
</template>
<script>
    export default {methods:{
        toHome() {
            this.$router.push('/home')	//可以返回，有历史记录
        },
        toAbout() {
            this.$router.replace('/home')	//不能返回，没历史记录
        },
    }}
</script>
```

# vue-router参数传递

- params类型路径：/user/123
- query类型路径： /user?id=123
- 我倾向于用query类型，更加的方便

## params类型

- 先在映射关系进行配置

```
{
path: '/user/:id',
component: User
}
```

- 利用router-link传递

```html
<router-link to="/user/123"></router-link>
```

- 利用代码传递

```javascript
data() {
return {
id: 123
}
}
,methods: {
	toUser() {
	this.$router.push('/user/'+this.id)
	}
}
```

- 获取params对象里属性

```
this.$route.params.id	//获取id，params就是参数列表的意思，注意是$route,表示的是当前活跃的路由对象
```

## query类型

- 通过router-link传递

```html
<router-link :to="{path:'/profile',query:{name:'why',age:18}}"></router-link>
```

- 通过代码传递

```
methods:{
	toProfile() {
	this.$router.push({
	path: '/profile',
	query: {
		name: 'robot',
		age: '18'
	}
	})
}}
```

- 获取query对象里的属性

```
this.$route.query.name
```

# keep-alive遇见vue-router

- keep-alive是vue内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。
- router-view也是一个组件，如果直接被包在keep-alive里面，所有路径匹配到的视图视图组件都会被缓存。
- 如果直接进行组件切换那么本质上是不断的销毁创建组件，这样的性能并不好，因此需要keep-alive

```html
<keep-alive>
    <router-view></router-view>
</keep-alive>
```

## keep-alive相关的回调函数

- 下面两个函数，只有该组件被使用了keep-alive时，才是有效的

- ```
  activated() {
  	console.log('activated')	//加载该组件时调用
  }
  ```

- ```
  deactivated() {
  	console.log('deactivated')		//离开该组件时调用
  }
  ```

## keep-alive组件匹配

- keep-alive有两个非常重要的组件属性
  - include，字符串或正则表达，只有被匹配的组件才会被缓存
  - exclude，字符串或正则表达，任何被匹配的组件都不会被缓存

```html
<keep-alive exclude="user,profile">		//这里字符串的值就是组件的name值,注意逗号右边不要有空格
    <router-view></router-view>
</keep-alive>
```

# 导航守卫

- 为了实现点击不同的例如首页，用户，关于而同时更改网页title可以借助导航守卫
- 给映射关系中再多配置meta属性，然后调用路由前置钩子来实现在跳转的前一步更改标题

```javascript
// router/index.js
const routes = [
{
	path: '/home',
	component:Home,
	meta: {
		title:'首页'	//设置标题
	}
}
{
	path: '/about',
	component: About,
	meta: {
		title:'关于'
	}
},{..}{..}]
//调用路由前置钩子设置标题
router.beforeEach((to,from,next) => {
    window.document.title = to.meta.title	//更改窗口标题
    next()
})
```



# 嵌套路由

- 比如在home页面中，我们希望通过/home/news，/home/message访问一些内容。一个路径映射一个组件，访问这两个路径也会分别渲染两个组件
- 它的根本思路还是在一个组件中继续抽取几个不同的组件根据选择进行显示，所以应首先新建几个需要的子组件并在index.js文件中导入，然后在index.js其父组件中添加children属性配置映射关系
- 最后在需要展示的父组件中通过router-link与router-view来进行选择性展示，注意这里的路径要写完整。当然也可以通过代码跳转

```html
//router/index.js
{
path:'/home',
component: Home,
children: [
	{
		path:'news',	//子模块中的path不能写'/'
		component: HomeNews
	},
	{
		path:'message',
		component: HomeMessage
	}
]}
//Home.vue
<router-link to="/home/news"></router-link>		//这里to的路径要写完整
<router-link to="/home/message"></router-link>
<router-view></router-view>
```

# 常用路由方法

- ```
  this.$route.path 	//获取当前活跃路由的path
  this.$route.path.indexOf(path)	//可以判断当前处于哪个路由
  ```

- ```
  //返回上一页，前提是不是通过replace方法进来的
  this.$router.back()
  this.$router.go(-1)
  ```

  