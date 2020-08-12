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
- mkdir+文件夹名：当前目录创建文件夹；
- rmdir+文件名：删除文件夹；
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

## node测试环境

- 在cmd中直接输入node便可以进入node测试环境，类似于浏览器中的console控制台
- 想要退出，ctrl+c连续输入两次即可退出。

## 路径问题

- 不管是require中的路径，还是readFile这种路径，如果直接写`/`代表的就是当前文件所在磁盘的根目录。

- ```javascript
  //	index.js
  fs.readFile('./a.txt')
  这个readFile里面的路径是相对于执行node命令时所处的路径。
  ```

- 所以在文件操作中使用相对路径是不可靠的，需要转换为绝对路径。又因为绝对路径需要根据场景进行切换，所有我们选择用node的path模块join方法和`__dianame`结合使用组成绝对路径。
- 补充：模块中的路径跟文件是不同的，不受影响，模块中的路径标识就是相对于当前文件模块，不受执行node命令所处路径影响。

## node中的其他成员

- 在每个模块中，除了`require`、`exprots`等模块相关api之外，还有两个特殊的成员
  - `__dirname`，可以用来获取当前文件模块所属目录的绝对路径，不包括自身文件
  - `__filename`，可以用来获取当前文件的绝对路径，包括自身文件

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

- 具名的核心模块，例如fs，http，即node本身提供的系统级别的api，使用需要通过require('模块名')来进行引入

  - 文件操作的`fs`模块
  - 服务器相关的`http`模块
  - 路径相关的操作`path`模块。
  - url路径操作模块
  - os操作系统信息

- 第三方模块，例如art-template，加载同核心模块相同。

  - 例如，require('art-template')

  ```javascript
  //它的加载顺序是先判断是不是核心模块，在发现不是后再去找
  //node_modules/art-template 目录
  //再找 node_modules/art-template/package.json文件
  //之后找到package.json文件中的main属性，
  //main属性中就记录了 art-template 的入口模块
  //然后加载使用这个第三方包，实际上加载的还是文件。
  ----------------------
  如果没有package.json或者main没有指定入口模块或指定错误模块，则会默认加载该目录下的index.js文件
  ---------------------
  如果以上所有任何一个条件都不成立，则会进入上一级目录中的node_modules目录，规则同上继续查找。如果继续没有则继续上一级，直到当前磁盘的根目录下，如果还是没有找到那么就报错。
  ----------------------
  注意，我们一个项目有且只有一个node_modules，放在项目根目录中，这样其他的子目录都可以从该node_moduels中查找模块。
  ```

- 用户自己编写的文件模块，相对路径必须加`./`，可以省略js后缀名。即js文件。不加会被判定为第三方模块或者核心模块。

## 不同模块之间的通信

- 在node中，每个模块内部都有一个自己的module对象，在该module对象中，有一个成员叫：exports也是一个对象。每个模块末尾都会默认有一句代码，`return module.exports`。为了简化导出设置，每个模块内部还会有这样一句代码`var  exports = module.exports`
  - 也就是说如果你需要对外导出成员，只需要把导出成员挂载到module.exports中

### require方法加载规则

- require的模块查找机制
  - 优先从缓存加载
  - 核心模块
  - 路径形式的文件模块
  - 第三发模块，即node_modules中模块的查找规则

- require方法有两个作用

  - 加载文件模块并执行里面的代码

  - 拿到被加载文件模块导出的接口对象`module.exports`

- 优先从缓存加载

```javascript
//main.js
requrie('./a')
var fn = require('./b')
//a.js
console.log('a.js被加载')
require('./b')
//b.js
console.log('b.js被加载')
module.exports = function add(x,y) {return x+y}
//最终main.js加载的结果b.js被加载只会显示一次，因为在a中已经被加载过了。所以node会优先从缓存加载
//所以在main.js中引入b仅仅只是为了得到导出的对象
```

### 导出多个成员

- 即连续调用exports对象，为其属性赋值，然后引入的文件便可以通过该对象调用。

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

### 导出单个成员

- 通过module.exports，该语法一个文件只能调用一次，多次调用以最后一次为准

```javascript
//	直接导出一个函数
function add(x,y) {return x+y}
module.exports = add
//	该方法也能导出多个变量，只要将其放在一个对象中即可，我也更喜欢这种导出。
module.exports = {
    name,
    age,
    job: 'full stack engineer'
}
```

## npm

### 含义

- 第一个意思是`www.npmjs.com`，这是npm的官网，可以查看包注册用户等。
- 第二层的含义就是一个命令行工具，只要你安装了node就已经安装了npm。
- npm也有版本这个概念，`npm --verion`查看版本，`npm install --global npm`升级npm

### 常用命令

- npm init
  - npm init -y 可以跳过向导快速生成package.json文件
- npm install
  - 一次性把dependencies选项中的依赖项全部安装
  - npm i
- npm install  包名
  - 只下载
  - npm i 包名
- npm install --save 包名
  - 下载并且保存依赖项(package.json文件中的dependencies选项)
  - npm i -S 包名
- npm uninstall 包名
  - 只删除，如果有依赖项会依然保存
  - npm  un  包名
- npm uninstall  --save 包名
  - 删除的同时也会把依赖信息也去除
  - npm  un -S 包名

### 解决npm被墙问题

- npm存储包文件的服务器在国外，有时候会被墙，速度很慢，所以需要解决这个问题。
- `http://npm.taobao.org/`淘宝的开发团队将npm在国内做了一个备份。

安装淘宝的cnpm

```
npm install --global cnpm	//全局安装淘宝的npm镜像
```

接下来你安装包的时候把之前的`npm`替换成`cnpm`

举个例子：

```
//这里还是走国外的npm服务器，速度比较慢
npm install jquery
//使用cnpm就会通过淘宝的服务器来下载jquery
cnpm install jquery
```

- 如果不想安装`cnpm`又想使用淘宝的服务器来下载：`npm install jquery --registry=https://registry.npm.taobao.org`。但是每一次手动这样加参数比较麻烦，所以我们可以把这个选项添加到配置文件中

```
npm config set registry https://registory.npm.taobao.org
#查看npm 配置信息
npm config list
```

## package.json

- 我们建议每一个项目都要有一个`package.json`文件，（包描述文件，就像产品的说明书一样，给人一种很踏实的感觉）。该文件中会有文件入口，第三方包依赖信息等等说明。
- 这个文件可以通过`npm init`的方式初始化创建。
- 建议执行`npm install 包名`的时候，都加上`--save`，这样会在package.json中写入依赖信息。
- 如果你的`node_modules`删除了也不用担心，我们只需要重新执行`npm install`就会自动把`package.json`中的所有依赖项都下载回来。

## package.json和package-lock.json

- npm5之前是不会有`package-lock.json`这个文件的。npm5以后加入了文件，当你安装包的时候，都会生成或更新该文件。
- `package-lock.json`这个文件会保存`node_modules`中所有包的信息，这样重新`npm install`的时候的速度就会快很多。
- `package-lock.json`这个文件还有个文件锁的功能，假如`package.json`文件中有个依赖包，如果重新`npm install`那么会自动安装该依赖的最新版，但是如果有这个文件就会锁定该文件里文件的版本，从而解决有时因版本冲突带来的问题。

# url模块

- `url.parse(httpUrl)`，将我们传递进去的url拆分为一个对象的形式，里面有包括域名，路径等属性。
- `url.resolve(headUrl,footUrl)`，合并url

```javascript
var url = require('url')
var httpUrl = "https://www.bilibili.com/video/BV1i7411G7kW?p=9"
//解析url
console.log(url.parse(httpUrl))
Url {		//结果
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'www.bilibili.com',
  port: null,
  hostname: 'www.bilibili.com',
  hash: null,
  search: '?p=9',
  query: 'p=9',
  pathname: '/video/BV1i7411G7kW',
  path: '/video/BV1i7411G7kW?p=9',
  href: 'https://www.bilibili.com/video/BV1i7411G7kW?p=9'
}
//合成url
var headUrl = "http://www.taobao.com/"
var footUrl = "./sxt/qianduan/test.html"
console.log(url.resolve(headUrl,footUrl))
http://www.taobao.com/sxt/qianduan/test.html	//结果
```

# node文件处理模块

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

- 如果文件不存在，则会创建出来并进行写入

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

## Stream(流)

Stream 是一个抽象接口，Node 中有很多对象实现了这个接口。例如，对http 服务器发起请求的request 对象就是一个 Stream，还有stdout（标准输出）。

Node.js，Stream 有四种流类型：

- **Readable** - 可读操作。
- **Writable** - 可写操作。
- **Duplex** - 可读可写操作.
- **Transform** - 操作被写入数据，然后读出结果。

所有的 Stream 对象都是 EventEmitter 的实例。常用的事件有：

- **data** - 当有数据可读时触发。
- **end** - 没有更多的数据可读时触发。
- **error** - 在接收和写入过程中发生错误时触发。
- **finish** - 所有数据已被写入到底层系统时触发。

### 从流中读取数据

```javascript
var fs = require("fs");
var data = '';
// 创建可读流
var readerStream = fs.createReadStream('input.txt');
// 设置编码为 utf8。
readerStream.setEncoding('UTF8');
// 处理流事件 --> data, end, and error
readerStream.on('data', function(chunk) {
   data += chunk;
});
readerStream.on('end',function(){
   console.log(data);
});
readerStream.on('error', function(err){
   console.log(err.stack);
});
console.log("程序执行完毕地址：");

//执行结果
程序执行完毕地址：www.sxt.com
```

### 写入流

```javascript
var fs = require("fs");
var data = 'www.sxt.com';
// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt');
// 使用 utf8 编码写入数据
writerStream.write(data,'UTF8');
// 标记文件末尾
writerStream.end();
// 处理流事件 --> data, end, and error
writerStream.on('finish', function() {
    console.log("写入完成。");
});
writerStream.on('error', function(err){
   console.log(err.stack);
});
console.log("程序执行完毕");

//执行结果
程序执行完毕 写入完成。
output.txt内容为：www.sxt.com
```

### 管道流

为了方便我们有时需要将读取的流再通过写入流写入，node官方提供了管道流来封装这一过程。

```javascript
var fs = require("fs");
// 创建一个可读流
var readerStream = fs.createReadStream('input.txt');
// 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');
// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);
console.log("程序执行完毕");
```

### 链式流

链式是通过连接输出流到另外一个流并创建多个流操作链的机制。链式流一般用于管道操作。

接下来我们就是用管道和链式来压缩和解压文件。

```javascript
//压缩文件
var fs = require("fs");
var zlib = require('zlib');
// 压缩 input.txt 文件为 input.txt.gz
fs.createReadStream('input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('input.txt.gz'));
  
console.log("文件压缩完成。");

//执行完以上操作后，我们可以看到当前目录下生成了 input.txt 的压缩文件 input.txt.gz。
```

```javascript
//解压文件
var fs = require("fs");
var zlib = require('zlib');
// 解压 input.txt.gz 文件为 input.txt
fs.createReadStream('input.txt.gz')
  .pipe(zlib.createGunzip())
  .pipe(fs.createWriteStream('input.txt'));
  
console.log("文件解压完成。");

//执行完以上操作后，就可以看到解压后的input.txt文件
```

# path模块与文件处理

## path模块常用方法

- `path.basename，`获取路径中的文件名
- `path.dirname`，获取路径中的目录名
- `path.extname，`获取文件的后缀名
- `path.isAbsolute`，判断一个路径是否为据对路径
- `path.parse`，得到一个对象，包括root 根路径，dir 目录，base 包含后缀名的文件名，ext 后缀名，name 不包含后缀名的文件名
- `path.join`，拼接路径的，可以传不限量的参数，当需要拼接路劲时就用它避免拼错

```
path.join('c:/a','b')	=>'c:\\a\\b'，这里是window系统的结果，需要对\进行转义
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
response.setHeader('Content-Type','text/plain;charset=utf-8')
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

## 修改完代码自动重启

- 我们这里可以使用一个第三方命名工具，`nodemon`来帮我们解决频繁修改代码重启服务器问题
- `nodemon`是一个基于node.js开发的一个第三方命令行工具，我们使用的时候需要独立安装

```
npm install nodemon --global
```

安装完毕之后，使用

```
// 默认
node app.js
// 用nodemon，这样执行之后nodemon会监视app.js的改变，当保存时自动重启服务器
nodemon app.js
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

## 通过服务器让客户端重定向

- 将状态码设置为302，临时重定向	`statusCode`
  - 在响应中通过Location告诉客户端往哪儿重定向	`setHeader`
- 如果客户端发现收到服务器的响应的状态码是302，就会自动去响应头中找location。所以就能看到客户端跳转。

```javascript
res.statusCode = 302
res.setHeader('Location','/')
res.end()
```

## 处理网站中的静态资源

- 因为读取的html文件中可能还会因为link，script标签等再引入别的文件，在服务端也视为请求，所以主要是对他们做一定的处理。
- 为了让目录结构保持统一清晰，我们约定把所有的html文件都放在views（视图）中
- 为了统一处理静态资源，我们约定把所有的静态资源都存放在public文件夹内。
- 注意，这样之后服务端中的文件路径就不要再用相对路径了，因为我们开放了`/public/`路径，因此要改成`/public/`开头这种形式。

```javascript
//	/comment?name=sdff&messagae=sdjfkl
对于这种表单的请求路径，由于其中具有用户动态填写的内容，所以你不可能通过去判断完整的url路径来处理这个请求。所欲我们只需判断请求头 /comment 即可。对于提交的详细信息可以借助url模块的方法。
var url = require('url')
var obj = url.parse('/comment?name=sdff&messagae=sdjfkl',true)
//obj为一个对象，里面包含请求的各种信息，通过true可以将query拆分成对象形式以获得数据
console.log(obj)
obj.pathname	//不包含?之后的pathname，例如上例就是/comment
obj.query	//被转成对象的查询字符串
```

```javascript
//最终的index.js代码，html以及一些css代码不做展示
var http = require('http')
var fs = require('fs')
var template = require('art-template')
var url = require('url')
let userInfo = [
	{
		name: '张三',
		message: '我是第一个',
		dateTime: '2020-7-11'
	},{
		name: '李四',
		message: '我是第二个',
		dateTime: '2020-7-11'
	}
]
// 这里采用了http模块的请求和监听合并的写法，链式调用的简写
http.createServer(function (req,res) {
	var obj = url.parse(req.url,true)
	var pathname = obj.pathname
	if(pathname==='/') {
		fs.readFile('./views/index.html', function(err,data) {
			if(err) {return res.end('404 not found')}
			else{
				let userStr = template.render(data.toString(),{
					comment: userInfo
				})
				res.end(userStr)
			}
		})
	}
	else if(pathname==='/post') {
		fs.readFile('./views/post.html',function (err,data) {
			if(err) {return res.end('404 not found')}
			else{
				res.end(data)
			}
		})
	}
	else if(pathname.indexOf('/public/')===0) {
		console.log(pathname)
        //这个地方注意是读文件，因此不要忘了前面的点，挺坑的
		fs.readFile('.'+pathname,function (err,data) {
			if(err) {return res.end('404 not found')}
			else{
				res.end(data)
			}
		})
	}
	else if(pathname==='/comment') {
		obj.query.dateTime = new Date()
		userInfo.unshift(obj.query)
        // 请求重定向,301:永久重定向，浏览器会记住；302，临时重定向，浏览器不记忆
		res.statusCode = 302
		res.setHeader('Location','/')
		res.end()
	}
	else {
		fs.readFile('./views/404.html' ,function(err,data) {
			if(err) {return res.end('404 not found')}
			else{
				res.end(data)
			}
		})
	}
}).listen(3000,function() {
	console.log('server is running')
})
```

# express

原生的http在某些方面表现不足以应付我们的开发需求，所以我们就需要使用框架来加快我们的开发效率。让我们的代码更高度统一。在node中，有很多web开发框架，这里以学习`express`为主。

- 安装，`npm install express --save`

```javascript
// 开始一个hello world
var express = require('express')
var app = express()		//相当于http.createServer
app.get('/',function (req,res) {	//根据默认路径返回hello world
    //在express中可以直接	req.query来获取查询字符串，省去了用原生的url的parse方法加query属性
    res.send('hello world')
})
app.listen(3000,function () {		//设置服务器端口
    console,log('server is running in 3000port')
})
```

- `res.redirect('/')`，重定向

## 基本路由

- get

```javascript
//当你以get方法请求 / 时，执行对应的处理函数
app.get('/',function (req,res) {
	res.send('hello world')
})
```

- post

```javascript
// 当你以post方法请求 / 时，指定对应的处理函数
app.post('/',function (req,res) {
	res.send('get a post request')
})
```

## 静态服务

- 开放某些静态资源

```javascript
//	当你以 /public/ 开头的时候，去./public目录中找对应的资源，推荐这种
app.use('/public/',express.static('./public/'))
//	当省略第一个参数时，访问的时候需要省略/public，直接访问public目录下文件的目录
//	如原本是/public/index.html，现在需要是/index.html
app.use(express.static('./public/'))
//	这里a就相当于public的别名，现在访问就是/a/index.html
app.use('/a/',express.static('./public/'))
```

## express中获取请求体参数

### 获取post请求体参数

- 在express中没有内置获取表单POST请求体的API，这里我们需要使用一个第三方包,`body-parser`

安装：`npm install body-parser --save`

配置：

```javascript
var express = require('express')
// 引包
var bodyParser = require('body-parser')
var app = express()
// 配置body-parser，只要加入这个配置则在req请求对象上会多出一个属性：body
// 也就是说你可以直接通过req.body 来获取表单post请求体shu'ju'le
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
// 可以获得req.body来获取post请求体得数据
req.body
```

### 获取get请求体参数

- `req.query`可以直接获得

## 在express中使用art-template模板引擎

安装：

```
npm install art-template --save
npm install express-art-template --save
```

配置并使用：

```javascript
配置使用 art-template 模板引擎
//	第一个参数表示，当渲染以 .html 结尾的文件时，使用 art-template 模板引擎
//	express-art-template 是专门用来在 express 中把art-template整合到exprees中
//	虽然这里不需要加载，但是也必须安装，因为express-art-template 依赖了art-template
app.engine('html',require('express-art-template'))

//	express为response响应对象提供了一个方法：render，默认不可用，但是配置了模板引擎就可以
res.render('html模板名',{模板数据})
//	第一个参数不能写路径，默认会去项目中的views目录查找该模板文件。第二个参数没数据时可以不传，那么就是直接渲染原始html

//如果想要修改默认的views目录，则可以
app.set('views',render函数的默认路径)
```

- 模板引擎有引用和继承两种方式来实现更好的代码复用。

## 路由模块封装

```javascript
//router.js，专门用来包装路由的
var fs = require('fs')
var express = require('express')
//创建一个路由容器
var router = express.Router()
//把路由挂载到router容器中
router.get('/students',function (req,res) {
    ...
})
router.post('/students',function (req,res) {
    ...
})
router.get('/students/new',function (req,res) {
    ...
})
module.exports = router
------------------
//app.js
var express = require('express')
var router = require('./router')
var app = express()
//把路由容器挂载到app服务器中
app.use(router)
```

## crud简单实现

- 自己查看文件夹代码

## express中session用法

- session默认是存储在服务器内存中的，服务器一重启session就会消失。真正的生产环境会把session进行持久化存储。

- `npm install express-session`，然后通过require引入
- 配置，一定要在app.use(router)之前
- 使用，当我们把这个插件配置好之后，就可以通过`req.session.属性 = 值`来设置session，之后也可以通过同样方法来读取session值。

```javascript
var express = require('express')
var session = require('express-session')
var app = express()
//	配置
app.use(session({
  secret: 'keyboard cat',	//配置加密字符串，增加安全性的
  resave: false,
  saveUninitialized: true	//无论你是否使用session，都会默认直接给你分配一把钥匙。如果为false，则是只在存数据时才会给你分发钥匙。
}))
//	使用
req.session.foo = 'bar'	//添加
req.session.foo	//读取
req.session.foo = null	//清除登录状态
```

## express中cookie用法

### 基本用法

1. 安装，`npm install cookie-parser --save`
2. 引入，`const cookieParser = require("cookie-parser")`；
3. 设置中间件，当然要在挂载路由前，`app.use(cookieParser())`
4. 设置cookie，在响应的时候设置

```javascript
// res.cookie（名称，值，{配置信息}
res.cookie("name",'zhangsan',{maxAge: 900000, httpOnly: true});
例如：
router.get('/setcookie',function(req,res) {
	//基础设置cookie，有效期为一个会话，即当前浏览器关闭之后就没了
	res.cookie("isLogin","true");
    //配置cookie额外参数
 	res.cookie("name",'zhangsan',{maxAge: 900000,httpOnly: true})
	res.send("cookie设置成功")
})
router.get('/admin',fucntion(req,res) {
	if(req.cookies.isLogin === 'true'){
    	res.send('登录成功')
	}else{
        res.send('请先登录')
    }           
})

关于设置cookie的参数说明：
1. domain: 域名  
2. name=value：键值对，可以设置要保存的 Key/Value，注意这里的 name 不能和其他属性项的名字一样 
3. Expires： 过期时间（秒），在设置的某个时间点后该 Cookie 就会失效，如 expires=Wednesday, 09-Nov-99 23:12:40 GMT。
4. maxAge： 最大失效时间（毫秒），设置在多少后失效 。
5. secure： 当 secure 值为 true 时，cookie 在 HTTP 中是无效，在 HTTPS 中才有效 。
6. Path： 表示 在那个路由下可以访问到cookie。
7. httpOnly：是微软对 COOKIE 做的扩展。如果在 COOKIE 中设置了“httpOnly”属性，则通过程序（JS 脚本、applet 等）将无法读取到COOKIE 信息，防止 XSS 攻击的产生 。
8. singed：表示是否签名cookie, 设为true 会对这个 cookie 签名，这样就需要用 req.signedCookies 而不是 req.cookies 访问它。被篡改的签名 cookie 会被服务器拒绝，并且 cookie 值会重置为它的原始值。
```

### 多个二级域名共享cookie

- 只需要增加res.cookie中option对象的值，即可实现对相应路由下多个二级路由的cookie进行共享，代码如下：

```javascript
const express=require("express");
const cookieParser=require("cookie-parser");
var app=express();
//设置中间件
app.use(cookieParser());
app.get("/",function(req,res){
	res.send("首页");
});
//设置cookie
app.get("/set",function(req,res){
	res.cookie("userName",'张三',{maxAge: 200000, httpOnly: true,domain: "ccc.com"});
	res.send("设置cookie成功");
});
//获取cookie
app.get("/get",function(req,res){
	console.log(req.cookies.userName);
	res.send("获取cookie成功，cookie为："+ req.cookies.userName);
});
app.listen(8080);
//不同的二级域名也能访问到相同的cookie，只要满足ccc.com这个顶级域名就行。
```

### cookie加密

```
// 将cookieParser添加secret参数
app.use(cookieParser('secret'))
// 设置加密操作
res.cookie('login','true',{
	signed: true
})
// 加密完之后进行获取需要先进行解密才能获取
req.signedCookies.login
```

### 加密原理解析

- node为我们提供了一个核心安全模块“crypto”，它提供了很多安全相关的功能，如摘要运算、加密、电子签名等。这样，我们便可很轻易的封装一个加密模块：

```javascript
//MD5.js
const crypto=require('crypto');
module.exports={
	//MD5封装
	MD5_SUFFIX:'s5w84&&d4d473885s2025s5*4s2',
	md5:function(str){
		var obj=crypto.createHash('md5');
		obj.update(str);		
		return obj.digest('hex');
	}
}
//xx.js
const common=require('./MD5');
var str='123456';
var str=common.md5(str+'s5w84&&d4d473885s2025s5*4s2');
//设置并加密cookie
app.use(function(req,res,next){
	res.cookie("userName", str, {maxAge: 5*60*1000, httpOnly: true});
	res.end("set ok");
});
```

- 如果是在判断登录时，只需将用户输入的账号进行同样加密操作在进行比较即可知道账户是否正确。
  crypto所涉及的加密方式有很多，推荐大家都写模块引用，这样更方便后期的维护。

## md5加密

- 可以在github上搜索相关项目，通过npm安装后可以对密码进行加密。

# 中间件

## 中间件概念

- 从客户端请求到服务端发送响应的过程，中间的一系列处理环节，把它分步骤来处理，每个步骤处理一些事，这个步骤就是中间处理环节。处理环节的方法就叫中间件。

## express中间件

- 中间件本身是一个方法，该方法接收三个参数，`request` 请求对象，`response` 响应对象，`next` 下一个中间件，next参数是可选的。第一个中间件执行的时候，不调`next`方法是不会向下一个中间件走的

- 在express中，对中间件有几种分类

  - 不关心请求路径和请求方法的中间件，也就是说任何请求都会进入这个中间件。

    ```javascript
    app.use(function (req,res,next){	//无论什么请求都会进来
    	console.log('请求进来了')
    })
    ```

  - 以`/xxx`开头的中间件

    ```javascript
    app.use('/a',function (req,res,next){	//无论什么请求都会进来
    	console.log('只有/a开头的请求才会进来')
        //例如/a/b/c
        console.log(req.url)   //打印的结果是/b/c，会将/a过滤掉，但是传给后面的还是原样的url
        next()			
    })
    ```

  - 除了以上中间件之外，还有一种最常用的严格匹配请求方法和请求路径的中间件，例如`router.get`，`router.post`，也有next参数，不过一般我们都没调用。

- 由上图几种中间件分类可知，我们可以对一个请求进行多次中间件处理来达到我们想要的效果。

## 配置中间件

- 配置404界面

```javascript
//	顺序处理很重要，必须要配置在其他路由后面，这样当前面的路由都不匹配时便渲染404界面
app.use(router)
app.use(function(req,res,next) {
    res.render('404.html')
})
```

- 配置服务器错误界面

```javascript
//	顺序也很重要，而且因为具有err参数，当next(err)时只会去找带有err参数即四个参数的中间件
//	所以会跳过404中间件，所以不必担心服务器错误响应被404截取的问题
//	router.js
router.get('/',function(req,res,next) {
    fs.readFile('./sjlfj.dsfjl',function (err,data) {
        if(err){
            return next(err)
        }
    })
})
//	app.js
app.use(function(err,req,res,next) {
    res.status(500).json({
        err_code: 500,
        message: err.message
    })
})
```

- 配置跨域中间件

```javascript
npm i -S cors
const cors = require('cors')

// ...
app.use(cors())
```

再次请求即可成功，这里我们在 Network 中会发现发起了两次 https 请求，这是因为由于触发跨域，所以会首先进行 OPTIONS 请求，判断服务端是否允许跨域请求，如果允许才能实际进行请求

# node爬虫技术

## 传统爬虫

- 这里先介绍个简单爬虫

```javascript
//如果是图片可以直接就二进制格式进行写入，如果是文字不要忘了加utf8编码。
//转换文字示例，这里借助了gbk模块将二进制文件转换为urf8文件。有些网站比较简单，它本身的文字就直接是utf8这样就会出错，这时可以直接输出字符串查看。
npm install gbk
var gbk = require('gbk')
var html = gbk.toString('utf-8',data)
fs.writeFile('xx.html',html)
//爬虫示例
const fs = requrie('fs')
const url = require('url')
getUrl("https://xxx.taobao.com",(data) => {
    fs.writeFile("xxx.jpg",data)
})
function getUrl(sUrl,success) {
    var urlObj = url.parse(sUrl)
    var http = ''
    if(urlObj.protocol ==='http:'){
        http = require('htpp')
    }
    else{
        http = require('https')
    }
    let req = http.request({
        'hostname': urlObj.hostname,
        'path': urlObj.path
    },res => {
        var arr = []
        var str = ''
        res.on("data",buffer  => {
            arr.push(buffer)
            str+= buffer
        })
        res.on("end",() => {
            let b = Buffer.concat(arr)
            success && success(b)
        })
    })
    //将请求结束
    req.end()
    //处理错误条件，如果出错进不来这里
    req.on('error',() => {
        console.log('404了，哥们，网址找不到')
    })
}
```

- 爬虫中对html字符串进行DOM操作从而取出相关值

```
npm install jsdom
var JSDOM = require('jsdom').JSDOM
let DOM = new JSDOM(html字符串)
let document = DOM.window.docuement
console.log(document.querySelector('.hehe').innerHTML)
```

- 小说网站挖掘示例

```
//这里需要用到中文中的关键词挖掘模块segment
npm install segment
const Segment = require('segment')
let seg = new Segment()
seg.useDefault()
var str = '今天天气小雨'
console.log(seg.doSegment(str))		//会返回传入的字符串到底由哪些词组成，格式是一个数组，元素为对象
```

## 爬虫更进一步

- 这里就是利用`axios`或者`request`模块来发起请求，然后利用`cheerio`模块来抓取页面模块，该模块是为服务器定制的，快速，灵活实施的jquery实现。适合各种web爬虫程序。

### 爬取表情包

- 这里写个思路，就是自己先搜索获得网页地址，之后观察网页源代码，找出自己想要爬取的元素的dom规律，对其内容进行读取。之后根据需要创建文件夹并进行写入。

### 爬取音乐

- 这里主要的不同是，有些内容属于前端渲染，即直接查看源代码无法获取。那么我们就要自己去network中找`XHR`请求，即ajax请求，找到我们想要的信息的ajax请求，然后再利用他进行爬取。
- 注意，图片，音频，视频这些文件是以二进制格式返回的，所以响应类型要记得设置为流类型，然后以流形式进行写入。

```javascript
//流形式读取，写入示例
async function download(mp3Url,title) {
    let res = axios.get(mp3Url,{    //因为音频是二进制文件，所以响应类型为流形式
        responseType: "stream"
    })
    let ws = fs.createWriteStream('./mp3/'+title+".mp3")
    res.data.pipe(ws)     //写入
    res.data.on("close",function (){
        ws.close()  //关闭写入流
    })
}
```

### puppeteer

- Puppeteer 是一个 Node 库，它提供了一个高级 API 来通过 [DevTools](https://zhaoqize.github.io/puppeteer-api-zh_CN/(https://chromedevtools.github.io/devtools-protocol/)) 协议控制 Chromium 或 Chrome。Puppeteer 默认以 [headless](https://developers.google.com/web/updates/2017/04/headless-chrome) 模式运行，但是可以通过修改配置文件运行“有头”模式。

```javascript
npm install puppeteer --save
let puppeteer = require('puppeteer')
async function test() {
    //puppeteer.launch实例开启浏览器，可以传入一个options对象，可以配置为无界面浏览器，也可以配置为
    //有界面浏览器，无界面浏览器性能更好点。有界面一般用于调试开发。
    let browser = await puppeteer.lanucn({
        headless: false,     //设为有界面
        defaultViewport: {      //设置浏览器宽高，默认为800*600
            width:1400,
            height:800
        }
    })
    //创建页面对象
    let page = await browser.newPage()
    //访问页面
    await page.goto("http://www.baidu.com")
    //截图
    await page.screenshot({path:'screenshot.png'})
    //获取页面内容，$$eval函数使得我们的回调函数可以运行在浏览器中，并且可以通过浏览器的方式进行监听
    page.$$eval("#menu li a",(elements) => {
        console.log(elements)   //这个输出是在浏览器的控制台输出
    })
    //事件监听
    page.on('console',function(eventMsg) {
        console.log(eventMsg.test())   //在终端输出
    })
}

//也可以模仿事件点击
elementsHandles = await page.$$("#menu li a")
elementsHandle[2].click()
```

# 将服务器部署到公网上

## 通过内网穿透部署

- 比较简单，利用一款叫做`花生壳工具`即可部署，可以叫做内网穿透技术，该工具也可以下载桌面端以方便管理。