### 一、前言：

![这里写图片描述](https://img-blog.csdn.net/20180719095113808?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9taW5nMTAwMDAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180719133425838?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9taW5nMTAwMDAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
先来观察这两张图，第一张访问域名http://www.12306.cn，谷歌浏览器提示不安全链接，第二张是https://kyfw.12306.cn/otn/regist/init，浏览器显示安全，为什么会这样子呢？2017年1月发布的Chrome 56浏览器开始把收集密码或信用卡数据的HTTP页面标记为“不安全”，若用户使用2017年10月推出的Chrome 62，带有输入数据的HTTP页面和所有以无痕模式浏览的HTTP页面都会被标记为“不安全”，此外，苹果公司强制所有iOS App在2017年1月1日前使用HTTPS加密。

### 二、HTTP和HTTPS发展历史

##### 什么是HTTP?

> 超文本传输协议，是一个基于请求与响应，无状态的，应用层的协议，常基于TCP/IP协议传输数据，互联网上应用最为广泛的一种网络协议,所有的WWW文件都必须遵守这个标准。设计HTTP的初衷是为了提供一种发布和接收HTML页面的方法。

发展历史：

| 版本     | 产生时间 | 内容                                                         | 发展现状           |
| -------- | -------- | ------------------------------------------------------------ | ------------------ |
| HTTP/0.9 | 1991年   | 不涉及数据包传输，规定客户端和服务器之间通信格式，只能GET请求 | 没有作为正式的标准 |
| HTTP/1.0 | 1996年   | 传输内容格式不限制，增加PUT、PATCH、HEAD、 OPTIONS、DELETE命令 | 正式作为标准       |
| HTTP/1.1 | 1997年   | 持久连接(长连接)、节约带宽、HOST域、管道机制、分块传输编码   | 2015年前使用最广泛 |
| HTTP/2   | 2015年   | 多路复用、服务器推送、头信息压缩、二进制协议等               | 逐渐覆盖市场       |

![这里写图片描述](https://img-blog.csdn.net/20180723103857872?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9taW5nMTAwMDAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这个Akamai公司建立的一个官方的演示，使用HTTP/1.1和HTTP/2同时请求379张图片，观察请求的时间，明显看出HTTP/2性能占优势。
![这里写图片描述](https://img-blog.csdn.net/20180723105652242?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9taW5nMTAwMDAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
多路复用：通过单一的HTTP/2连接请求发起多重的请求-响应消息，多个请求stream共享一个TCP连接，实现多留并行而不是依赖建立多个TCP连接。

##### HTTP报文格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019080311162578.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9taW5nMTAwMDAx,size_16,color_FFFFFF,t_70)

##### 什么是HTTPS？

> 《图解HTTP》这本书中曾提过HTTPS是身披SSL外壳的HTTP。HTTPS是一种通过计算机网络进行安全通信的传输协议，经由HTTP进行通信，利用SSL/TLS建立全信道，加密数据包。HTTPS使用的主要目的是提供对网站服务器的身份认证，同时保护交换数据的隐私与完整性。
>
> PS:TLS是传输层加密协议，前身是SSL协议，由网景公司1995年发布，有时候两者不区分。

##### 参考连接： 

1.[https://kamranahmed.info/blog/2016/08/13/http-in-depth/](https://blog.csdn.net/xiaoming100001/article/details/81109617)

2.[https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol](https://blog.csdn.net/xiaoming100001/article/details/81109617)

3.[https://tools.ietf.org/html/rfc1945](https://blog.csdn.net/xiaoming100001/article/details/81109617)

4.[https://http2.github.io/http2-spec/](https://blog.csdn.net/xiaoming100001/article/details/81109617)

5.[https://www.zhihu.com/question/34074946](