# vue引入

- script标签引入
  - cdn引入
  - 直接引入vue文件，可以去官网下载
- npm引入

# 一些补充

## $nextTick介绍

- created，mounted，$nextTick官方解释

  - created：在实例创建完成后被立即调用，在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算，watch/event事件回调。然而挂载还没开始，$el属性目前不可见。

  - mounted：el被创建的vm.$el替换，并挂载到实例上去之后调用该钩子，如果root实例挂载了一个文档内元素，当mounted被调用时vm.$el也在文档内。

    注意mounted不会承诺所有的子组件也都一起被挂载，如果你希望等到整个视图都渲染完毕，可以用vm.$nextTick替换掉mounted。

  - $nextTick：在下次DOM更新循环结束之后执行延迟回调，在修改数据之后立即调用这个方法，获取更新后的DOM。

## 混入(mixins)

- 混入一般是为了减少代码，生命周期函数或者data里的变量，components注册等都可以。
- 一般流程为：先定义混入对象，并在对象内部写上要混入的代码，当然如果混入代码中包含别的外部函数要先导入。
- 在需要的组件中导入混入对象，记住混入的本质是为了减少相同代码的书写，因此在引用了混入对象后记得删除组件中跟混入对象相同的代码

```javascript
// mixin.js
import {debounce} from 'common/util'
export const mixImage = {
    //混入代码，还可以写components，data等
	mounted() {
	  const refresh = debounce(this.$refs.scroll.refresh,200)
      this.$bus.$on('imageLoad',() => {
          refresh()
      })
	}
}
// test.vue
import {mixImage} from './mixin'
export default {
    ...,
    mixins: [mixImage]	//引入混入对象
}
```

# mvvm介绍

- mvvm，m就是model，v就是view
- View层
  - 视图层
  - 在我们前端开发中，通常就是dom层
  - 主要的作用就是给用户展示各种信息
- Model层
  - 数据层
  - 数据可能是我们固定的死数据，更多的是来自我们服务器，从网络上请求下来的数据
  - 在我们计算器的案例中，就是后面抽取出来的obj，当然里面的数据可能没有这么简单
- ViewModel层：
  - 视图模型层
  - 视图模型层是View和Model沟通的桥梁
  - 一方面它实现了Data Binding，也就是数据绑定，将Model的改变实时的反映到view中
  - 另一方面它实现了DOM Listener，也就是DOM监听，当DOM发生一些事件（点击，滚动，touch等）时，可以监听到，并在需要的情况下改变对应的data。

# vue生命周期图解

![image-20200708203223916](C:\Users\22140\Desktop\web-study\note\assets\img\image-20200708203223916.png)

# 数据绑定与指令与事件

- vue的响应式并不是一尘不变的，例如有些方式去改变数组中的数据就并不是响应式的，例如直接通过数组索引去改变
- 数组的pop，push，shift，splice等都是响应式的
- 因此要根据情况选择合适的方法才能实现响应式

## vue实例与数据绑定

### 实例与数据

```vue
var app = new Vue({
//选项
el:'#app' , ////挂载的dom元素，也可以是htmlElement
data: {
a: 2
}})
```

- 变量app就代表了这个Vue实例，事实上几乎所有的代码都是一个对象，写入Vue实例的选项内的。
- 实例中除了有el，data外，还有生命周期函数钩子，computed计算属性，filters过滤器，methods方法，还有在一些情况下存在的例如设置了路由keep-alive的activated，deactivated
- vue实例本身也代理了data对象里的所有属性，所以也可以app.a这样来访问vue实例的数据

### 插值与表达式

- 使用双大括号（mustach语法）{{}}是最基本的文本插值方法，它会自动将我们双向绑定的数据显示出来
- {{}}中除了简单的绑定属性值外，也可以用js表达式进行简单的运算，三元运算符等，例如：
  - {{number/10}}
  - {{isOk? '确定' : '取消'}}
  - {{ text.split(',').reverse().join(',') }}
- Vue.js只支持单个表达式，不支持语句和流控制

### 过滤器

- vue支持在{{}}插值的尾部添加一个管道符“（|）”来对数据进行过滤，常用于格式化文本例如字母全部大写，数字保留2位小数等。
- 过滤器是通过给vue实例添加filters选项来设置，value参数就是数据本身

```vue
new Vue({
..,
filters: {
handleNumber(value) {
return value.toFixed(2)
}
}})
```

- 过滤器也可以串联，而且可以接受参数

  - ```
    {{message | handleNumber | filterb}}  //串联
    ```

  - ```
    {{message | filterb('arg1','arg2')}}	//接收参数，这里的arg1与arg2是传给过滤器的第2个和												第三个参数，因为第一个是数据本身
    ```

## 指令与事件

- 指令是vue.js模板中最常用的功能，它带有前缀v-

### v-on

- 当没有参数时可以省略小括号，不过事件一般默认会传回去一个event事件对象
- 如果想直接在用v-on时传event，需要写$event

```
@click = "test($event)"
```

- 用来监听事件的属性，后面一般跟vue示例中的方法，或者直接是简单的表达式

```
<button v-on:click="handleClick">点击</button> //方法
<button v-on:click="show = false">点击</button> //表达式
```

#### 修饰符

- 在@绑定的事件后加小圆点”.“，再跟一个后缀来使用修饰符，vue支持下列修饰符

  - ```
    <a @click.stop = "handle"></a>   //阻止单击事件冒泡
    ```

  - ```
    <form @submit.prevent="handle"></form>   //提交事件不再重载界面
    ```

  - ```
    <a @click.stop.prevent = "handle"></a>   //修饰符可以串联
    ```

  - ```
    <form @submit.prevent></form>    //只有修饰符
    ```

  - ```
    <div @click.capture="handle">..</div>  //添加事件侦听器时使用事件捕获模式
    ```

  - ```
    <div @click.self="handle">..</div>   //只当事件在该元素本身(而不是子元素)触发时回调
    ```

  - ```
    <div @click.once="handle">..</div>    //只触发一次，组件同样适用
    ```

- 在表单元素上监听键盘事件时，还可以使用按键修饰符，比如按下具体某个键时才调用

  - ```
    <input @keyup.13="submit">	//只有在keyCode是13时调用vm.submit()
    ```

  - ```
    <input @keyup.shift.83="handleSave">	//shift+s
    ```

### v-for

- v-for要跟key属性结合使用以此来提高效率，key绑定item可以提高效率不过前提是item不重复，绑定index仅仅只是消除警告

  ```vuevue
  v-for="(item,index) in movies" :key="item" //绑定item
  v-for="(item,index) in movies" :key="index" //绑定index
  ```

- 主要是为了遍历的，可以遍历数组，对象，数字

- 遍历数组

  - ```
    v-for="(item,index) in items" //item为元素，index为下标
    ```

- 遍历对象

  - 当只有一个参数遍历时得到的为value，为两个时(value,key)前面为value后面为key，为三个时顺序为(value,key,index)

- 遍历数字

  - 遍历数子是从1开始的，例如

    ```
    v-for="item in 5"
    ```

### v-bind

- 绑定属性，在属性前面加上v-bind就会将属性值当作变量去进行解析，例v-bind:src=”imgurl”

#### 绑定class的几种方式

- 对象语法，key为类名，value为vue示例中的数据，当值为true时类存在，否则不存在

```
<div :class="{active:isActive}"
```

- 数组语法，适用于绑定多个类名，类名为vue实例中test1等代表的值

```
<div :class="[test1,test2,test3]">...</div>
```

- 如果直接在自定义组件上使用class或者:class，样式规则会直接应用到这个组件的根元素上

#### 绑定内联样式

- 使用v-bind可以给元素绑定内联样式，方法与:class类似，也有对象语法和数组语法，css属性使用驼峰命名或者短横分隔命名

```
<div :style="{'color':color,'fontSize':fontSize+'px'}"
```

- 数组语法不常用，不多介绍

### v-if,v-else-if,v-else

- 根据条件来决定是否展示某块区域，用法就如字面意思

```
<div v-if="score>90">优秀 </div>
<div v-else-if="score>=80">良好 </div>
<div v-else>madamada</div>
```

### v-show

- 根据布尔值决定区域是否显示

```
<div v-show="true">...</div>
```

- 跟v-if的区别就是，v-show改变显示的原理是display:none，而v-if则是完全的删除，重建dom元素，因此要根据数据变化的频率选择合适的方式

### 其他指令

- v-once，直接使用，指令使用后改变插值不会重新渲染dom树

- v-html，例

  ```
  <span v-html="link"> </span>
  ```

  link为带有html标签的一个字符串，那么该指令就会解析html标签之后将解析后的内容作为span标签的html内容。

- v-text，跟v-html基本一样，不过不会解析html标签

- v-pre，直接使用，mustache语法就不会被解析

- v-cloak，直接使用，利用的是vue的一个机制在vue解析之前该vue属性存在，解析之后该属性被删除，所以可以配合css的display:none来使用，这样如果vue的js还没执行完会先不进行显示

```
<style>
[v-cloak] {
display: none;
}
</style>
<div id="app" v-cloak>{{message}}</div>
```

## 语法糖

- 就是指令的简洁写法
- v-on的语法糖为@
- v-bind为“：”

# 计算属性

- 模版内的表达式常用于简单的运算，当期过长或逻辑复杂时，会难以维护，计算属性就是为了解决这问题。在一个计算属性里可以完成各种复杂的逻辑，包括运算，函数调用等，只要最终返回一个结果就可以。
- 计算属性还可以依赖多个vue实例的数据，只要其中任一数据发生变化，计算属性就会重新执行，视图也会更新。
- 计算属性可以依赖其他计算属性，计算属性不仅可以依赖当前vue实例数据，也可以依赖其他实例的数据。
- 计算属性是基于它的依赖缓存的，一个计算属性所依赖的数据发生变化时它才会重新取值，但是methods则不同，只要重新渲染它就会被调用，因此函数也会被执行。使用计算属性还是methods取决于你是否需要缓存，当遍历大数组和做大量计算时，应当使用计算属性，除非你不希望得到缓存。
- 计算属性的本质是个对象，里面包含getter和setter方法，但是一般不会用到setter方法，所以一般都是直接简写。

```
computed: {
	total() {
	return this.one + " " + this.two
	}
}
```

# 侦听属性

- 有时需要根据某个数据的变动而做出相应响应，这时可以用watch属性。
- watch的本质是侦听的属性只要一改变便会执行该方法
- watch里方法的参数也可以不传或者传1个或2个，传1个代表的是该属性变动后的新值。传2个第一个是新值，第二个为旧值

```
watch: {
question(newVal,oldVal) {
thie.answer = newVal + odlVal
}}
```

# 元素复用问题

- vue底层出于性能考虑，有时会出现元素复用，为了解决这问题，例如input，可以为两个input加入不同的key属性值，只要key不一样就不会进行复用。

# 表单与v-model

- v-model用于在表单元素上双向绑定数据，例如在输入框上使用时，输入的内容会实时映射到绑定的数据上
- 如果v-model，value，placeholder同时存在且值不同，那么优先级是v-model最高，其次是value，最后是placeholder。当然一般不会出现这种情况

## 基本用法

### text与textarea

```html
//直接用即可
<input type="text" placeholder="来输入吧" v-model="message" >
<p>你输入的内容是：{{message}}</p>
---js
new Vue({
el: "...",
data: {
message: ''
}})
```

### 单选按钮radio

- 单独使用

  - 单独使用不需要v-model，直接通过v-bind绑定checked属性并等于一个布尔值，控制布尔值即可，例如

    ```html
    <input type="radio" :checked="picked"><label>单选按钮</label>
    new Vue({
    ...,
    data: {
    picked: false
    }})
    ```

- 组合使用达到互斥效果，需要v-model配合value来使用

```html
//这样系统会选中v-model值与value值相同的按钮，即开始就会选中js，不过点击选择别的按钮可以改变v-model的值进而达到互斥效果
<input type="radio" value="html" id="html" v-model="picked">
<label for="html">html</label>
<input type="radio" value="css" id="css" v-model="picked">
<label for="css">css</label>
<input type="radio" value="js" id="js" v-model="picked">
<label for="js">js</label>
new Vue({
..,data:{
picked: 'js'
}})
```

### 复选框checkbox

- 单独使用

  - 单独使用也需要v-model，并绑定一个布尔值。当v-model值为true时checkbox会被选中，反过来取消勾选也会使得v-model值变为false

  ```html
  <input type="checkbox" id="check" v-model="isChecked">
  <label for="check">选择状态是{{isChecked}}</label>
  new Vue({...,data:{isChecked: true}})
  ```

- 组合使用

  - 组合使用也是利用v-model与value，同时考虑到复选框可以同时选中多个，所以v-model绑定的是数组

  ```html
  //这样开始就只选中了js，但是每当勾选相应的checkbox，picked数组也会发生更新
  <input type="checkbox" value="html" id="html" v-model="picked">
  <label for="html">html</label>
  <input type="checkbox" value="css" id="css" v-model="picked">
  <label for="css">css</label>
  <input type="checkbox" value="js" id="js" v-model="picked">
  <label for="js">js</label>
  new Vue({
  ..,data:{
  picked: ['js']
  }})
  ```

### 下拉列表select-option

- 单选

```html
//开始就会选中html选项，option是备选项，如果有value，v-model就会优先匹配value。例如如果选中js选项，那么selected的值会是javascript，而不是js
<select v-model="selected">
    <option>html</option>
    <option>css</option>
    <option value="javascript">js</option>
</select>
new Vue({..,data:{selected:'html'}})
```

- 多选
  - 如果想实现多选，只要给select添加multiple属性，并给v-model绑定一个数组即可

## 修饰符

- 与事件的修饰符类似，v-model也有修饰符用于控制数据同步的时机

- .lazy

  - v-model默认是在input事件中同步输入的数据，使用修饰符.lazy会转变为在change事件中同步

  ```
  <input type="text" v-model.lazy = "message">
  ```

  - 这样message并不是实时改变的，而是在失焦或按回车时才更新

- .number

  - 使用修饰符.number可以将输入转换为Number类型，否则虽然你输入的是数字，但它的类型其实是string，在数字输入框时会比较有用

  ```
  <input type="number" v-model.number = "message">
  ```

- .trim

  - .trim可以自动过滤输入的首尾空格

  ```
  <input type="text" v-model.trim = "message">
  ```

# 组件详解

- 组件化开发是为了使得开发更加的简洁与更好管理
- 组件命名推荐用小写加减号分割的形式命名，例如a-component，不过如果是单vue文件开发的化，文件命名最好是用大写。类似AComponent

- 注意组件中的data是一个函数，并返回一个对象，我们的数据要写在那里

```
data() {
return {}
}
```

## 模板写法

- ```html
  <script type="text/x-template" id="test">
  模板内容
  </script>
  ```

- ```html
  <template id="test">
      模板内容
  </template>
  ```

## 组件注册

- 全局注册，全局注册后任何vue实例都可以使用

```
//my-component是自定义的组件名
Vue.component('my-component',{
template: '#test',
data() {
return {}}
})
```

- 局部注册

  - 使用components选项，注册后的组件只有在该实例作用域下有效。组件中也可以继续使用components来注册组件，即可以嵌套

  ```
  new Vue({
  el:'#app',
  components: {
  'my-component': {
  	template:'#test'
  }
  }})
  ```

## 父组件像子组件传递数据

- 主要是通过子组件中定义props属性然后父组件进行传值来实现的
- 注意如果不通过v-bind绑定，那么只会传过去字符串
- 如果props属性名为驼峰式，那么在传值时要转换成分隔符写法

```
//这里props共有两种，一种是直接类型限制，一种是对象写法，可以写默认值，注意当类型是对象或者数组时，默认值必须是一个函数，并返回默认值
子组件，假设名字为my-component
{
props: {
	title: String,	//类型限制
	goodList: {			//对象写法，Array类型
	type: Array,
	default() {
	return []
	}},
	name: {			//对象写法，string类型
	type: String,
	default: '名字'
	}
}
}
父组件传值
<my-component :good-list="arrayA" name="codeRobot" :title="标题"></my-component> 
```

## 组件通信

### 父子组件通信

- 通过$emit

```
//子组件，在某个事件发生后
this.$emit('事件名称',传递的参数)	//如果要传递的参数比较多可以传一个对象过去
//父组件接收
<my-component @事件名称="自定义方法名"></me-component>	//在methods里定义该自定义方法即可
```

- 直接在父组件内监听子组件事件，不过要加个.native修饰符

```
<my-component @click.native="事件名"></my-component>
```

### 非父子组件通信

- 利用的是中央事件总线（bus）

```
在Vue构造函数的原型上定义$bus
Vue.prototype.$bus = new Vue()
//发送事件
this.$bus.$emit('事件名'，可选参数)
//接收事件
this.$bus.$on('事件名',回调函数)
//取消事件
this.$bus.$off('事件名'，回调函数)	//如果不传回调函数就会取消所有事件的监听，而加个回调函数就可以限制在										某个组件内
```

## 父组件访问子组件数据

- this.$children可以拿到子组件，返回一个类数组，之后可以通过数组下标再利用.来调用子组件的属性和方法，不常用，一般只在取得所有子组件对象时使用。
- 通过this.$refs.自定义名称

```
<my-component ref="mycom"></my-component>
this.$refs.mycom	//拿到组件对象，里面包含组件对象的数据，方法等
this.$refs.mycom.$el	//拿到组件的根元素
```

## 子组件访问父组件

- this.$parent得到的就是上一级的组件对象有可能是Vue实例，也有可能是某一个组件对象，不常用。
- this.$root可以直接得到Vue实例。

## 插槽

- 组件的插槽是为了使我们的组件更具有扩展性

### 基本使用

- 基本使用

```html
//如果有多个值，会一起作为替换元素
<template id="cpn">
   <div>
       <h1>我是组件</h1>
       <slot></slot>
    </div>
</template>
<cpn>
    <button> 我是按钮</button>
    <p>我是段落</p>
</cpn>
```

- 默认值

```html
//这样即使不传元素，也会有个默认的button，传了就会进行替换
<template id="cpn">
   <div>
       <h1>我是组件</h1>
       <slot><button>我是按钮</button></slot>
    </div>
</template>
```

### 具名插槽的使用

- 就是通过设置多个插槽并对其进行命名来实现更灵活的使用插槽，替换时要通过slot属性指定名称进行替换

```html
//这样就换替换中间的内容两边保持不变。
<template id="cpn">
    <div>
        <slot name="left"><span>左边</span></slot>
        <slot name="center"><span>中间</span></slot>
        <slot name="right"><span>右边</span></slot>
    </div>
</template>
----
<div>
    <cpn><span slot="center">标题</span></cpn>
</div>
```

###  作用域插槽

- 作用域插槽是一种特殊的slot，使用一个可以复用的模板替换已渲染元素。直接上代码吧

```html
//这样就可以取到子组件中的title值，props可以随意定义，只是建立与子组件的连接
<template id="cpn">
    <div>
        <slot :msg="title"></slot>
    </div>
</template>
<cpn>
    <template slot-scope="props">
        <p>
            来自子组件的内容：{{props.msg}}
        </p>
    </template>
</cpn>
```

## 高级组件封装

- 高级组件封装就是为了在任何组件中都可以直接调用该组件，连引入都不用。核心思想就是将它封装在Vue的prototype上。主要就是为了我们平时使用可以更方便的调用。

```javascript
下面以toast组件为例
// toast/Toast.vue
  methods: {	//在组件中定义方法来实现toast显示和隐藏
      showMessage(Message = "默认内容",duartion = 2000) {
          this.message = Message
          this.isShow = true
          setTimeout(() =>{
              this.isShow =false
              this.message = ''
          },duartion)
      }
  }
// toast/index.js
import Toast from './Toast.vue'
const obj = {
    install(Vue) {
        //Vue.extend是基础vue构造器，创建一个子类，参数是一个包含组件选项的对象
        const toastConstructor = Vue.extend(Toast)	
        const toast  = new toastConstructor()
        toast.$mount(document.createElement('div'))	//将toast挂载到div上
        document.body.appendChild(toast.$el)	//将toast组件添加到body上
        Vue.prototype.$toast = toast	//在vue原型上定义toast引用
    }
}
export default obj
// main.js
import toast from 'components/common/toast/index.js'
Vue.use(toast)	//Vue.use会去调用参数对象的install函数

到时使用的时候直接this.$toast.showMessage()传入默认内容与持续时间即可。
```

# Vue进阶上

## $emit和$on用法

- this.$on

```javascript
//$on的第一个事件名可以定义成数组即多个事件名的形式。另外$on方法本身也可以调用多次，那么处理函数就按定义顺序去执行。
//普通的
this.$on("my_events",this.handleEvents)
//多个事件处理函数
this.$on("my_events",this.handleEvents)
this.$on("my_events",this.handleEvents2)
//多个事件
this.$on(['my_events','my_events2'],this.handleEvents)

当然，不要忘了$on接收的是this.$emit发送的事件。
```

## directive指令用法

我们有时需要去定义些自定义指令来辅助我们更好的完成某些操作。

- 注册

  - 全局注册，就是直接

  ```
  Vue.directive('指令名',{
  	//指令选项
  })
  ```

  - 局部注册

  ```
  var app = new Vue({
  	el:'#app',
  	directives: {
  		指令名:{
  			//指令选项
  		}
  	}
  })
  ```

- 自定义指令的选项是由几个钩子函数组成的，每个都是可选的。
  - bind：只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作
  - inserted：被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于document中）
  - update：被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。
  - componentUpdated：被绑定元素所在模板完成一次更新周期时调用
  - unbind：只调用一次，指令与所在元素解绑时调用

- 每个钩子函数都有几个参数可用，例如
  - el：指令所绑定的元素，可以用来直接操作DOM
  - binding：一个对象，包含下列属性
    - name：指令名，不包括`v-`前缀
    - value：指令的绑定值，例如v-my-directive="1+1"，value的值就是2
    - oldValue：指令绑定的前一个值，仅在update和componentUpdated钩子中可用。无论值是否改变都可用。
    - expression：绑定值的字符串形式。例如v-my-directive："1+1"，expression的值是”1+1“
    - arg：传给指令的参数。例如v-my-directive:foo，arg的值是foo
    - modifiers：一个包含修饰符的对象。例如v-my-directive.foo.bar，修饰符对象modifiers的值是{foo：true，bar：true}
  - vnode：Vue编译生成的虚拟节点
  - oldVnode：上一个虚拟节点仅在update和componentUpdated钩子中可用。

```javascript
//配置一个打开页面input自动聚焦的指令
<div id="app">
    <input type="text" v-focus>
</div>
<script>
	Vue.directive('focus',{
		inserted: function(el) {
            //聚焦元素
            el.focus()
		}
    })
	var app = new Vue({
        el: '#app'
    })
</script>
```

## 组件通信inject和provide

主要是为了使得跨组件间的通信更加的方便，传统的props太臃肿，vuex有点大材小用。

provider/inject：简单的来说就是在父组件中通过provider来提供变量，然后在子组件中通过inject来注入变量

需要注意的是这里不论子组件有多深，只要调用了inject那么就可以注入provider中的数据。而不是局限于只能从当前父组件的prop属性来获取数据。

```html
这里写简写形式，能看懂就行
//父组件
<template>
    <div>
        <childOne></childOne>
    </div>
</template>
<script>
    export default {
        name: 'parent',
        provide: {
            for: "demo"
		},
        components: {
			childOne
        }
	}
</script>
//子组件
<template>
    <div>
        {{demo}}
        <childTwo></childTwo>
    </div>
</template>
<script>
    export default {
        name: 'childOne',
		inject: ['for'],
        data() {
            return {
                demo: this.for
            }
        }
        components: {
			chilTwo
        }
	}
</script>
//孙组件
<template>
    <div>
        {{demo}}
    </div>
</template>
<script>
    export default {
        name: 'childTwo',
		inject: ['for'],
        data() {
            return {
                demo: this.for
            }
        }
	}
</script>
```

上述代码在父组件中用provide提供for变量，之后子组件与孙组件分别注入到自己的组件中，最终显示的结果是两个demo。

## 监听器watch

### **1.常见用法**

```javascript
<div id="root">
      <h3>Watch 用法1：常见用法</h3>
      <input v-model="message">
      <span>{{copyMessage}}</span>
</div>
 new Vue({
        el: '#root',
        watch: {
          message(value) {
            this.copyMessage = value
          }
        },
        data() {
          return {
            message: 'Hello Vue',
            copyMessage: ''
          }
        }
      })
```

### **2.绑定方法**

```javascript
<div id="root2">
      <h3>Watch 用法2：绑定方法</h3>
      <input v-model="message">
      <span>{{copyMessage}}</span>
</div>
 new Vue({
        el: '#root2',
        watch: {
          message: 'handleMessage'
        },
        data() {
          return {
            message: 'Hello Vue',
            copyMessage: ''
          }
        },
        methods: {
          handleMessage(value) {
            this.copyMessage = value
          }
        }
      })
```

### **3.deep+handler**

```javascript
<div id="root3">
      <h3>Watch 用法3：deep + handler</h3>
      <input v-model="deepMessage.a.b">
      <span>{{copyMessage}}</span>
</div>
     new Vue({
        el: '#root3',
        watch: {
          deepMessage: {
            handler: 'handleDeepMessage',
            deep: true	//这里注意要设置为true
          }
        },
        data() {
          return {
            deepMessage: {
              a: {
                b: 'Deep Message'
              }
            },
            copyMessage: ''
          }
        },
        methods: {
          handleDeepMessage(value) {
            this.copyMessage = value.a.b
          }
        }
      })
```

### **4.immediate**

immediate设置了就相当于在执行created函数时候执行了一遍监听器，因此页面刚加载完span的内容就已经发生了改变。

```javascript
<div id="root4">
      <h3>Watch 用法4：immediate</h3>
      <input v-model="message">
      <span>{{copyMessage}}</span>
</div>
new Vue({
        el: '#root4',
        watch: {
          message: {
            handler: 'handleMessage',
            immediate: true,
          }
        },
        data() {
          return {
            message: 'Hello Vue',
            copyMessage: ''
          }
        },
        methods: {
          handleMessage(value) {
            this.copyMessage = value
          }
        }
      }),
```

### **5.绑定多个handler**

绑定多个handler有助于更好的处理复杂的监听器逻辑，以下handler函数会依次执行。

```javascript
<div id="root5">
      <h3>Watch 用法5：绑定多个 handler</h3>
      <input v-model="message">
      <span>{{copyMessage}}</span>
</div>
new Vue({
        el: '#root5',
        watch: {
          message: [{
            handler: 'handleMessage',
          },
          'handleMessage2',
          function(value) {
            this.copyMessage = this.copyMessage + '...'
          }]
        },
        data() {
          return {
            message: 'Hello Vue',
            copyMessage: ''
          }
        },
        methods: {
          handleMessage(value) {
            this.copyMessage = value
          },
          handleMessage2(value) {
            this.copyMessage = this.copyMessage + '*'
          }
        }
      })
```

### **6.监听对象属性**

这种相当于deep+handler的优化版

```html
<div id="root6">
      <h3>Watch 用法6：监听对象属性</h3>
      <input v-model="deepMessage.a.b">
      <span>{{copyMessage}}</span>
</div>
 new Vue({
        el: '#root6',
        watch: {
          'deepMessage.a.b': 'handleMessage'
        },
        data() {
          return {
            deepMessage: { a: { b: 'Hello Vue' } },
            copyMessage: ''
          }
        },
        methods: {
          handleMessage(value) {
            this.copyMessage = value
          }
        }
      })
```

## class和style绑定的高级用法

```html
    <div id="root">
      <div :class="['active', 'normal']">数组绑定多个class</div>
      <div :class="[{active: isActive}, 'normal']">数组包含对象绑定class</div>
      <div :class="[showWarning(), 'normal']">数组包含方法绑定class</div>
      <div :style="[warning, bold]">数组绑定多个style</div>
      <div :style="[warning, mix()]">数组包含方法绑定style</div>
      <div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }">style多重值</div>
    </div>
<script>
      new Vue({
        el: '#root',
        data() {
          return {
            isActive: true,
            warning: {
              color: 'orange'
            },
            bold: {
              fontWeight: 'bold'
            }
          }
        },
        methods: {
          showWarning() {
            return 'warning'
          },
          mix() {
            return {
              ...this.bold,
              fontSize: 20
            }
          }
        }
      })
    </script>
```

## Vue2.6新特性

### vue.observable

vue2.6中添加的新特性，可以理解为vue自带的小型vuex，在应用比较简单时使用它做状态管理即可。

```html
 <div id="root">
      {{message}}
      <button @click="change">Change</button>
    </div>
    <script>
      const state = Vue.observable({ message: 'Vue 2.6' })
      const mutation = {
        setMessage(value) {
          state.message = value
        }
      }
      new Vue({
        el: '#root',
        computed: {
          message() { 
            return state.message
          }
        },
        methods: {
          change() {
            mutation.setMessage('Vue 3.0')
          }
        }
      })
    </script>
```

### 插槽v-slot

这里主要介绍的是作用域插槽的用法，以前的写法是`slot-scope`，现在是`v-slot`，另外`v-slot`有个语法糖写法就是直接写`#`。

**1.slot基本用法**

```vue
显示结果为：
    自定义header1
    自定义body2	
<div id="root">
      <div>案例1：slot的基本用法</div>
      <Test>
        <template v-slot:header="{user}">
          <div>自定义header({{user.a}})</div>
        </template>
        <template v-slot="{user}">
          <div>自定义body({{user.b}})</div>
        </template>
      </Test>
</div>
<script>
      Vue.component('Test', {
        template: 
          '<div>' +
            '<slot name="header" :user="obj" :section="\'header\'">' +
              '<div>默认header</div>' +
            '</slot>' +
            '<slot :user="obj" :section="\'body\'">默认body</slot>' +
          '</div>',
        data() {
          return {
            obj: { a: 1, b: 2 }
          }
        }
      })
      new Vue({ el: '#root' })
```

**2.动态slot**

组件还是跟上面一样的模板，因此不重复粘贴

```html
每次点击因为会将section在header和default之间进行切换，所以当section为header时替换的是具名插槽，当为default时替换的就是默认插槽。
因此每次点击组件内容将在以下两种间进行切换：
    this is header
    默认body
或者：
	默认header
	this is body
<div id="root2">
      <div>案例2：Vue2.6新特性 - 动态slot</div>
      <Test>
        <template v-slot:[section]="{section}">
          <div>this is {{section}}</div>
        </template>
      </Test>
      <button @click="change">switch header and body</button>
</div>
<script>
 new Vue({ 
        el: '#root2',
        data() {
          return {
            section: 'header'
          }
        },
        methods: {
          change() {
            this.section === 'header' ?
              this.section = 'default' :
              this.section = 'header'
          }
        }
      })
</script>
```

