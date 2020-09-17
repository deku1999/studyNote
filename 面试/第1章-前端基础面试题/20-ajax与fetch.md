## Ajax

Ajax(asynchronous JavaScript and XML)是使用客户端上的许多 Web 技术，创建异步 Web 应用的一种 Web 开发技术。借助 Ajax，Web 应用可以异步（在后台）向服务器发送数据和从服务器检索数据，而不会干扰现有页面的显示和行为。通过将数据交换层与表示层分离，Ajax 允许网页和扩展 Web 应用程序动态更改内容，而无需重新加载整个页面。实际上，现在通常将 JSON 替换为 XML，因为 JavaScript 对 JSON 有原生支持优势。<br>
XMLHttpRequest API 经常用于异步通信。此外还有最近流行的fetch API。

```
let xmlhttp
if (window.XMLHttpRequest) {
	//  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
	xmlhttp = new XMLHttpRequest()
} else {
	// IE6, IE5 浏览器执行代码
	xmlhttp = new ActiveXObject("Microsoft.XMLHTTP")
}
xmlhttp.onreadystatechange = () => {
	if (xmlhttp.readyState === 4 && xmlhttp.status === 200) {
		document.getElementById("myDiv").innerHTML = xmlhttp.responseText
	}
}
xmlhttp.open("GET", "/ajax/test.txt", true)
xmlhttp.send()
```



## 使用Ajax的优缺点分别是什么

**优点**

* 交互性更好。来自服务器的新内容可以动态更改，无需重新加载整个页面。
* 减少与服务器的连接，因为脚本和样式只需要被请求一次。
* 状态可以维护在一个页面上。JavaScript 变量和 DOM 状态将得到保持，因为主容器页面未被重新加载。
* 基本上包括大部分 SPA 的优点。

**缺点**

* 动态网页很难收藏。
* 如果 JavaScript 已在浏览器中被禁用，则不起作用。
* 有些网络爬虫不执行 JavaScript，也不会看到 JavaScript 加载的内容。
* 基本上包括大部分 SPA 的缺点。 搜索引擎优化比较难

https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md

#### 

## Ajax和Fetch区别

* ajax是使用XMLHttpRequest对象发起的，但是用起来很麻烦，所以ES6新规范就有了fetch，fetch发一个请求不用像ajax那样写一大堆代码。
* 使用fetch无法取消一个请求，这是因为fetch基于Promise，而Promise无法做到这一点。
* 在默认情况下，fetch不会接受或者发送cookies
* fetch没有办法原生监测请求的进度，而XMLHttpRequest可以
* fetch只对网络请求报错，对400，500都当做成功的请求，需要封装去处理
* fetch由于是ES6规范，兼容性上比不上XMLHttpRequest