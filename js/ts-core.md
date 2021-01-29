## 环境搭建

1.安装nodejs

2.全局安装ts `npm install -g typescript`

3.创建一个`ts`后缀的文件，进入ts文件所在目录使用tcs进行编译，`tcs xx.ts`，这样就会生成一个编译后的js文件，默认编译后的js文件是es3

## TS的类型声明

### 常见类型限制

#### number/string/boolean

**声明的类型都是小写，例如number, string, boolean**

声明一个数字型变量，字符型变量同理

```typescript
let a: number
```

直接声明并赋值，不推荐

```typescript
let c: boolean = false
```

#### 函数声明

##### **基本示例**

限制参数类型就在参数后面写类型，限制返回值就在括号后面写类型

```typescript
function sum (a: number, b: number) :number {
	return a + b
}
```

##### **函数类型**

```javascript
//语法：(形参：类型，形参类型...) => 返回值
let d: (a: number, b: number) => number
d = function(a: number, b: number): number {
    return a + b
}
```

##### **可选参数和默认参数**

```typescript
function buildName(firstName: string='A', lastName?: string): string {
  if (lastName) {
    return firstName + '-' + lastName
  } else {
    return firstName
  }
}

console.log(buildName('C', 'D'))
console.log(buildName('C'))
console.log(buildName())
```

##### **剩余参数**

```typescript
function info(x: string, ...args: string[]) {
  console.log(x, args)
}
info('abc', 'c', 'b', 'a')
```

##### 函数重载

```typescript
/* 
函数重载: 函数名相同, 而形参不同的多个函数
需求: 我们有一个add函数，它可以接收2个string类型的参数进行拼接，也可以接收2个number类型的参数进行相加 
*/

// 重载函数声明
function add (x: string, y: string): string
function add (x: number, y: number): number

// 定义函数实现
function add(x: string | number, y: string | number): string | number {
  // 在实现上我们要注意严格判断两个参数的类型是否相等，而不能简单的写一个 x + y
  if (typeof x === 'string' && typeof y === 'string') {
    return x + y
  } else if (typeof x === 'number' && typeof y === 'number') {
    return x + y
  }
}

console.log(add(1, 2))
console.log(add('a', 'b'))
// console.log(add(1, 'a')) // error
```

#### 数组声明

```typescript
//为了使数组中存储的都是同意类型的值
let a: array	//不好

//下面两种好，两种写法
let e: string[]		
let e: Array<String>	
```

#### 对象声明

```typescript
let a: object	//不实用，因为在js种一般的function，array也是对象

// {}用来指定对象中可以包含哪些属性
// 语法：{属性名：属性值， 属性名：属性值}
let a: {name: string}	

// 在属性后边加上？，表示属性是可选的
let a: {name: string, age? : number}

//[propName: string]: any表示任意类型的属性
//当只需要限制一个，其余的都是任意添加减少时
let c: {name: string, [propName: string]: any}
```

### 联合类型

通过`|`连接多个选择，多是用于集合似选择，例如

```typescript
let b: 'male' | 'female'	//b只能为这两个
let b: boolean | string		//b只能是布尔或者字符类型
```

### 其他类型

#### 类型推断

类型推断: TS会在没有明确的指定类型的时候推测出一个类型。有下面2种情况: 

1. 定义变量时赋值了, 推断为对应的类型. 
2. 定义变量时没有赋值, 推断为any类型

```javascript
/* 定义变量时赋值了, 推断为对应的类型 */
let b9 = 123 // number
// b9 = 'abc' // error

/* 定义变量时没有赋值, 推断为any类型 */
let b10  // any类型
b10 = 123
b10 = 'abc'
```

#### 类型断言

即我们手动告诉解析器变量的实际类型来消除报错，两种写法

```typescript
let e: unknown
s = e as string
s = <string>e
```

#### 值类型

```typescript
let a: 10	//这样a就只能等于10

//当有多个值类型时，可以给类型起别名来更省事
let k: 1 | 2 | 3 | 4 | 5
```

#### 自定义类型

```javascript
//自定义类型
type myType = 1 | 2 | 3 | 4 | 5
let m: myType
```

#### any类型

任意类型，可以赋值给任意类型，尽量别用。

any表示的是任意类型，一个变量设置类型为any后相当于对该变量关闭了TS的类型检测

```typescript
let a: any		//a什么类型都可以
let a		//隐式any
```

#### unknown类型

未知类型，实际上就是一个类型安全的any，一般不能赋值给其他变量

```typescript
let e: unknown
//一种赋值方法是提前检测，这样可以赋
if(typeof e === 'string'){
    s = e 
}
//一种是类型断言，即我们手动告诉解析器变量的实际类型来消除报错，两种写法
s = e as string
s = <string>e
```

#### void类型

空值，主要是用作函数返回值类型约束，不过当你返回`undefined`,`null`时不会报错

```typescript
function fn(): void {
}
```

#### never类型

表示永远不会返回结果，`undefined`，`null`也不行

```typescript
//常见用法
function fn(): never {
    throw new Error('报错了')
}
```

#### 元组

就是固定长度的数组

```typescript
//元组类型在使用的时候，数据的类型的位置和数据的个数，应该和在定义元组的时候的数据是一致的。
let h: [string, string]
```

#### 枚举

`enum` 类型是对 JavaScript 标准数据类型的一个补充。 使用枚举类型可以`为一组数值赋予友好的名字`。当我们在多个值之间进行选择的时候适合用这个

```javascript
enum Color {
  Red,
  Green,
  Blue
}

// 枚举数值默认从0开始依次递增
// 根据特定的名称得到对应的枚举数值
let myColor: Color = Color.Green  // 1
console.log(myColor, Color.Red, Color.Blue)

//默认情况下，从 0 开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 1 开始编号：
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green	//c = 2

//或者，全部都采用手动赋值：
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green
```

```typescript
enum Gender {
	Male = 0,
	Female = 1
}
let i = {name: string, gender: Gender}
i = {
    name: '孙悟空',
    gender: Gender.Male
}
console.log(i.gender === Gender.Male)
```

枚举类型提供的一个便利是你可以由枚举的值得到它的名字。 例如，我们知道数值为 2，但是不确定它映射到 Color 里的哪个名字，我们可以查找相应的名字：

```javascript
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2]

console.log(colorName)  // 'Green'
```

#### undefined和null

所有类型的子类型，即你可以将undefined和null赋值给其它类型的变量。不过注意要关闭严格模式。

```typescript
let num2: number = undefined
```

## 编译选项

### 自动编译

**单个文件自动编译**

当我们不想每次一改变文件就手动编译，可以通过`tsc xx.ts -w`来开启该文件的自动编译。

**文件夹下所有文件自动编译**

1.需要先在文件夹下新建`tsconfig.json`文件

2.`tsc`编译文件夹下所有文件，`tsc -w`自动编译文件夹下所有文件

### tsconfig.json配置

1.inclued，用来指定哪些文件目录需要被编译

2.exclude，排除哪些文件目录不被编译，有默认值为 ["node_modules", "bower_components", "jspm_packages"]

3.不常用：extends，继承别的json文件中的配置；files，单个文件。

**4.compilerOptions，跟ts的编译有关**

```json
//**表示任意目录，*表示任意文件，如果有多个目录可以继续添加，因为值是数组格式
{
	"include": [
        "./src/**/*"
    ],
    "exclude": [
        "./src//hello/**/*"
    ],
    "compilerOptions": {
        //target， 用来指定编译后的js版本
        "target": "ES3",
        //module， 指定要使用的模块化的规范，例如ES6,commonjs
        "module": "ES2015",
        //lib， 用来指定项目中要使用的库，例如"dom"库，一般不需要动它
        "lib": ["dom"],
     	//outDir，用来指定编译后文件所在的目录
        "outDir": "./dist",
        // outFile, 用来将代码合并为一个文件，设置后，所有的全局作用域中的代码会合并到同一个文件中
        "outFile": './dist/app.js',
        // allowJs，是否对js文件进行编译，默认是false
        "allowJs": false,
       	//checkJs, 是否检查js代码是否符合语法规范，默认也是false
      	"checkJs": false,
        //removeComments，是否移除注，默认是false
        "removeComments": false,
        //noEmit, 不生成编译后的文件，默认false
        "noEmit": false,
        //noEmitOnError, 当有错误时不生成编译后的文件，默认false
        "noEmitOnError": false,
        //alwaysStrict，用来设置编译后的文件是否使用严格模式，默认false
        "alwaysStrict": false,
        //noImplicitAny，不允许隐式的any类型，默认false
        "noImplicitAny": false,
        //noImplicitThis,不允许不明确类型的this，默认false
        "noImplicitThis": false,
        //strictNullChecks，严格的检查空值，默认false
        "strictNullChecks": false,
        //所有严格检查的总开关，默认false，当为true时上面的四个都默认为true
        "strict": false 
    }
}
```

## webpack打包ts代码

### 基础配置

1.`npm init -y`生成初始化的package.json文件

2.安装生产时依赖，`npm install webpack webpack-cli ts-loader typescript --save-dev`

3.创建webpack.config.js文件并配置

```javascript
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
//webpack中所有的配置信息都应该写在module.exports中
module.exports = {
    //指定入口文件
    entry: './src/index.ts',
    //打包文件的配置
    output: {
        //指定打包文件的目录
        path: path.resolve(__dirname, 'dist'),
        //打包后的文件
        filename: 'bundle.js'
    },
    //指定webpack打包时要使用模块
    module: {
        //指定要加载的规则
        rules: [
            {
                // test指定的是规则生效的文件，下面是所有以ts为后缀的文件
                test: /\.ts$/,
                // 要使用的loader
                use: 'ts-loader',
                // 要排除的文件
                exclude: /node_modules/
            }
        ]

    },
    //配置webpack插件
    plugins: [
        new CleanWebpackPlugin(),
        //括号里可以传入配置，可以不传就使用默认配置
        new HtmlWebpackPlugin({
            title: '这是一个自定义的title',
            //模板
            template: './src/index.html'
        })
    ],
    //用来设置引用模块，告诉webpack什么文件可以作为模块
    resolve: {
        extensions: ['.ts', '.js']
    }
}
```

4.创建并配置tsconfig.json文件

```json
{
    "compilerOptions": {
        "module": "ES2015",
        "target": "ES2015",
        "strict": true
    }
}
```

5.将package.json文件的scripts属性里加一条快捷命令build，对应webpack

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  }
```

### 常用配置

**注：以下配置都是在第一步配置的基础上进行配置**

- html-webpack-plugin

可以在输出目录下自动生成html文件并引入打包后的js文件

1.安装，`npm install html-webpack-plugin --save-dev`

2.webpack.config.js文件中配置，见上图

- webpack-dev-server

打包完后自动打开生成的html文件，并保持热更新，即更新文件就自动重新打包并刷新页面

1.安装，`npm install webpack-dev-server --save-dev`

2.配置package.json，添加快捷命令

```json
  "scripts": {
    "start": "webpack serve --open"
  }
```

- clean-webpack-plugin

打包时清空dist文件夹下的缓存再重新生成

1.安装，`npm install clean-webpack-plugin --save-dev`

2.webpack.config.js中配置，见上图

- 一般的ts文件如果不配置模块选项，当出现模块引用时打包会出错，通过配置webpack.config.js中的resolve选项即可。详细配置见上图

### 配置babel

为了将打包后的代码兼容不同的浏览器，需要安装babel

1.安装，`npm install @babel/core @babel/preset-env babel-loader core-js --save-dev`

2.因为要作用的还是ts后缀的文件，因此不用增加新的规则，配置webpack.config.js中的rules规则

```javascript
module.exports = {
    //指定webpack打包时要使用模块
    module: {
        //指定要加载的规则
        rules: [
            {
                // test指定的是规则生效的文件，下面是所有以ts为后缀的文件
                test: /\.ts$/,
                // 要使用的loader，谁写在后面先调用谁
                use: [
                    //配置babel
                    {
                        //指定加载器
                        loader: 'babel-loader',
                        // 设置babel
                        options: {
                            //设置预定义环境
                            presets: [
                                [
                                    //指定环境的插件
                                    '@babel/preset-env',
                                    //配置信息
                                    {
                                        //要兼容的浏览器版本
                                        targets: {
                                            "chrome": '85'
                                        },
                                      //下的是多少版本就写几，指定corejs版本
                                        "corejs": '3',
                                      // 使用corejs的方式,usage表示按需加载
                                        'useBuiltIns': 'usage'
                                    }
                                ]
                            ]
                        }
                    },
                    'ts-loader',
                ],
                // 要排除的文件
                exclude: /node_modules/
            }
        ]
    }
}
```

3.因为webpack默认会将打包后的bundle.js文件设置为立即执行的箭头函数，在ie浏览器中是不行的，因此需要更改设置

```javascript
module.exports = {
    output: {
            //指定打包文件的目录
            path: path.resolve(__dirname, 'dist'),
            //打包后的文件
            filename: 'bundle.js',
            //告诉webpack不使用箭头
            environment: {
                arrowFunction: false
            }
        }
}
```

## 类

### 基本使用

**基本介绍**

```javascript
class Person {
	//定义实例属性
	name: string = '孙悟空',
	age: number = 18

	//在属性前使用static关键字可以定义类属性（静态属性），只能通过对象去访问Person.height
	static height: number = 175

	//readonly定义只读属性，不能改
	readonly First:string = '只读'

	//定义方法，如果以static开头就是类方法同静态属性同理
	say() {
        console.log('hello world')
    }
}
```

**正式使用**

```typescript
class Person {
	name: string
	age: number
	constructor(name: string, age: number) {
		this.name = name
		this.age = age
	}
}
```

### **继承**

使用继承后，子类将会拥有父类所有的方法和属性，通过继承可以将多个类中共有的代码写在一个父类中，这样只需要写一次即可让所有的子类同时拥有父类中的属性和方法。

```typescript
class Son extends Father {
}
```

**super关键字**

super就代表的是当前类的父类，多用于子类中添加新的属性

```typescript
//如果在子类中写了构造函数，在子类构造函数中必须对父类的构造函数进行调用
class Animal {
	name: string
    constructor(name: string){
		this.name = name
    }
}
class Dog extends Animal {
    age: number
    constructor(name: string, age: number) {
        super(name)	//调用父类的构造函数
        this.age = age
    }
}
```

### **抽象类**

禁止一个类被用来创建对象，例如被继承的超类一般我们不希望别人用它来创建对象，因此会用到抽象类，可以说它就是专门用来继承的类

```typescript
abstract class Animal {
	name: string
	constructor(name: string) {
		this.name = name
	}
}
```

抽象类中可以添加抽象方法

```javascript
//以abstract开头，没有方法体，抽象方法只能定义在抽象类中，子类必须对抽象方法进行重写
abstract sayHello(): void
```

## 接口

接口用来定义一个类中应该包含哪些属性和方法，同时接口也可以当成类型声明去使用

```typescript
interface myInterface {
	name: string,
	age: number
}
```

接口可以重复声明，最终的类型就是多个接口合在一起

```typescript
interface myInterface {
	name: string,
	age: number
}
interface myInterface {
	gender: string
}
```

### 类类型实现接口

接口可以在定义类的时候去限制类的结构，接口中的所有的属性都不能有实际的值，接口只定义对象的结构，而不考虑实际值，接口中所有的方法都是抽象方法

```typescript
interface myInter {
	name: string,
	sayHello(): void
}
 // 定义MyClass类实现接口
class MyClass implements myInter {
    name: string,
    constructor(name: string) {
        this.name = name
    }
    sayHello() {
        console.log('大家好')
    }
}
//类也可以实现多个接口
class Car2 implements Alarm, Light {
  lightOn() {
    console.log('Car light on');
  }
  lightOff() {
    console.log('Car light off');
  }
}
```

### 函数类型实现接口

为了使用接口表示函数类型，我们需要给接口定义一个调用签名。它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型

这样定义后，我们可以像使用其它接口一样使用这个函数类型的接口。 下例展示了如何创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量。

```typescript
/* 
接口可以描述函数类型(参数的类型与返回的类型)
*/

interface SearchFunc {
  (source: string, subString: string): boolean
}

const mySearch: SearchFunc = function (source: string, sub: string): boolean {
  return source.search(sub) > -1
}

console.log(mySearch('abcd', 'bc'))
```

### 接口当作类型声明

接口当作类型声明来使用

```javascript
interface Person {
	name: string
	age: number
}
function myPerson(person: Person) {
	return person.name + person.age
}
myPerson({name:'haha', age: 12})
```

### 接口继承接口

和类一样，接口也可以相互继承。 这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。

```typescript
interface LightableAlarm extends Alarm, Light {
}
```

## 类属性的封装

TS可以在属性前添加属性的修饰符

public：修饰的属性可以在任意位置访问（修改），默认值

private：私有属性，私有属性只能在当前类内部进行访问（修改），可以通过在类中添加方法使得私有属性可以被外部访问

protected：受保护的属性，只能在当前类和当前类的子类中访问（修改）

```typescript
class Person {
	private name: string
	private age: number
	constructor(name: string, age: number) {
		this.name = name
		this.age = age
	}
	setter(name: string) {
		this.name = name
	}
	getter() {
		return this.name
	}
    //TS中设置getter方法的方式，这样直接通过实例的per.name即可获取，不过本质还是调用该方法
    get name() {
        return this.name
    }
    //TS中设置setter的方式，同上述get同理，直接per.name = '新名字'即可 
    set name(value: string) {
        this.name = value
	}
}
```

可以直接将属性定义在构造函数中并加上修饰符

```typescript
//语法糖写法
class C {
	constructor(public name: string, public age: number, private height: number) {
	}
}
//相当于
class C {
    name: string
    age: number
    private height: number
    constructor(name: string, age: number, height: number) {
		this.name = name
        this.number = number
        this.height = height
    }
}

//使用，直接new并赋值就行
let c = new C('hehe', 3, 123)
```

## 泛型

### 函数泛型

在定义函数或是类时，如果遇到类型不明确就可以使用泛型。

```typescript
//定义泛型，直接<T>定义泛型，T是类型名，随便起。
//下面函数代表定义一个函数与泛型T，参数与返回值类型都是T，这与any的区别就是没有关闭ts的类型检测。传入什么类型T就会变成什么样的类型。
function fn<T>(a: T): T {
	return a
}
//泛型也可以指定多个
function fn<T, K>(a:T, b: K): T {
    return a
}

//调用
fn(10)	//方式1，不指定泛型，TS可以自动对类型进行判断，不过可能有时会判断不出来
fn<string>('hello')		//方式2，直接指定就是字符类型
```

### 泛型接口

在定义接口时, 为接口中的属性或方法定义泛型类型。在使用接口时, 再指定具体的泛型类型。

```typescript
interface IbaseCRUD <T> {
  data: T[]
  add: (t: T) => void
  getById: (id: number) => T
}

class User {
  id?: number; //id主键自增
  name: string; //姓名
  age: number; //年龄

  constructor (name, age) {
    this.name = name
    this.age = age
  }
}

class UserCRUD implements IbaseCRUD <User> {
  data: User[] = []
  
  add(user: User): void {
    user = {...user, id: Date.now()}
    this.data.push(user)
    console.log('保存user', user.id)
  }

  getById(id: number): User {
    return this.data.find(item => item.id===id)
  }
}


const userCRUD = new UserCRUD()
userCRUD.add(new User('tom', 12))
userCRUD.add(new User('tom2', 13))
console.log(userCRUD.data)
```

### 泛型类

```typescript
class MyClass<T> {
	name: T,
	constructor(name: T) {
		this.name = name
	}
}
const mc = new MyClass<string>('孙悟空')
```

### 泛型约束

如果我们直接对一个泛型参数取 `length` 属性, 会报错, 因为这个泛型根本就不知道它有这个属性

```typescript
// 没有泛型约束
function fn <T>(x: T): void {
  // console.log(x.length)  // error
}
```

我们可以使用泛型约束来实现

```typescript
interface Lengthwise {
  length: number;
}

// 指定泛型约束
function fn2 <T extends Lengthwise>(x: T): void {
  console.log(x.length)
}
```

我们需要传入符合约束类型的值，必须包含必须 `length` 属性：

```typescript
fn2('abc')
// fn2(123) // error  number没有length属性
```

## 其他

### 声明文件

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

什么是声明语句？假如我们想使用第三方库 jQuery，一种常见的方式是在 html 中通过 `<script>` 标签引入 `jQuery`，然后就可以使用全局变量 `$` 或 `jQuery` 了。

但是在 ts 中，编译器并不知道 $ 或 jQuery 是什么东西

```typescript
/* 
当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。
声明语句: 如果需要ts对新的语法进行检查, 需要要加载了对应的类型说明代码
  declare var jQuery: (selector: string) => any;
声明文件: 把声明语句放到一个单独的文件（jQuery.d.ts）中, ts会自动解析到项目中所有声明文件
下载声明文件: npm install @types/jquery --save-dev
*/

jQuery('#foo');
// ERROR: Cannot find name 'jQuery'.
```

这时，我们需要使用 declare var 来定义它的类型。一般声明文件都会单独写成一个 `xxx.d.ts` 文件

```typescript
declare var jQuery: (selector: string) => any;
```

很多的第三方库都定义了对应的声明文件库, 库文件名一般为 `@types/xxx`, 可以在 `https://www.npmjs.com/package/package` 进行搜索

有的第三方库在下载时就会自动下载对应的声明文件库(比如: webpack),有的可能需要单独下载(比如jQuery/react)

### 内置对象

JavaScript 中有很多内置对象，它们可以直接在 TypeScript 中当做定义好了的类型。

内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。

1. ECMAScript 的内置对象

> Boolean
> Number
> String
> Date
> RegExp
> Error

2. BOM 和 DOM 的内置对象

> Window
> Document
> HTMLElement
> DocumentFragment
> Event
> NodeList

## 贪吃蛇项目练习

### 环境搭建

0.安装之前webpack打包ts的配置

1.安装css，less的插件

```
npm install less less-loader css-loader style-loader -D
```

配置webpack.config.js

```javascript
module.exports = {
	module: {
        rules: [
            //设置less文件的处理
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'less-loader'
                ]
			}
        ]
	}
}
```

src文件夹下index.ts文件引入less文件即可

```javascript
import './style/index.less'
```

2..安装postcss，为了兼容一些老版本浏览器的css代码

```javascript
npm install postcss postcss-loader postcss-preset-env -D
```

配置webpack.config.js

```typescript
        //指定要加载的规则
        rules: [           
            //设置less文件的处理
            {
                test: /\.less$/,
                use: [
                    'style-loader',
                    'css-loader',
                    //引入postcss
                    {
                        loader: 'postcss-loader',
                        options: {
                            postcssOptions: {
                                plugins: [
                                    [
                                        "postcss-preset-env",
                                        {
                                            //最新的两个浏览器版本
                                            browers: 'last 2 versions'
                                        }
                                    ]
                                ]
                            }
                        }
                    },
                    'less-loader'
                ]
            }
        ]
```

