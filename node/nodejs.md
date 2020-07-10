# 额外知识

## IP地址和端口号

- ip地址用来定位计算机
- 端口号用来定位具体的应用程序
- 所有需要联网通信的应用程序都会占用一个端口号
- 端口号的范围从0-65536之间
- 在计算机中有一些默认端口号，最好不要去使用，例如http服务的80
- 我们在开发过程中使用一些简单好记的就可以了，例如3000，5000等
- 可以开启多个服务，但一定要确保不同服务占用的端口号不一样

## window下常用命令

- dir：列出当前目录下的所有文件；
- cd+目录名：切换目录；
- md+文件夹名：当前目录创建文件夹；
- rd+文件名：删除文件夹；
- 想进哪个盘直接盘符加冒号就行，例如e：即可进入e盘。
- cls，将cmd清屏

## 环境变量

- 当我们在命令行打开一个文件或者调用一个程序时，系统会首先在当前目录下寻找文件程序，如果找到了则直接打开，如果没找到则会依次到环境变量的path的路径中寻找，直到找到为止，如果没找到就报错。所以我们可以将一些经常需要访问的程序和文件的路径添加到path中，这样我们就可以在任意位置来访问这些文件和程序了。

- 关于配置环境变量，系统变量是无论用户是谁都可以用的，而用户变量是针对当前用户的，所以我们一般更改用户变量也是为了避免出现系统错误。改完环境变量需要重启命令行窗口才会生效。

## 进程和线程

- 进程：
  - 进程负责为程序的运行提供必备的环境，代码要放到进程中才能跑。
  - 进程就相当于工厂中的车间。

- 线程：
  - 线城是即时计算中的最小的计算单位，线程负责执行程序中的程序。线程就相当于工厂中的工人。
  - 单线程与多线程：即一个人干活和多个人干活，没有绝对的好坏，一般情况下是多线程好，不过多线程也会存在并发的问题，Js，浏览器这种都是单线程，不过像java这种就是多线程。

## 判断是否是服务端渲染

- 检查网页源代码，如果数据本身就在网页里那么就是服务端渲染，如果不在那么就是客户端渲染。
- 一般的网站都是服务端渲染与客户端渲染结合着使用，因为客户端渲染不利于SEO搜索引擎的优化。服务端渲染是可以被爬虫抓取到的，而客户端异步渲染是很难被爬虫抓取到的。
- 例如京东的商品列表就是服务端渲染，为了SEO优化。但是商品评论列表为了用户体验采用了异步渲染。

# node特点

- event-driven 事件驱动

- 非阻塞IO模型（异步）

- 轻量和高效

- node中没有dom和bom

- 在node中没有全局作用域，只有模块作用域，外部访问不到内部，内部也访问不到外部。

# node执行js文件

- 通过命令行找到相应文件，之后通过node 文件名即可执行，例`node test.js`

- 注：js文件名不能以 node.js 来命名
- 如果想一次性执行多个文件，需要按照想要的执行顺序在相应的js文件中通过require引入别的js文件并执行。

# node模块化

## 模块类型

- 具名的核心模块，例如fs，http
- 用户自己编写的文件模块，相对路径必须加`./`，可以省略js后缀名。

## 官方模块接口

- node为js提供了很多服务器级别的API，这些API绝大多数都被包装到了一个具名的核心模块中了。例如
  - 文件操作的`fs`模块
  - 服务器相关的`http`模块
  - 路径相关的操作`path`模块。
- 想要用模块，必须要通过require('模块名')引入模块对象。

## 不同模块之间的通信

- require方法有两个作用
  - 加载文件模块并执行里面的代码
  - 拿到被加载文件模块导出的接口对象`exports`
- 因此可以通过在被require的文件中配置exports对象，之后引入的文件便可以调用

```javascript
//b.js
var b = 'foo'
exports.foo	= 'hello'
exports.add = function(x,y) {
return x+y
}
//a.js
var bexports = require("./b")
console.log(bexports.foo)	//hello
console.log(bexports.add(2,43))	//45
```

# node文件处理

## 读取文件

- 这里用toString是因为默认读取到的文件格式即data是二进制数据，因此我们不认识，所以需要用toString方法转换一下。
- toString方法仅当我们需要对字符串进行相关的处理时使用。

```javascript
var fs = require("fs")		//fs文件相关操作的核心模块，译为file-system
//第一个参数是路径，第二个参数是回调函数，有error和data两个参数
//成功：data是数据，error是null；失败：data是undefined，error是错误对象。
fs.readFile('./hello.txt',function(error,data) {
    if(error) {
        console.log('读取文件失败了')
    }
    else{
       console.log(data.toString()) 
    }
})
```

## 写文件和简单的错误处理

```javascript
var fs = require('fs')
//第一个是路径，第二个是内容，第三个参数回调函数，只有一个参数即error错误对象。
//成功：文件写入成功，error是null；失败：文件写入失败，eroor就是错误对象
fs.writeFile('./data/你好.md','大家好我是robot',function(error) {
    if(error) {
        console.log('写入失败了')	//当我们文件名非法时就会写入失败
	}
    else{
        console.log('文件写入成功')
    }
})	
```

# node构建web服务器

- 服务器负责干嘛
  - 提供服务：对数据的服务
  - 发请求
  - 接收请求
  - 处理请求
  - 给个反馈（发送响应）
- cmd执行该文件成功之后cmd会不能被进行操作，因为服务器已经启动成功了，cmd仅仅可以用来显示服务器相关的信息。要想关闭掉服务器，可以通过   ctrl + c
- 如果想根据不同的请求路径返回不同的结果，只要利用if，else判断request.url的路径就可以了。
- `end`方法接收的数据格式共有两种，一种是字符串，一种是二进制数据。

```javascript
var http = require('http')	//http模块用来处理网络相关的操作
var server = http.createServer()	//创建服务器
//接收到request请求后执行回调函数，回调函数可以传两个参数即request，response
//request：请求对象，可以获取客户端的一些信息。
//response：响应对象，可以用来给客户端发送响应数据
server.on('request',function(request,response) {
    //	这里url指的是端口号后面的路径，默认是/
	console.log('收到客户端的请求了，请求路劲是：' + request.url)	
    //write可以写入要返回的数据，可以多次调用
    response.write('hello')		
    response.write('nodejs')
    //执行完响应语句之后一定要执行end方法来告诉服务器话说完了，即响应完了
    response.end()	//我们一般用简写，response.end('hello nodejs')。直接end的同时发送响应
})
//绑定端口号，启动服务器，这里之所以用回调函数是因为服务器启动需要时间
server.listen(3000,function() {
    console.log('服务器启动成功了，可以通过localhost:3000来进行访问')
})
```

## http模块接口补充

```javascript
server.on('request',function(request,response) {
	let url = request.url
	console.log('请求我的客户端的ip地址与端口号是：',
                //请求的ip地址
                request.socket.remoteAddress,
                //请求的端口号
                request.socket.remotePort)
})
```

## 响应返回json类数据

- 因为默认响应内容只能是二进制数据或者字符串，所以我们需要先手动将json数据转成字符串

```
response.end(JSON.stringify(products))
```

## 响应内容格式编码问题

- 在服务端默认发送的数据，其实是utf8编码的内容，但是浏览器不知道你是utf8编码的内容。
- 浏览器在不知道服务器响应内容的编码的情况下，会按照当前操作系统的的默认编码去解析。
- 我们用的中文的操作系统的默认编码是gbk。
- 解决方法就是告诉浏览器我们用的编码是什么类型

```javascript
//固定格式，不要更改。
//Content-Type表示告知对方我给你发送的信息是什么类型
//text/plain 就是普通文本
//如果你发送的是html格式的字符串，则也要告诉浏览器我给发送的是text/html格式的内容
resquest.setHeader('Content-Type','text/plain;charset=utf-8')
```

## 响应不同类型的文件

- 我们一般是根据不同的路径，去读取不同的文件，然后将读取到的内容响应回去。

- 不同的文件格式要对应不同的Content-Type
- 图片不需要设置编码。一般只需要为字符设置编码。给图片设置编码反而可能会出问题。

- `tool.oschina.net`，该网址可以查看不同文件格式的content-type。

## 模仿apache访问

- 就是利用将访问路径的url直接与根目录的路径拼接起来当作读取文件的路径，从而实现根据路径读取相应的文件。

```javascript
//模仿apache根据确定的路径返回确定的文件
var fs = require('fs')
var http = require('http')
var server = http.createServer()
let rootDirectory = 'E:/www'
server.on('request',fucntion(req,res) {
	let path = '/index.html'
	if(url !=='/') {
		path = req.url
	}
	fs.readFile(rootDirectory+path,fucntion(err,data) {
	if(err) {console.log('404 not Found')}
	else {
			res.end(data)
		}
	})
})
server.listen(3000,fucntion() {
    console.log('服务启动成功')
})
```

- 模仿apache当没有index文件时，显示文件列表。推荐用模板引擎方法。

```javascript
//老师的服务端渲染比较粗暴，主要思路为在网页文件中用一个特殊符号做标记，之后进行字符传替换，将tbody中的内容替换该特殊符号。
//tbody中的内容是通过数组遍历进行字符串累加实现，以下介绍有关方法。
var fs = require('fs')
fs.readdir('D:/www',function(err,files) {	//读取目录列表
    if(err) {return res.end('can not find www.dir')}	
    console.log(filse)	//files为目录列表，格式为，['a.txt','login.html','img']这种
})
```

# 在node中使用模板引擎

- 其实可以说就是服务端渲染，即利用模板引擎在后端将数据替换之后返回网页源代码的字符串然后浏览器进行解析。

- ```html
  //模板引擎介绍
  //xx.html
  //模板引擎不关心模板的内容是什么，只关心{{}}语法中的内容，别的内容都被识别为字符串
  <script src="node_modules/art-template/lib/template-web.js"></script>
  <script type="text/template" id="tpl">
  大家好，我叫: {{name}}
  我今年 {{age}}	岁了
  
  //each与/each中间的内容是循环体，会根据hobbies数组长度遍历相应的次数，如果有其他内容也会一
  //起遍历，并会每次替换相应的value值，即hobbies中的元素
  
  我的爱好是： {{each hobbies}} 	
  				{{$value}} 
  			{{/each}}
  </script>
  <script>
      var ret = template('tpl', {
          name: 'Jack',
          age: 18,
          hobbies: ['敲代码','游戏','动漫']
      })
      console.log(ret)	
  //ret 的结果是	大家好，我叫：Jack
  //			   我今年18岁了
  //			   我的爱好是：敲代码 游戏 动漫
  </script>
  ```

- 安装，`npm install art-template`

- 在需要使用的文件模板中加载 art-template

  - 在node中可以通过require引入，`var template = require('art-template')`

- 查文档，使用模板引擎的API

```javascript
//	xx.html
在该文件中定义html模板
// 	xx.js
var fs = require('fs')
var template = require('art-template')
fs.readFile('./xx.html',function(err,data) {
    if(err) {return console.log('读取文件失败')}
    let temStr = data.toString()
    var ret = template.render(temStr,{
        name: 'Jack',
        age: 18,
        hobbies: ['写代码','唱歌','打游戏']
    })
    res.end(ret)	//将用模板引擎替换过后的字符串响应给浏览器并进行解析
})
```

# 留言板实战

## 处理网站中的静态资源

- 因为读取的html文件中可能还会因为link，script标签等再引入别的文件，在服务端也视为请求，所以主要是对他们做一定的处理。

- 为了让目录结构保持统一清晰，我们约定把所有的html文件都放在views（视图）中
- 为了统一处理静态资源，我们约定把所有的静态资源都存放在public文件夹内。
- 注意，这样之后服务端中的文件路径就不要再用相对路径了，因为我们开放了`/public/`路径，因此要改成`/public/`开头这种形式。

```javascript

```

