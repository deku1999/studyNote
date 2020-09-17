## bind、call、apply、箭头函数、new的区别

call和apply其实是一样的，区别就在于传参时参数是一个一个传或者是以一个数组的方式来传。<br>
call和apply都是在调用时生效，改变调用者的this指向。<br>

```
let name = 'Jack'
const obj = {name: 'Tom'}
function sayHi() {console.log('Hi! ' + this.name)}

sayHi() // Hi! Jack
sayHi.call(obj) // Hi! Tom

```

bind也是改变this指向，不过不是在调用时生效，而是返回一个新函数。

```
const newFunc = sayHi.bind(obj)
newFunc() // Hi! Tom
```





## 请简述`JavaScript`中的`this`。

JS 中的`this`是一个相对复杂的概念，不是简单几句能解释清楚的。粗略地讲，函数的调用方式决定了`this`的值。我阅读了网上很多关于`this`的文章，[Arnav Aggrawal](https://medium.com/@arnav_aggarwal) 写的比较清楚。`this`取值符合以下规则：

1. 在调用函数时使用`new`关键字，函数内的`this`是一个全新的对象。
1. 如果`apply`、`call`或`bind`方法用于调用、创建一个函数，函数内的 this 就是作为参数传入这些方法的对象。
1. 当函数作为对象里的方法被调用时，函数内的`this`是调用该函数的对象。比如当`obj.method()`被调用时，函数内的 this 将绑定到`obj`对象。
1. 如果调用函数不符合上述规则，那么`this`的值指向全局对象（global object）。浏览器环境下`this`的值指向`window`对象，但是在严格模式下(`'use strict'`)，`this`的值为`undefined`。
1. 如果符合上述多个规则，则较高的规则（1 号最高，4 号最低）将决定`this`的值。
1. 如果该函数是 ES2015 中的箭头函数，将忽略上面的所有规则，`this`被设置为它被创建时的上下文。

想获得更深入的解释，请查看[他在 Medium 上的文章](https://codeburst.io/the-simple-rules-to-this-in-javascript-35d97f31bde3)。

https://github.com/yangshun/front-end-interview-handbook/blob/master/Translations/Chinese/questions/javascript-questions.md#%E8%AF%B7%E7%AE%80%E8%BF%B0javascript%E4%B8%AD%E7%9A%84this

#### 

## 如何确定this指向

如果要判断一个运行中函数的 this 绑定，就需要找到这个函数的直接调用位置。找到之后就可以顺序应用下面这四条规则来判断 this 的绑定对象。

1. 由 new 调用？绑定到新创建的对象。
2. 由 call 或者 apply （或者 bind ）调用？绑定到指定的对象。
3. 由上下文对象调用？绑定到那个上下文对象。
4. 默认：在严格模式下绑定到 undefined ，否则绑定到全局对象。

一定要注意，有些调用可能在无意中使用默认绑定规则。如果想“更安全”地忽略 this 绑定，你可以使用一个 DMZ 对象，比如 ø = Object.create(null) ，以保护全局对象。<br>
ES6 中的箭头函数并不会使用四条标准的绑定规则，而是根据当前的词法作用域来决定this ，具体来说，箭头函数会继承外层函数调用的 this 绑定（无论 this 绑定到什么）。这其实和 ES6 之前代码中的 self = this 机制一样