# react基本介绍

## 特点

- react主要有以下特点
  - 声明式的设计
  - 高效，采用虚拟DOM来实现DOM的渲染，最大限度的减少DOM的操作
  - 灵活，跟其他库灵活搭配使用
  - JSX，俗称JS里面写HTML，JavaScript语法的扩展
  - 组件化，模块化。代码容易复用
  - 单向数据流，没有实现数据的双向绑定。
- 像`react`这种框架是可以在一个js文件里直接引入css，js等文件的。会有打包工具帮我们处理。

## react中的虚拟DOM

1.  state数据
2.  JSX模板
3.  数据 + 模板生成虚拟DOM（虚拟DOM就是一个JS对象，用它来描述真实的DOM）
4.  用虚拟DOM的结构生成真实的DOM来显示
5.  state发生变化
6.  数据 + 模板 生成新的虚拟DOM
7.  比较原始虚拟DOM和新的虚拟DOM的区别，找到区别的内容
8.  直接操作DOM，改变区别的内容

## 创建一个项目

- 通过script标签然后通过cdn引入或是通过react.js文件引入。适合调试学习。
- 通过react的脚手架，创建项目进行开发部署。适合真正的项目
  - 安装脚手架`npm  install -g create-react-app`
  - 创建项目，`npm init react-app 项目名`，或者`create-react-app 项目名`

## react Jsx

- 编译`jsx`语法必须要引入`react`

- jsx对象中传递变量一般使用，`{}`这种写法，这里也可以定义一些简单的表达式，类似于三目运算符，简单的+，-，*，/这种。大括号中也可以是jsx对象。例如`let element = (<div>我是测试的。</div>)`，`{man=='测试'?<button>按钮</button>:element`}。
- 属性和html内容一样都是用{}插入内容。

- 优点：
  - jsx执行更快，编译为JavaScript代码时进行优化
  - 类型更安全，编译过程如果出错就不能编译，及时发现错误
  - jsx编写模板更加简单快速（不确定和vue哪个快）
- 注意事项：
  - jsx必须要有根节点
  - 正常的普通HTML元素要小写。如果要大写，默认是组件。

## 一些补充

1. `Fragment`可以充当组件的外围占位符，在需要时避免多余的div包裹。使用

```
import {Fragment} from 'react'
```

2. `charles`可以模拟前端mock接口，具体步骤为点击tools，然后点击mapLocal即可
3. `react-transition-group`是基于react的动画框架，可以在github中找到，通过npm进行安装。这要是使用`CSSTransition`这是针对单个元素的，如果是要对一组元素使用动画，那么要用`TransitionGroup`包裹住一组元素，并在其中使用`CSSTransition`包裹单个元素。

4. 如果有时想让列表渲染出的元素直接被转义，可以按下面这样写，用`dangerouslySetInnerHTML`

```react
<ul>
	{this.state.list.map((item) => {
		return (<li dangerouslySetInnerHTML = {{__html: item}}></li>)
	})}
</ul>
```

# react元素渲染

- render函数与基本的jsx写法

```
ReactDOM.render(<App />,document.getElementById('root'))
这里的app是jsx写法的对象，/与<>都是必须的，不加会报错。后面是取得id为root的元素。即将app对象挂载到root元素上。

let h1 = <h1>hellow world</h1> 
使用jsx的写法，可以创建js元素对象
注意：jsx元素对象，或者组件对象，必须只有一个根元素（根节点）。就类似的vue组件我们常用一个div来包裹。
```

- 函数式组件开发

```
// index.js
function Clock(props) {
	return (
		<div>
			<h1>现在的时间是 {props.date.toLocaleTimeString()} </h1>
		</div>
	)
}
function run(){
	ReactDom.render(
	<Clock date= {new Date()}/>,
	document.querySelector('#root')
	)
}
setInterval(run,1000)
```

# react样式和注释

- 如果要在jsx中写label标签的for，必须写成`<label htmlFor="id名">点我</label>`，因为单纯的for属性会与for循环产生歧义。

- react绑定style，在jsx语法里不能用传统的style写法，类似`style="height:200px;"`

```
let exampleStyle = {
	background: 'skyblue',
	borderBottom: '1px solid red'
}
let element = (
	<div>
		<h1 style={exampleStyle}>hello world</h1>
	</div>
)
ReactDom.render(element,document.querySelector('#root'))
```

- react绑定class，注意要用className，因为class是关键字

```
//绑定单个class
let test = ‘bgcolor'
let element = (
	<div>
		<h1 className={test}>hello world</h1>
	</div>
)

//绑定多个class
react不会自动合并多个class，也不会自动合并数组语法，即
<div className="navtop" class={test}></div>   ×，最终只会显示传入的test

let test=['navtop','navbottom']		
<div className={test}>我是测试的</div>		×，最终展示的是class='navtop,navbottom'

let test = ['nvatop','nvabottom'].join(' ')   将其转化为空格分割的字符串之后传入才可以
<div className={test}>我是测试的</div>
或者运用加的表达式来添加多个类名
let element = (
<div>
	<h1 className={"abc "+test}></h1>
</div>
)
```

- react里jsx对象中的注释

```
let element = (
<div>
	{/* 这里写注释 */}
	<h1>hello world</h1>
</div>
)
```

# react组件

- react自己定义的组件必须是大写字母开头，不能是小写。

## 函数式组件

- 函数式组件的名首字母必须要大写，调用的使用要么双尖括号要么单尖括号加/。
- 当类组件只有`render`函数时可以用函数式组件进行代替。

```
//函数式组件定义，并利用props传参，这里props传过来的是个对象，weather是它的一个属性
function ChildCom(props) {
	let title = '我是标题'
	let weather = props.weather
	let isGo = weather==='下雨'?"不出门"："出门"
	return (
	<div>
		<h1>函数式组件{title}，是否出门{isGo}</h1>
	</div>
	)
}
ReactDom.render(<ChildCom weather="下雨"/>,
document.querySelector('#root'))
```

## 类组件

- 类组件继承自React.Component

```
类组件定义,传参也是直接通过props，不过是this.props.weather
class HelloWorld extends React.Component {
	render() {
			return (
				<div>
				 <h1>类组件</h1>
				</div>
			)
		}
}
ReactDom.render(<HelloWorld weather="下雨"/>,
document.querySelector('#root'))
```

## 函数式组件与类组件区别

- 函数式组件比较简单，一般用于静态没有交互事件内容的组件页面
- 类组件，一般又称为动态组件，一般会有交互或者数据修改的操作。

- 组件中可以进行嵌套，直接写组件对象即可。

## 组件与props

类似于插槽的作用，动态控制组件中的值

```javascript
//函数式组件
function Test(props) {
	return (
	<div>{props.name}呵呵</div>)
}
//类组件
class Welcome extends React.Component {
	render() {
		return (<div>{this.props.name>呵呵</div>)
	}
}

function App() {
	return (
	<div>
		<Test name="哈哈哈"></Test>
		<Welcome name="嘿嘿嘿"></Welcome>
	</div>)
}
```

## props,state与render函数之间的关系

每当一个组件的`state`或者`props`发生改变的时候，它自己的`render`函数就会被执行一次。

另外，当父组件的render函数调用时，那么子组件的render函数也会被调用。

## 组件优化

React.PureComponent 与 React.Component 几乎完全相同，但 React.PureComponent 通过props和state的浅对比来实现 shouldComponentUpate()。

### PureComponent区别点：

**PureComponent缺点**

可能会因深层的数据不一致而产生错误的否定判断，从而shouldComponentUpdate结果返回false，界面得不到更新。

**PureComponent 优势**

不需要开发者自己实现shouldComponentUpdate，就可以进行简单的判断来提升性能。

# react State

- 相当于vue中的data，不过使用方式有区别。
- react没有实现数据的双向绑定，即数据改变并不会自动重新渲染页面。我们为了达到这种效果需要调用它的setState方法来进行修改。
- 通过this.setState修改完数据后，并不会立即修改DOM里面的内容，react会在这个函数的所有设置状态改变之后，统一对比虚拟DOM对象，然后再统一修改，提升性能。

```
class Clock extends React.Component {
	constructor(props) {
		//获得父类的this对象，并传递props参数以便this.props可以调用
		super(props)
		//定义state
		this.state = {
			time: new Date().toLocaleTimeString()
		}
	}
	render() {
		return (
			<div>
				<h1>现在时间是：{this.state.time}</h1>
			</div>
		)
	  }
	//生命周期函数，下面这个类似于vue中的mounted，即组件挂载完成调用
	componentDidMount() {
		setInterval(()=>{ 
			this.setState({
				time:new Date().toLocaleTimeString()
				})
		},1000)
	}
}
```

- 现在`setState`更合适的写法是括号里传入回调函数

```
这里的prevState指代的是上一次的state，不是必须传入
this.setState((prevState) => {
	return {
		list: [...prevState.list, prevState.inputValue]
		inputValue: ''
	}
})
```

`setState`还有第二个参数，是一个回调函数，当`setState`执行完之后进行调用

```
this.setState((prevState) => {
	return {
		list: [...prevState.list, prevState.inputValue]
		inputValue: ''
	}
}, () => {
	console.log('setState调用完了就调用我了')
})
```

# react数据传递

## 父传子传递数据

- 父组件传递给子组件数据，单向流动，不能子传递给父。props的传值可以是任意的类型。
- props可以设置默认值，例如`HelloMessage.defaultProps = {name: 'robot'}`
- 基本用法参照上面实例

## 子传父传递数据

- 调用父元素的函数从而操作父元素的数据，从而实现数据从子元素传递至父元素。

```react
class FatherCom extends React.Component{
	constructor(props) {
		super(props)
		this.state = {
			childData: null
		}
	}
	render() {
		return (
			<div>
				<h1>子元素传递给父元素的数据：{this.state.childData}</h1>
				<ChildCom setChildData={this.setChildData}/>
			</div>
		)
	}
	setChildData = (data)=> {
		this.setState({
			childData: data
		})
	}
}
class ChildCom extends React.Component {
	constructor(props) {
		super(props)
		this.state = {
			msg: 'helloworld'
		} 
	}
	render() {
		return (
		  <div>
		  	<button onClick={this.sendData}>传递helloworld给父元素</button>
		  </div>
		)
	}
	sendData = ()=> {
		this.props.setChildData(this.state.msg)
	}
}
ReactDOM.render(<FatherCom/>,
document.querySelector('#root'))
```

## PropTypes与DefaultProps的应用

通过PropTypes限制传值类型

```react
TodoItem.js
import PropTypes from 'prop-types'
TodoItem.propTypes = {
    content: PropTypes.string,	//限定为字符串
    deleteItem: PropTypes.func,		//限定为函数
    index: PropTypes.number		//限定为数字
}
```

通过DefaultProps定义默认传递的值

```react
TodoItem.defaultProps = {
	test: 'hello world'
}
```

更多细节运用请参阅官网。

# react事件详解

- react绑定事件的命名，是采用驼峰命名法，例如`onClick`。
- 绑定事件时，必须通过`{}`语法传入一个函数而不是字符串，例如onClick={this.sendData}

- 事件对象，react返回的事件对象是代理的原生的事件对象，如果要查看事件对象的具体值，必须直接输出事件对象的属性。

- 阻止默认行为需要通过`event.preventDefault()`

## react事件传参

- 不传参时默认会传入一个event事件对象，当需要传入参数时要利用箭头函数，因为直接通过括号传参在react中相当于执行了。所以代码如下

```
onClick={(e)=>{this.sendData(e,'我是传参的内容')}}		
sendData = (e,msg)=>{
	console.log(e)
	console.log(msg)
}
```

# react渲染

## 条件渲染

- react中条件渲染即和js类似，是利用条件运算，例如if..else，三元运算符等来实现的

```javascript
//普通写法
function FisrtCom(props) {
 	return (<div>请登录</div>)
}
function SecondCom(props){
	return (<div>登录成功</div>)
}
class LoginCom extends React.Component {
	counstructor(props) {
		super(props)
		this.state = {
			isLogin: false
		}
	}
	render() {
		if(this.state.isLogin) {
			return <SecondCom/>
		}
		else{
			return <FisrtCom/>
		}
	}
}
//或者也可以利用类似插槽的实现
class LoginCom extends React.Component {
	counstructor(props) {
		super(props)
		this.state = {
			isLogin: false
		}
	}
	render() {
		let element = null
		if(this.state.isLogin) {
			element = <FisrtCom/>
		}
		else{
			element = <SecondCom/>
		}
		return (
		<div>
			<h1>展示状态信息</h1>
			{element}
			<h2>这是尾部</h2>
		</div>
		)
	}
}
//三元运算符简洁写法
render() {
		return (
		<div>
			<h1>展示状态信息</h1>
			{this.state.isLogin?<FirstCom/>:<SecondCom/>}
			<h2>这是尾部</h2>
		</div>
		)
	}
```

## 列表渲染

- 列表渲染主要是通过数组来实现的

```javascript
//普通实现，返回jsx对象，注意要加key属性
class Com extends React.Component {
	counstructor(props) {
		super(props)
	}
	render() {
	let listarr = this.state.list.map((item,index) => {
	return (
			<li key={index}>
			{index}:{我是标题:item.title}
			{我是内容:item.content}
			</li>
			)
	})
		return (
		<div>
			<ul>
			{listarr}
			</ul>
		</div>
		)}}
//返回函数式组件对象，也要有key属性，不过key是加在组件对象上
function ListItem(props) {
    return (
    <li>
        {props.index}:{我是标题:props.item.title}
		{我是内容:props.item.content}
    </li>
    )
}
class Com extends React.Component {
    ...
    render() {
	let listarr = this.state.list.map((item,index) => {
	return (<ListItem data={item} index={index} key={index}/>)
	})
    return (
		<div>
			<ul>
			{listarr}
			</ul>
		</div>
   )
}
```

# react生命周期函数

## 基本介绍

- 生命周期即是组件从实例化到渲染到最终从页面中销毁，整个过程就是生命周期，在这生命周期中，我们有许多可以调用的事件，也俗称为钩子函数。
- 生命周期的3个状态
  - Mounting：将组件插入到DOM中
  - Updating：将数据更新到DOM中
  - Unmounting：将组件移除DOM中
- 生命周期中的钩子函数（方法，事件）
  - constructor
  - getDeriverStateFromProps（新版）/componentWillMount（旧版）： 组件将要挂载；只会在第一次挂载的时候被执行。
  - render
  - componentDidMount：组件渲染完毕，只会在第一次挂载的时候被执行。
  - componentWillUnmount：组件将要卸载
  - shouldComponentUpdate：组件接收到新的state或者props判断是否更新，返回布尔值。组件被更新之前，他会自动被执行。
    - 这个生命周期函数一旦声明，如果我们希望更新就返回true，否则就返回false。如果声明了却不定义返回值那么就会无法更新，因为默认返回的是undefined。
  - componentWillUpdate（旧版）：组件被更新之前，它会自动执行，但是如果shouldComponentUpdate返回true它才会执行，否则就不会。
  - render
  - getSnapshotBeforeUpdate（新版）
  - componentDidUpdate：组件已经更新
- 一个特殊的`componentWillReceiveProps`生命周期函数。它的执行必须要满足  
  - 所在组件必须要用父组件接受参数
  - 如果这个组件第一次存在于父组件中，不会执行。
  - 如果这个组件之前已经存在于父组件中，才会执行。

```javascript
例如：
正常页面加载顺序
constructor
getDeriverStateFromProps
render
componentDidMount

改变页面state数据
getDeriverStateFromProps
shouldComponentUpdate
render
getSnapshotBeforeUpdate
componentDidUpdate

组件卸载
componentWillUnmount
```

## 图解

<img src="C:\Users\22140\Desktop\web-study\note\assets\img\image-20201019125452856.png" alt="image-20201019125452856" style="zoom:200%;" />

## 实际应用

1. 例如当父组件数据一更新子组件的render也会被调用，可以通过`shouldComponentUpdate`来优化

```
shouldComponentUpdate(nextProps, nextState) {
	if(nextProps.content !== this.props.content){
		return true
	}else {
		return false
	}
}
```

2. 网络请求最好是放在`componentDidMount`里，放在别的生命周期函数里会产生或多或少的问题。

# ref绑定Dom对象

react推荐尽量不要直接操作dom，因为可能会出现一些意料之外的错误。

- 第一种

```
this.state = {
	textInput:React.createRef()
}
<input ref = {this.state.textInput}>
```

这样直接通过`this.state.textInput.current`就可以获取dom元素对象。

- 第二种

ref是一个回调函数，参数起名随意

```
<input ref = {(input) => {this.input = input}} >
```

这样组件内`this.input`就指向该input dom元素。

# 表单

- 表单输入必须绑定`value`和`onChange`事件
- onKeyDown：键盘按下事件，event事件里有具体的键盘编码。可以利用它来绑定一些键盘的快捷键操作。

```
<input type="text" value={this.state.value} onChange={this.changeEvent}/>
changeEvent(e) {
	this.setState({
		value: e.target.value
	})
}
```

# 高阶组件

高阶组件就是一个函数，这个函数的参数是一个组件，并且能够返回一个组件。本质上就是对组件的加工

```javascript
例如
function A(props) {
	return (
	<div>
		<h1>{props.user.name + props.user.age}</h1>
	</div>)
}
function B(props) {
	return (
	<div>
		<h2>{props.user.name + props.user.age}</h2>
	</div>)
}
function Parent(Componenet) {
    let user = {name: 'hehe', age: 18}
    return ()=> <Component user= {user}/>	//注意这里因为要返回的是组件所以写成函数式组件形式
}
之后
let highA = Parent(A)
let highB = Parent(B)
就可以得到两个加工后的组件
```

# react的ajax案例

- 这里仅补充一下，axios既可以用来在node中，也可以用在前端中。不过是前端`import`导入，后端`require`导入。
  - 后端的axios一般是用来后端请求数据的。例如前端请求`/news`路径，而后端针对这个路径又要去请求`https://xxxx.com/news`。

# react插槽

- 组件中写入内容，这些内容可以被识别和控制。React需要自己开发支持插槽功能。
  - 原理是，组件中写入的HTML，可以传入到props中。

```html
//组件对象中写入的html会存到props的children中，因此可以按照下面来进行渲染。
//普通插槽
class FatherCom extends React.Component {
	...
	render() {
		return(
		<div>
			我是父组件
			{this.props.children}
		</div>
		)
	}
}
ReactDOM.render(<FatherCom>
	<h2>我是插槽内容</h2>
	<h2>我是插槽内容</h2>
	<h2>我是插槽内容</h2>
</FatherCom>,
document.querySelector('#root'))

//具体位置的插槽
class FatherCom extends React.Component {
	..
	render() {
		return (
		<ChildCom>
			<h2 data-position="header">我是头部内容</h2>
			<h2 data-position="middle">我是中间内容</h2>
			<h2 data-position="foot">我是尾部内容</h2>
		</ChildCom>
		)
	}
}
class ChildCom extends React.Component{
	..
		render() {
		let headerCom,middleCom,footCom
		this.props.children.forEach((item) => {
			let a = item.props['data-position']
			if(a==='header') {
				headerCom = item
			}else if(a==='middle') {
				middleCom = item	
			}else {
				footCom = item
			}
		})
		return (
		<div>
			<div class="header">
                {headerCom}
            </div>
            <div class="middle">
                {middleCom}
            </div>
            <div class="foot">
                {footCom}
            </div>
		</div>
		)
	}
}
```

# react路由

- react路由需要自己安装，`npm install react-router-dom --save`
- ReactRouter三大组件。如果要精确匹配，可以在route上设置exact属性。
  - Router：所有路由组件的根组件（底层组件），包裹路由规则的最外层容器
  - Route：路由规则匹配组件，显示当前规则对应的组件
  - Link：路由跳转的组件

## 路由导入与基本使用

```html
app.js
//hash模式
import {HashRouter as Router,Link,Route} from 'react-router-dom'
//history模式
import {BrowserRouter as Router,Link,Route} from 'react-router-dom'

class App extends React.Component {
...
render() {
	return (
	<div>这里可以写外部内容，即所有路由节目都有的内容</div>
//basename设置的是路由的根路径。就是点击别的路径时会自动在前面加上她
	<div id="app" basename="/">
        //一个组件中可以写多个router，有啥用我也没感觉出来，目前看来可以通过不同的路径指向同一组件
		<Router>
			<div className="nav">
                <Link to="/">Home</Link>
                <Link to="/product">product</Link>
                <Link to="/me">个人中心</Link>
			</div>
            <Route path="/" exact component={Home}></Route>
            <Route path="/product"  component={Product}></Route>
            <Route path="/me"  component={Me}></Route>
		</Router>
	</div>
	)
}}
```

## 路由link标签补充

- link标签的to可以写成对象的形式

```
let meobj = {
	pathnam:'/me',	//路径设置
	seacrch:"?username=admin",	//get请求参数
	hash:"#abc",	//hash值
	state:{msg:'helloworld'}	//传递的数据
}
<Link to={meobj}></Link>
之后，我们在相应的组件的props属性中取得相关的数据,props.location.属性名
```

- link标签默认进行的路由操作是可以进行回退，前进的。不过我们可以设置replace属性来消除历史记录。

```
<Link to="/me" replace>me</Link>
```

- 动态路由

```javascript
<Link to="news/45678">新闻</Link>
<Route path="/news/:id" component={News}></Route>
function News(props) { 
		return(
		<div>props.match.params.id</div>	//获取到id
		)}
```

## 路由重定向

- 重定向也是一个组件，需要导入，`import {Redirect} from 'react-router-dom'`

```
function LoginInfo(props) {
	if(props.loginState==='success'){
		return <Redirect to="/admin"></Redirect>
	}else{
		return <Redirect to="/login"></Redirect>
	}
}
```

## Switch组件

- 让Switch组件内部的route只匹配一个，只要匹配到了剩余的规则将不再匹配

```html
这样输入/abc，就只会显示一个abc内容
import {Switch} from 'react-router-dom'
<Router>
	<Switch>
		    <Route path="/" exact component={Home}></Route>
            <Route path="/product"  component={Product}></Route>
            <Route path="/me"  component={Me}></Route>
       		<Route path="/abc" component=()=>{return <div>abc内容</div>}</Route>
          	<Route path="/abc" component=()=>{return <div>abc内容</div>}</Route>
	</Switch>
</Router>
```

## 代码跳转路由

```
//事件函数
clickEvent = (e)=>{
	this.props.history.push('/',{msg:'这是我发给首页的内容'})
	this.props.history.replace('/')	
	this.props.history.go(1)	//前进
	this.props.history.go(-1)	//后退
	this.props.history.goBack()	//后退
	this.props.history.goForward()	//前进
}
```

# redux

## 介绍

- 解决React数据管理，用于中大型，数据比较庞大，组件之间数据交互多的情况下使用。
  - 解决组件的数据通信
  - 解决数据和交互较多的应用
- redux相关属性介绍
  - Store：数据仓库，保存数据的地方
  - State：state是一个对象，数据仓库里的所有数据都放到一个state里。
  - Action：一个动作，触发数据改变的方法。
  - Dispatch：将动作触发成方法
  - Reducer：是一个函数，通过获取动作改变数据。生成一个新state，从而改变页面
- 安装，`npm install redux --save`。
- 图解

![image-20201019160523479](C:\Users\22140\Desktop\web-study\note\assets\img\image-20201019160523479.png)

可以把这几个想象成

ReactComponent：借书人

ActionCreators：我要说的话，例如要借什么书，要改哪本书

Store：图书馆的管理员，负责管理图书但是不知道具体的图书在哪，只能通过reducer这个笔记本。

Reducers：笔记本。

## 基本使用

1. 新建store文件夹，并新建`index.js`文件创建并导出store，`reducer.js`文件创建并导出reducer函数，用来初始化state和action

```javascript
//reducer.js
const defaultState = {		//初始的数据，也可以是空对象
	inputValue: '123',
	list: [1, 2]
}
export default (state = defaultState, action) => {
	return state
}

//index.js
import {createStore} from 'redux'
import reducer from './reducer'
const store = createStore(reducer)	//通过reducer创建store
export default store
```

2. 组件内使用，引入store，store本身有一个`getState`方法可以获取state数据，例如

```javascript
import store from './store/index'
...{
	this.state = store.getState()	//state会获得reducer里的数据
}
```

## action和reducer的编写

当需要改变state数据时需要编写action

1.  创建action，必需要有type属性，然后也可以传递额外参数

2.  通过dispatch将action传递给store

   ```javascript
   handleInputChange(e) {
   	const action = {
   		type: 'change_input_value',
   		value: e.target.value
   	}
   	store.dispatch(action)	//传递action
   }
   ```

3.  store自动把action传递给reducer，reducer根据action的类型改变并返回新state

```javascript
//reducer.js
reducer 可以接受state，但是绝不能修改state，所以才要进行拷贝修改
export default (state === defaultState, action) => {
	if(action.type === 'change_input_value') {
		const newState = JSON.parse(JSON.stringify(state))	//复制一份数据
        newState.inputValue = action.value
        return newState
	}
    return state 
}
```

4.  组件内通过subscribe监视store的改变，只要store一改变就会自动调用，然后通过该回调函数可以改变页面数据

```javascript
..
constructor(props) {
	this.handleStoreChange = this.handleStoreChange.bind(this)
	store.subscribe(this.handleStoreChange)	//store一改变自动调用的函数
}
handleStoreChange() {
    this.setState(() => {
       return store.getState()
    })
}
```

## redux编写优化

### ActionTypes的拆分

为了防止action书写出错，我们可以在store文件夹下新建`actionTypes.js`文件，就是用常量代替变量，可以避免出错。

```javascript
export const CHANGE_INPUT_VALUE = 'change_input_value'
....
```

然后分别在`reducer.js`和组件中引入该文件，用常量代替变量。

### 使用actionCreator统一创建action

在store文件夹下新建`actionCreators.js`文件，从`actionTypes.js`文件中导入常量，编写函数

```javascript
import {CHANGE_INPUT_VALUE} from './actionTypes'
export const getInputChangeAction = (value) => ({
	type: CHANGE_INPUT_VALUE,
	value
})
```

之后组件中导入需要的函数调用即可

```javascript
import {getInputChangeAction} from 'actionCreators'
...
const action = getInputChangeAction(value)
```

## redux进阶

### 什么是redux中间件

redux中间件就是对store的`dispatch`方法进行升级，以前只能接收对象，现在可以接收函数了。还有很多别的中间件

![image-20201020114309258](C:\Users\22140\Desktop\web-study\note\assets\img\ReduxDataFlow.png)

### 使用redux-thunk中间件实现ajax数据请求

直接在github搜`redux-thunk`即可详细了解

1. 安装`npm install redux-thunk --save`
2. 在redux中引入`applyMiddleware`，从redux-thunk中引入thunk，创建store时传入参数

```javascript
import {createStore, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'
const store = createStore(reducer,
	applyMiddleware(thunk)
)

因为redux_devtools也是中间件，并且也要传入，所以有一种特殊的写法，参照下面
import {compose} from 'redux'
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?   window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;

const enhancer = composeEnhancers(
  applyMiddleware(thunk),	//如果是多个中间件，就一个个写进去
);
const store = createStore(reducer, enhancer);
```

3. 当你使用了这个中间件后，action就可以不是一个对象而是一个函数，从而可以在其中写异步的代码，这时候再通过`store.dispatch(action)`可以执行该函数，且该函数会默认传入dispatch方法，因此可以在函数中通过`dispatch`去修改state的值。

```javascript
//actionCreators.js
export const initListAction = (data) => ({
	type: INIT_LIST_ACTION,
	data	
})
export const getTodoList = () => {
	return (dispatch) => {
		axios.get('./list.json').then((res) => {
			const data = res.data
			cosnt action = initListAction(data)
			dispatch(action)
		})
	}
}
//组件中
import {getTodoList} from './store/actionCreators'
...
componentDidMount() {
    const action = getTodoList()
    store.dispatch(action)
}
```

### redux-saga中间件使用

github也可以直接搜索到它了解详情，使用了它之后`store.dispatch(action)`就不止reducer可以接收到，sagas也可以接收到。

1. 安装`npm install redux-saga --save`
2. 导入并使用

```javascript
import { createStore, applyMiddleware } from 'redux'
import createSagaMiddleware from 'redux-saga'	//导入创建saga中间件的方法
import mySaga from './sagas'	//导入自己的saga文件
// create the saga middleware
const sagaMiddleware = createSagaMiddleware()

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?   window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose;
const enhancer = composeEnhancers(
  applyMiddleware(sagaMiddleware),	//如果是多个中间件，就一个个写进去
);

// mount it on the Store
const store = createStore(
  reducer,
  enhancer
)

sagaMiddleware.run(mySaga)	//运行自己的saga
```

3. 在store文件夹下新建`sagas.js`文件用来存放异步逻辑

```javascript
import {takeEvery, put} from 'redux-saga/effects'
import {GET_INIT_LIST} from './actionTypes'		//导入自己的action的type名
import {initListAcion} from './actionCreators'
//异步请求一般就会写在该回调函数里，注意generator函数里异步请求不要使用promise这种形式
//为了防止网络请求失败，最好做try,catch异常处理
function* getInitList() {
    try {
    cosnt res = yield axios.get('./list.json')
    const action = initListAction(res.data)
    yield put(action)
    }
	catch(e) {
        console.log('请求失败')
    }
}
function* mySaga() {
    //监听action，一旦GET_INIT_LIST这种action发出那么就会自动自动调用getInitList方法
	yield takeEvery(GET_INIT_LIST, getInitList)		
}
export default mySaga
```

### ui组件和容器组件

就是避免一个组件过于紊乱，然后将其分为ui组件和容器组件，ui组件就只负责页面显示，容易组件存放数据，函数等，然后通过props传递给ui组件。

### react-redux

是为了简化`redux`的使用过程而产生的，是在`redux`存在的基础上存在的，store文件夹下的`index.js`和`reducer.js`不用动，主要是根组件以及使用组件的变化

1. 安装，`npm install react-redux --save`
2. 在比较根的组件例如index.js中导入并使用

```react
import {Provider} from 'react-redux'
import store form './store/index'
import TodoList from './TodoList'
const App = (
	<Provider store = {store}>
        //将需要使用react-redux的组件包裹在provider中，这样它里面的所有组件都可以获取store里面的数据
        <TodoList></TodoList>	
    </Provider>
)
ReactDom.render(App, document.getElementByID('root'))
```

3. 在需要的组件中进行使用。将组件与store进行连接，并有自己的连接方式

```react
import connect from 'react-redux'
class TodoLIst extends Component {
    ..
}
//将state中的数据映射到组件的props中
const mapStateToProps = (state) => {
    return {
        //这样我们组件中就可以通过this.props.inputValue获取数据了
        inputValue: state.inputValue	
    }
}
//将改变state行为的dispatch映射到props中，这里的dispatch就等于store.dispatch
const mapDispatchToProps = (dispatch) => {
    return {
        //这样组件中通过this.props.handleInputChange就可以调用该方法了从而改变state
        //当然reducer中还是要根据action的type值来进行处理
        handleInputChange(e) {
            const action = {
                type: 'input_value_change',
                value: e.target.value
            }
            dispatch(action)
        }
    }
}
//让我们的TodoList组件和store做连接，因为我们的组件在provider中，因此可以连接
export default connect(mapStateToProps, mapDispatchToProps)(TodoList)	
```

### 使用combineReducers完成对数据的拆分

当reducer数据过多时，可以利用combineReducers进行拆分

```javascript
import {combineReducers} from 'redux'
import headerReducer from '../common/header/store/reducer'	//导入小reducer
const reducer = combineReducers({
	header: headerReducer	//这里header属性名可以随意起
})
export default reducer
```

注意当这样拆分后，如果想用`react-redux`，那么创建`mapStateToProps`时回调函数应该是`state.header.属性名`。

### immutable.js来管理store中的数据

因为reducer不能直接修改state，但是难免有时会忘记，因此可以用`immutable`。

**注意immutable类型的数组不支持直接通过下标获取元素，只能变相进行类型转换。**`const jsList = list.toJS()`

1. `npm install immutable --save`
2. 引入使用

```javascript
// reducer.js
import {fromJS} from 'immutable'
//通过fromJS函数生成一个immutable对象
const defaultState = fromJS({
	focused: false
})
```

3. 当使用了`immutable`之后就不能在组件中直接通过`state.属性名`获取数据了，需要通过`state.get('属性名')`来获取，另外reducer.js文件中要返回成下面这种

```javascript
//reducer.js
export default (state = defaulteState, action) => {
    if(action.type === ...) {
       // 如果特别多可以用merge，例如
       		return state.merge({
       			list: action.data,
       			totalPage: action.totalPage
       		})
      //  如果有多个属性要设置可以进行链式调用set
       		return state.set('属性名',属性值)		
      		}
    return state
}
```

4. immutable类型的数组没有`length`属性，只有`size`属性。

### 使用redux-immutable来统一数据格式

这个包一般是当自己拆分了reducer后使用，因为通过`state.header.get('属性名')`很不一致，我们要达到`state.get('header').get('属性名')`的效果

1. `npm install redux-immutable --save`
2. store文件夹下的index.js文件中的`combineReducers`由`redux-immutable`生成

```javascript
import {combineReducers} from 'redux-immutable'
import headerReducer from '../common/header/store/reducer'	//导入小模块reducer
const reducer = combineReducers({
	header: headerReducer	//这里header属性名可以随意起
})
export default reducer
```

# reactUI

- 这里仅介绍一个代码，`npm run eject`，可以详细展开package.json里的配置代码，进而去配置一些信息。
- ui库，react有`ant design`，`ant design mobile`分别对应移动端和网页端。
