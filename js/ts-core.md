## 环境搭建

1.安装nodejs

2.全局安装ts `npm install -g typescript`

3.创建一个`ts`后缀的文件，进入ts文件所在目录使用tcs进行编译，`tcs xx.ts`，这样就会生成一个编译后的js文件，默认编译后的js文件是es3

## TS的类型声明

### 常见类型限制

**声明的类型都是小写，例如number, string, boolean**

声明一个数字型变量，字符型变量同理

```typescript
let a: number
```

直接声明并赋值，不推荐，如果变量的声明和赋值同时进行，TS可以自动对变量进行类型检测。

```typescript
let c: boolean = false

let c = false
c= 2 //报错，因为ts自动检测认为其是布尔类型
```

函数声明，限制参数类型就在参数后面写类型，限制返回值就在括号后面写类型

```typescript
function sum (a: number, b: number) :number {
	return a + b
}
```

数组声明

```typescript
//为了使数组中存储的都是同意类型的值
let a: array	//不好

//下面两种好，两种写法
let e: string[]		
let e: Array<String>	
```

对象声明

```typescript
let a: object	//不实用，因为在js种一般的function，array也是对象

// {}用来指定对象中可以包含哪些属性
// 语法：{属性名：属性值， 属性名：属性值}
// 在属性后边加上？，表示属性是可选的
//[propName: string]: any表示任意类型的属性
let a: {name: string}	
let a: {name: string, age? : number}
//当只需要限制一个，其余的都是任意添加减少时
let c: {name: string, [propName: string]: any}
```

设置函数结构的类型声明

```typescript
//语法：(形参：类型，形参类型...) => 返回值
let d: (a: number, b: number) => number
d = function(a: number, b: number): number {
    return a + b
}
```

### 联合类型

通过`|`连接多个选择，多是用于集合似选择，例如

```typescript
let b: 'male' | 'female'	//b只能为这两个
let b: boolean | string		//b只能是布尔或者字符类型
```

### 其他类型

- 值类型

```typescript
let a: 10	//这样a就只能等于10

//当有多个值类型时，可以给类型起别名来更省事
let k: 1 | 2 | 3 | 4 | 5
//自定义类型
type myType = 1 | 2 | 3 | 4 | 5
let m: myType
```

- any，任意类型，可以赋值给任意类型，尽量别用

any表示的是任意类型，一个变量设置类型为any后相当于对该变量关闭了TS的类型检测

```typescript
let a: any		//a什么类型都可以
let a		//隐式any
```

- unknown，未知类型，实际上就是一个类型安全的any，一般不能赋值给其他变量

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

- void，空值，主要是用作函数返回值类型约束，不过当你返回`undefined`,`null`时不会报错

```typescript
function fn(): void {
}
```

- never，表示永远不会返回结果，`undefined`，`null`也不行

```typescript
//常见用法
function fn(): never {
    throw new Error('报错了')
}
```

- 元组，就是固定长度的数组

```typescript
let h: [string, string]
```

- 枚举，当我们在多个值之间进行选择的时候适合用这个

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
const {cleanWebpackPlugin} = require('clean-webpack-plugin')
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
        new cleanWebpackPlugin(),
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

**继承**

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

**抽象类**

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
```

## 属性的封装

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

