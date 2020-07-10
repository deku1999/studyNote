# 防抖函数

- 短时间内频繁进行某个操作时可以设置延迟对它进行防抖处理使得执行的性能更高。

```javascript
//这样就对test函数进行了防抖处理，执行test2时以200ms为延迟进行检测
function debounce(func,delay) {
let timer = null
return function(...args) {
if(timer) 
{clearTimetout(timer)}
timer = setTimeout(() => {
func.apply(this,args)},delay)
}
}
const test2 = debounce(test,200)
test2()
```

# 反引号字符串`

- 反引号定义的字符串支持换行，可以更方便的拼接。

```javascript
var foo = `Hello hehe
床前明月光
疑似地上霜`
console.log(foo)	//输出的结果就是排版的结果
```

- 在es6的`字符串中，可以使用${ }来引用变量，变量的值会被渲染进字符串中。

# 模块化

- 这里介绍下common Js和es6的模块化

## commonJS

```
//test.js，导出
module.exports = {
name :'robot'
age: 18,
job: 'fullStackEngineer'
}
//nedd.js，导入
const userInfo = require('./test.js')
console.log(userInfo.name) 	//robot
```

## es6

- 导出

  - 导出方式1，一整个对象导出

  ```
  export {
  flag,sum
  }
  ```

  - 导出方式2，一个一个导出

  ```
  export var num1 = 1000
  export const obj = {age:18}
  export function sum() {}
  export class Person{}
  ```

  - 导出默认值，好处就是导入的时候可以重命名。不过一般一个js文件只能有一个默认导出，不然导入的时候会发生混乱。

  ```
  const address = "Beijing"
  export default address
  export default function() {}
  export default {}
  ```

- 导入

  - 一般导入

  ```
  import {flag,sum} from './test.js'
  ```

  - 导入默认值

  ```
  重命名address并导入
  import addr from './add.js'
  ```

  - 统一导入，主要是为了方便普通的导出

  ```
  //将test文件中导出的全部包含在一个testInfo对象里，之后通过该对象就可以调用导出的值
  import * as testInfo from './test.js'
  ```

# let与const

- 传统的es6之前的版本中的var定义是个缺陷，因为它没有块级作用域，例如大括号里定义的变量外部也可以访问。它的作用域只能通过函数来加以限制。es6中，用let定义的变量是有类似if，for这样的块级作用域了，就不用有时再借用闭包来解决问题。

## const注意点

- 一是常量不能被修改，否则会报错；
-  二是定义的时候就要赋值
- 三是常量的含义是指向的对象不能被修改，不过对象内部的属性可以进行修改不会报错，因为引用类型实际上存储的是对象的地址，所以就是地址不能修改。

## 建议

在es6开发中，优先使用const，只有需要改变某一个标识符的时候才使用let。

# 对象字面量增强写法

```javascript
const name = 'ccc'
const obj = {
name,
test() {
console.log('hehe')}
}
```

# 遍历对象数组

```javascript
for(let item of arrayA){}
```

这样拿到的item就直接是数组元素

# class用法

- es6中的构造函数

```javascript
class Person = {
constructor(obj1,obj2) {
this.name = obj.name
this.title = obj2.title
}
}
var obj = new Person(obj1,obj2)
console.log(obj) // {name: '..', title: '...'}
```

# 拓展运算符...

- 该运算符也可以用来写在函数参数上，以表示不确定参数的个数

```javascript
function sum(...count) {
    return test.apply(this,count)
}
```

- 可以用来取出参数对象所有可遍历属性然后拷贝到当前对象。
- 或者取出参数数组所有元素并拷贝到当前数组

```javascript
//一次性推入数组中的多个元素
var a = [],list1 = [1,2,3,4,5]
a.push(...list1)
console.log(a) // [1,2,3,4,5]
```

```javascript
//克隆数组
var list1 = [23,45,54,342,342,'e4','134']
var a = [...list1]
console.log(a) //[23,45,54,342,342,'e4','134']
//注意这是克隆数组，即并不会改变原数组
```

```javascript
//克隆对象
const obj = {
	name: 'john',
	age: '13',
	job: 'student'
}
const objb = {...obj}
console.log(objb) //{name: 'john',age: '13',job: 'student'}
```

# 箭头函数

## 初始形式

```javascript
() => {}
```

## 简写

- 当只有一个参数，或返回值只有一行代码时，可以这样写

```
item => item*item
```

## 使用时机

- 一般当我们准备将一个函数作为参数传到另一个函数中的时候，这个时候该函数参数一般用箭头写法。

## this指向问题

- 箭头函数的this指向为向外层作用域中，一层层查找this，直到有this的定义就指向该this。

# Promis

- Promise可以以一种非常优雅的方式来处理异步操作，避免掉入回调地狱。
- 一般情况下是有异步操作时，使用promise对这个异步操作进行封装

## 基本使用

```javascript
new Promise((resolve,reject) => {
setTimeout(() =>{
resolve('hello world')
// reject('error message')
},2000)
}).then(res => 
{console.log(res)
}).catch((err) => {
console.log(err)})
```

成功就调用then，失败就调用catch。传入的数据分别就是resolve和reject的参数。

## 三种状态和另外处理方式

- Promise有等待，成功，失败三种状态。
- 如果不想分别写then和catch，其实then方法可以同时传入两个函数参数，第一个表示成功，第二个表示失败

```javascript
new Promise((resolve,reject) => {
setTimeout(() =>{
resolve('hello world')
// reject('error message')
},2000)
}).then(res => 
{console.log(res)
},err => {
    console.log(err)
})
```

## 链式调用的额外写法

### resolve简洁写法

1. 直接return Promise.resolve

```javascript
new Promise((resolve,reject) => {
setTimeout(() =>{
resolve('hello world')
// reject('error message')
},2000)
}).then(res => {
    return Promise.resolve(res+'222')
}).then(res => {console.log('第二层')})
```

2. 直接return 数据

```javascript
new Promise((resolve,reject) => {
setTimeout(() =>{
resolve('hello world')
// reject('error message')
},2000)
}).then(res => {
    return res+'222'
}).then(res => {console.log('第二层')})
```

### reject简洁写法

1. 直接return Promise.reject()，该种方法和resolve的基本一样，不多赘述
2. 手动throw抛出异常信息

```javascript
new Promise((resolve,reject) => {
setTimeout(() =>{
 reject('error message')
},2000)
}).catch( err => {
    throw 'error message'
}).catch(err => {console.log('err message')})
```

## all方法使用

- all方法一般使用场景是需要同时请求多个请求进行处理时比较好用。这里的then里的results参数是一个数组，保留着从第一次请求到最后一次请求的数据。

```javascript
Promise.all([
new Promise((resolve,reject) =>{
$.ajax({
url: 'url1',
success: (data) => {resolve(data)}
}),    
}),
new Promise((resolve,reject) =>{
$.ajax({
url: 'url2',
success: (data) => {resolve(data)}
}),    
})
]).then(results => {console.log(results)})
```

# symbol

- es6中新增了一种原始数据类型Symbol，表示独一无二的值，最大的用法是用来定义对象的唯一属性名。

