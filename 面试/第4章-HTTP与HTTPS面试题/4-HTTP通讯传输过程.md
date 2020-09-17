### 四、HTTP通信传输

![这里写图片描述](https://img-blog.csdn.net/20180719094739178?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9taW5nMTAwMDAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

客户端输入URL回车，DNS解析域名得到服务器的IP地址，服务器在80端口监听客户端请求，端口通过TCP/IP协议（可以通过Socket实现）建立连接。HTTP属于TCP/IP模型中的运用层协议，所以通信的过程其实是对应数据的入栈和出栈。
![这里写图片描述](https://img-blog.csdn.net/20180719094756330?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9taW5nMTAwMDAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
报文从运用层传送到运输层，运输层通过TCP三次握手和服务器建立连接，四次挥手释放连接。

![这里写图片描述](https://img-blog.csdn.net/20180719110828114?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9taW5nMTAwMDAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

为什么需要三次握手呢？为了防止已失效的连接请求报文段突然又传送到了服务端，因而产生错误。

比如：client发出的第一个连接请求报文段并没有丢失，而是在某个网络结点长时间的滞留了，以致延误到连接释放以后的某个时间才到达server。本来这是一个早已失效的报文段，但是server收到此失效的连接请求报文段后，就误认为是client再次发出的一个新的连接请求，于是就向client发出确认报文段，同意建立连接。假设不采用“三次握手”，那么只要server发出确认，新的连接就建立了，由于client并没有发出建立连接的请求，因此不会理睬server的确认，也不会向server发送数据，但server却以为新的运输连接已经建立，并一直等待client发来数据。所以没有采用“三次握手”，这种情况下server的很多资源就白白浪费掉了。

![这里写图片描述](https://img-blog.csdn.net/20180719110841774?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpYW9taW5nMTAwMDAx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

为什么需要四次挥手呢？TCP是全双工模式，当client发出FIN报文段时，只是表示client已经没有数据要发送了，client告诉server，它的数据已经全部发送完毕了；但是，这个时候client还是可以接受来server的数据；当server返回ACK报文段时，表示它已经知道client没有数据发送了，但是server还是可以发送数据到client的；当server也发送了FIN报文段时，这个时候就表示server也没有数据要发送了，就会告诉client，我也没有数据要发送了，如果收到client确认报文段，之后彼此就会愉快的中断这次TCP连接。