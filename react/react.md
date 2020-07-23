# react基本介绍

## 特点

- react主要有以下特点
  - 声明式的设计
  - 高效，采用虚拟DOM来实现DOM的渲染，最大限度的减少DOM的操作
  - 灵活，跟其他库灵活搭配使用
  - JSX，俗称JS里面写HTML，JavaScript语法的扩展
  - 组件化，模块化。代码容易复用
  - 单向数据流，没有实现数据的双向绑定。

## 创建一个项目

- 通过script标签然后通过cdn引入或是通过react.js文件引入。适合调试学习。
- 通过react的脚手架，创建项目进行开发部署。适合真正的项目
  - 安装脚手架`npm  install -g create-react-app`
  - 创建项目，`npm init react-app 项目名`

## react Jsx

- jsx对象中传递变量一般使用，`{}`这种写法，这里也可以定义一些简单的表达式，类似于三目运算符，简单的+，-，*，/这种。大括号中也可以是jsx对象。例如`let element = (<div>我是测试的。</div>)`，`{man=='测试'?<button>按钮</button>:element`}。
- 属性和html内容一样都是用{}插入内容。

- 优点：
  - jsx执行更快，编译为JavaScript代码时进行优化
  - 类型更安全，编译过程如果出错就不能编译，及时发现错误
  - jsx编写模板更加简单快速（不确定和vue哪个快）
- 注意事项：
  - jsx必须要有根节点
  - 正常的普通HTML元素要小写。如果要大写，默认是组件。

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

## 函数式组件

- 函数式组件的名首字母必须要大写，调用的使用要么双尖括号要么单尖括号加/。

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

# react数据传递

## 父传子传递数据

- 父组件传递给子组件数据，单向流动，不能子传递给父。props的传值可以是任意的类型。
- props可以设置默认值，例如`HelloMessage.defaultProps = {name: 'robot'}`
- 基本用法参照上面实例

## 子传父传递数据

- 调用父元素的函数从而操作父元素的数据，从而实现数据从子元素传递至父元素。

```
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

# react事件详解

- react绑定事件的命名，是采用驼峰命名法，例如`onClick`。
- 绑定事件时，必须通过`{}`语法传入一个函数而不是字符串，例如onClick={this.sendData}

- 事件对象，react返回的事件对象是代理的原生的事件对象，如果要查看事件对象的具体值，必须直接输出事件对象的属性。

- 阻止默认行为需要通过`event.preventDefault()`

### react事件传参

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

# 生命周期

- 生命周期即是组件从实例化到渲染到最终从页面中销毁，整个过程就是生命周期，在这生命周期中，我们有许多可以调用的事件，也俗称为钩子函数。
- 生命周期的3个状态
  - Mounting：将组件插入到DOM中
  - Updating：将数据更新到DOM中
  - Unmounting：将组件移除DOM中
- 生命周期中的钩子函数（方法，事件）
  - componentWillMount：组件将要渲染
  - componentDidMount：组件渲染完毕
  - componentWillReceiveProps：组件将要接受props数据，查看接收props的数据是什么。
  - shouldComponentUpdate：组件接收到新的state或者props判断是否更新，返回布尔值。
    - 这个生命周期函数一旦声明，如果我们希望更新就返回true，否则就返回false。如果声明了却不定义返回值那么就会无法更新，因为默认返回的是undefined。
  - componentWillUpdate：组件将要更新
  - componentDidUpdate：组件已经更新
  - componentWillUnmount：组件将要卸载

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

# react的ajax案例

- 这里仅补充一下，axios既可以用来在node中，也可以用在前端中。不过是前端`import`导入，后端`require`导入。
  - 后端的axios我感觉一般是用来做跨域访问的。例如前端请求`/news`路径，而后端针对这个路径又要去请求`https://xxxx.com/news`。

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

```
<Link to="news/45678">新闻</Link>
<Route path="/news/:id" component={News}></Route>
function News(props) { 
		return(
		<div>props.match.params.id</div>
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

## 基本使用

```
import Redux,{createStore} from 'redux'
//用于通过动作创建新的state
//reduce有2个作用，一是初始化数据，第二个就是通过获取动作，改变数据
const reducer = function(state={num:0},action) {
	switch(action.type) {
		case 'add':
			state.num++
			break
		case 'decrement':
			state.num--
			break
	}
	return {...state}
}
//创建仓库
const store = createStore(reducer)
function add() {
	store.dispatch({type:'add'})
}
function decrement() {
	store.dispatch({type:'decrement'})
}
//获取数据
let state = store.getState()
//修改数据(通过动作修改数据)
//通过仓库的dispatch方法进行修改，可以传递额外的参数
store.dispatch({type:'add'},content:{id:1,msg:'helloworld'})
//修改视图（监听数据的变化，重新渲染内容
store.subscribe(()=> {
	ReactDOM.render(<Counter/>,document.querySelector('#root'))
})
```

## react-redux

- 安装，`npm install react-redux --save`。
- Provider组件：自动的将store里的state和组件进行关联

```
import {createStore} from 'redux'
import {Provider,connect} from 'react-redux'
class Counter extends React.Component {
	render() {
		//计数，通过store的state传给props，直接通过props就可以将state的数据获取
		{value} = this.props
		//将修改数据的事件或者方法传入到props，等同于vuex的mapMutation mapStte
		const onAddClick = this.props.onAddClick
		return (
			<div>
				<h1>计数的数量：{value}</h1>
				<button onClick={onAddClick}>数字+1</button>
			</div>
		)
	}
}
const addAction = {
	type: 'add'
}
function reducer(state={num:0},action) {
		switch(action.type) {
		case 'add':
			state.num++
			break
		case 'decrement':
			state.num--
			break
	}
	return {...state}
}
const store = createStore(reducer)
//将state映射到props函数
function mapStateToProps(state) {
	return {
		value: state.num
	}
}
//将修改state数据的方法，映射到props，默认会传入store里的dispatch方法
function mapDispatchToProps(dispatch) {
	return {
		onAddClick: ()=>{dispatch(addAction)}
	}
}
//将上面的这2个方法，将数据仓库的state和修改state的方法映射到组件上，形成新的组件
const App = connect(
	mapStateToProps,
	mapDispatchToProps
)(Counter)
ReactDOM.render(
	<Provider store={store}>
		<App</App>
	</Provider>
,document.querySelector('#root'))
```

- 如果有多个action方法，为了避免那么多的swtich，有下面这种比较简洁的写法

```
let actionObj = {
	'add':function (state,action) {
		state.num++
		return state
	},
	'addNum':function (state,action) {
		state.num = state.num + action.num
		return state
	}
}
function reducer(state={num:0},action) {
	if(action.type.indexOf('redux')!===-1) {
		state = actionObj[action.type](state,action)
		return {...state}
	}
	else{
		return state
	}
}
```

# reactUI

- 这里仅介绍一个代码，`npm run eject`，可以详细展开package.json里的配置代码，进而去配置一些信息。
- ui库，react有`ant design`，`ant design mobile`分别对应移动端和网页端。

# 项目总结

- 数据导入
  - 利用navcat，先创建数据库，然后导入文件，选择json文件，一直下一步即可
- 服务端
  - 这里利用node和mysql来做