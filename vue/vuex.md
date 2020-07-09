# 基本介绍

- vuex的作用是用来帮我们管理多界面时需要共同调用的变量的工具，一般叫状态管理工具。

- google应用商店下载devtools插件，可以用来监控state里数据的变化，当然前提是通过mutations里的方法改变的

# 使用介绍

- ```
  npm install vuex --save
  ```

- 在src文件夹下新建store文件夹，并新建index.js文件，注册vuex并导出

```
// store/index.js
import Vue from 'vue'
import Vuex from 'vuex'	
Vue.use(Vuex)		//安装插件
const store = new Vuex.Store({
	state: {},
	mutations: {},
	actions: {}
})
export default store	//导出
//main.js
import store from './store/index.js'	//导入
new Vue({
el: '#app',
store		//挂载store对象
})
```

- this.$store.state.变量名便能拿到state里的数据

# Store对象属性介绍

## mutations

- mutations中是定义方法用的，且一般是直接改变state中数据的

```
//mutations中方法默认自带state参数
const store = new Vuex.Store({
state: {count: 0},
mutations: {
increment(state) {
	state.count++
}
}
})
```

- 组件中调用

```
this.$store.commit('方法名'，可选参数)
```

- mutations规范化管理写法
  - 在store文件夹下新建mutations-types.js文件，专门用来导出mutations方法名
  - 之后在store文件夹下的index.js以及需要的组件文件中import需要的变量，统一用这些变量名即可。这样可以避免方法名书写出错并方便管理
  - mutations里的方法要写成[函数名] () {}这种形式

```
//mutation-types.js
export const INCREMENT = 'increment'
//store/index.js
import INCREMENT from './mutation-types.js'
{
...,mutations: {
[INCREMENT] (state) {
	state.count++
}
}
}
```

## actions

- 当要通过异步操作处理state中的数据时，必须要先通过actions，不然devtools将无法记录到状态变化。
- 不是异步操作也可以，因为mutations中方法只是单纯的对数据进行操作，因此多余出来的逻辑可以用actions中的方法来表示。

- actions中方法的默认参数context，译为上下文的意思，不过可以暂时把它相当于store。

```
{...,
actions: {
	updateInfo(context,payload)	{		//这里payload只是传的一个参数
	return new Promise((resolve,reject) => {
	setTimeout(()=>{
	context.commit('update')	//actions中调用mutations中方法
	console.log(payload)
	resolve('成功了')
},2000)
	})
}}}
```

- 组件中调用actions中方法

```
this.$store.dispatch('方法名'，可选参数)
```

## getters

- getters有点类似一般组件中的计算属性，当需要对state中的数据进行一系列变换时就可以定义getter里的属性。

- 一般都是用默认的state参数，不过需要时可以传入第二个参数getters来更方便通过getters属性得到getters属性达到简写的目的

```javascript
{
...,getters: {
//默认state参数
totalPrice(state) {
return state.list.reduce((pre,next) => {
			return pre + next.counter*next.price
		},0)
},
//state和getters参数
morePrice(state,getters) {
return state.number + getters.totalPrice
	}
}
```

- 组件中调用

```
this.$store.getters.getters中计算属性名
```

### getters的更简洁写法

- 即将getters映射到组件的computed属性上，之后直接在组件中使用就行了而不用再通过$store.getters

```javascript
// test.vue
import {mapGetters} from 'vuex'
export default {
    ...,computed:{
    	//写法1，数组写法
    	...mapGetters(['cartLength','cartList'])	//数组的值就是getters里的计算属性
        //写法2，对象写法，这种写法就是给getters起别名
        ...mapGetters({
            length: 'cartLength',
            list: 'cartList'
        })
}
}
```

- actions里的方法也可以这么映射，就是把mapGetters中的Getters替换一下就行了。

## modules

- vue使用单一状态树，那么也意味着很多状态都会交给vuex来管理。当应用变得非常复杂时，store对象就有可能变得相当臃肿
- 为了解决这问题，Vuex允许我们将store分割成模块（module），而每个模块拥有自己的state，mutations，actions，getters等。
- 组织模块

```javascript
//注意各个模块中以及原本的store中的mutations，actions，getters不要重复
const moduleA = {
state:{..},mutations:{..},actions:{...},getters:{...}
}
const moduleB = {
state:{..},mutations:{..},actions:{...},getters:{...}
}
const store = new Vuex.Store({
modules: {
a: moduleA,
b: moduleB
}
})
```

- 组件中调用

```
//调用state
this.$store.state.a.name
//调用mutations
this.$store.commit('update'，可选参数)
//调用actions
this.$store.dispatch('aupdate'，可选参数)
//调用getters
this.$store.getters.getters中计算属性名
```

# vuex-store文件夹目录组织

- 因为一般项目比较大的时候可能一个store对象特比的长，导致想要修改一个属性特别困难，因此我们一般需要对store文件夹的index.js做一些抽离。

- 针对state属性，是在参数对象外单独定义一个对象然后另state属性等于它。
- 针对mutations，getters，actions都是单独定义一个js文件进行export default{}对象导出然后在index.js文件中导入。
- 针对modules属性是先单独建一个文件夹，因为可能会有多个模块，所以每有一个模块就导出它然后进行导入赋值。

```javascript
/store/index.js
import actions from './actions'
import getters from './getters'
import mutations from './mutations'
import moduleA from './modules/moduleA'
const state = {...}
const store = new Vuex.Store({
state,
actions,
mutations,
getters,
modules:{
a: moduleA
}
})
```

