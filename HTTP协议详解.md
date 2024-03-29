HTTP协议详解 

#### 爬虫又称`网路爬虫`，先了解一下什么是网络？网络是由若干节点和连接这些节点的链路构成，然后网络与网络之间所串连成的庞大网络叫做互联网，HTTP（HyperText Transfer Protocol 超文本传输协议）是互联网上应用最为广泛的一种网络协议，它是由万维网协会（World Wide wed Consortium)制定发布。

> HTTP请求整个过程：HTTP起源、TCP/IP协议、建立TCP连接、客户端请求、服务端响应、断开TCP连接、HTTP相关知识   。   

HTTP请求大概流程

1. 客户端   ->  访问www.baidu.com  ->    通过DNS域名解析    ->  ip  115.239.210.23 ->   服务器
2. 客户端  ->  建立TCP连接   ->     服务器
3. 客户端   ->  发起请求         ->     服务器进行处理
4. 服务器   ->   响应结果         ->    客户端
5. 服务器   ->   断开TCP连接  ->    客户端

##### 1.起源

一位计算机科学家`蒂姆-伯纳斯-李`的构想。1991年8月6日，蒂姆-伯纳斯-李在位于欧洲粒子物理研究所（CERN）的NexT计算机上，正式公开运行世界上第一个Web网站（http://info.cern.ch）,建立起基本的互联网概念和技术体系，由此开启了网络信息时代的序幕。

> 伯纳斯-李的提案包含了网路的基本概念并逐步建立了所有必要的工具                                                                                                                    

1. 提出`HTTP（HyperText  Transfer  Protocol ）`超文本传输协议，允许用户通过单击超链接访问资源。
2. 提出使用`HTML（HyperText Markup Language）`标记语言作为创建网页的标准。
3. 创建了统一的资源定位器`URL(Uniform Resource Locator)`作为网站地址系统,就是沿用自今的http://www URL格式。
4. 创建了第一个Web浏览器，成为万维网浏览器，这也是一个Web编译器。
5. 创建了第一个Web服务器（http://info.cern.ch），并描述了本身的第一个Web页面。

##### 2.特点

> HTTP协议一共有5大特点

1. 支持客户/服务器模式
2. 灵活：          HTTP允许传输任意类型的数据对象，正在传输的类型有`Content-Type`是HTTP包中用来表示内容类型的标识。
3. 简单快速：  客户向服务器请求服务时，只需传送请求方法和路径。
4. 无连接：      无连接的含义就是限制每次连接只处理一个请求。服务器处理完客户的请求，收到客户的应答后，即断开连接，采用这种方式可以节省传输时间。
5. 无状态：      无状态是指协议对于事物处理没有记忆能力，服务器不知道客户端是神马状态。即我们给服务器发送HTTP请求之后，服务器根据需求，会给我们发送数据过来，但是，发送完，不会记忆任何信息（`Cookie`/`Session`孕育而生）。

##### 二.TCP/IP协议

我们经常听到一句话就是：`HTTP是一个基于TCP/IP协议蔟来传送数据。`

TCP/IP基础----层次图

1. 应用层 ： Ping   、  HTTP、   SMTP、  FTP 、  Telnet   、 Traceroute   、 DNS   、SNMP、   NFS 、   RTP ....
2. 传输层： TCP     、   UDP
3. 网络层 ： ICMP 、   IP 、 IGMP 
4. 链路层 :    ARP   、   Data Link  RARP
5. 物理层：  Media

HTTP使用的传输层协议是`TCP协议` 而网络层使用的是`IP协议` ，所以说HTTP是一个基于TCP/IP协议簇来传送数据。

> PS：同样ping使用的是ICMP协议，这也就是为什么有时候我们开vps可以上网，但是ping  google 却ping不同的原因，因为走的是不同的协议。

那么TCP/IP协议簇是如何工作的。

|                         数据封装                         |                   数据拆分                    |
| :------------------------------------------------------: | :-------------------------------------------: |
|                        应用数据体                        |                  应用数据体                   |
|                   TCP头  \|   应用数据                   |               TCP头 \| 应用数据               |
|           IP头  \|    TCP头     \|    应用数据           |         IP头  \|  ITCP头 \| 应用数据          |
| 链路层数据头   \|   IP头   \|    TCP头     \|   应用数据 | 链路层数据头\|    IP头  \| TCP头 \|  应用数据 |
|              电信号传输（光纤、无线电波等）              |                                               |



在数据的发送端是一层一层封装数据，数据接收一层一层拆封，最后应用层获得数据。

##### 三. 建立TCP连接

知道了TCP/IP协议簇大致的工作原理之后，我们来看看HTTP是如何建立连接的。

##### 1.TCP包头信息

前面讲到`HTTP是一个基于TCP/IP协议簇来传输数据` ，所以这HTTP建立连接也就是建立TCP连接，TCP如何建立连接，一起来看看TCP包信息结构吧

|                      TCP头信息                      |                 数据封装                 |
| :-------------------------------------------------: | :--------------------------------------: |
|       源端口号（16位）  \| 目标端口号（16位）       |                应用数据题                |
|                   序列号（32位）                    |            TCP头 \| 应用数据             |
|                 确认序列号（32位）                  |        IP头  \| TCP头  \|应用数据        |
| 数据偏移（4位） \| 保留（6位）     窗口大小（16位） | 链路层数据头\| IP 头 \|TCP 头 \|应用数据 |
|          校验和（16位）   紧急指针（16位）          |                                          |
|                  选项（长度可变）                   |                                          |

`TCP报文包 = TCP头信息 + TCP数据体`，而在TCP头信息中包含了6中控制位（原本想在表格中写，发现上面表格太小写不下，嘿嘿）

1. URG   :  紧急数据（urgent data ）--这是一条紧急信息
2. ACK    :  确认已收到
3. PSH    :  提示接收端的应用程序应该立即从TCP缓存区中读走数据
4. RST    :  表示要求对方重新建立连接
5. SYN   :  表示请求建立一个连接
6. FIN    ： 表示通知对方本端要关闭连接了 

##### 2. 建立连接过程

了解TCP头信息之后，我们就可以正式看看TCP建立连接的三次握手了。

1. 客户端 socket connect(主动打开)                                          服务器 socket,bind listen accept(被动等待)
2.    ->       发送SYN     J         ->    
3.   <-       Connect返回         <-
4.   ->          ack  K+1              ->                                                     Accept 返回  ，read

三次握手协议指的是在发送数据的准备阶段，服务器端和客户端之间需要进行三次交互

三次握手详解：

1. 客户端发送位码为syn = 1 ，随机产生seq  number  = 1234567 的数据包到服务器，服务器由syn = 1 知道客户端要求建立的联机（客户端：我要连接你）

   > ps :   SYN：同步序列编号(Synchronize Sequence Numbers)       seq是序号 ，   ack是确认号

2. 服务器收到请求后要确认联机信息，向A发送ack  number = (客户端的seq   +1 ),syn = 1 ,ack  = 1 ,随机产生seq = 7654321的包（服务器：好的，你来吧）

3. 客户端收到后检查ack number 是否正确，即第一次发送的seq  number +1 ,以及位码ack 是否为1 ，若正确，客户端会再发送ack number = (服务器的seq +1),ack = 1,服务器收到后确认seq值与ack = 1 则会建立成功

> 面试官：为什么http建立连接需要三次握手，不是两次或四次
> 答：三次是最少的安全次数，两次不安全，四次浪费资源

##### 四.客户端请求

客户端与服务器连接上了之后，客户端就可以向服务器请求资源，就可以开始发送HTTP请求了。

##### 1. HTTP请求报文结构

我们之前说过`TCP报文包 = TCP头信息 + TCP数据体`，TCP头信息我们以及讲过了，现在来讲TCP数据体，也就是我们的HTTP请求报文。

|    HTTP请求报文结构    |
| :--------------------: |
|         请求行         |
|         请求头         |
| 空行（表示请求体开始） |
|         请求体         |
| 空行（表示请求体结束） |

##### 2. HTTP请求实例

看看实际的HTTP请求例子

> 请求网址:https://iknowpc.bdimg.com/static/common/widget/upgrade-tips/upgrade-tips.f217a81.css
>
> 请求方法:GET
>
> 状态码:
>
> 200
>
> 版本:HTTP/2.0
>
> Referrer 政策:no-referrer-when-downgrade

​                                                                                   HTTP请求报文

1. 请求方法：HTTP/1.1定义的请求方法有8种

   GET  , POST  , DELETE  ,   PATCH  , HEAD  ,  OPTIONS  ,  TRACE  ,  PUT   最常用两种GET和POST，如果是RESTful接口的话一般会用到  GET  ,  POST  ,  DELETE  ,  PUT

   请求对应的URL地址，它和报文头的Host属性组成完整的请求URL

2. 协议名称及版本号
3. HTTP的报文头，报文头包含若干个属性，格式为“属性名称：属性值”，服务端据此获取客户端的信息
4. 报文体，他将一个页面表单中的组件值通过param1=value1&param2=value2的键值对形式编码成一个格式化串，它承载多个请求参数的数据。
5. 不但报文体可以传递请求参数，请求的URL也可以通过类似与‘/chapter15/user.html?param1=value1’的方式传递请求参数

请求头参数非常多  说明两个低级的反扒参数

> 1. User-Agent:客户端使用的操作系统和浏览器的名称和版本，有些网站会限制请求浏览器
> 2. REferer:先前的网页的地址，表示此请求来自哪儿，有些网站会限制请求来源

##### 五：服务器响应

服务器在收到客户端请秋处理完需要响应并返回客户端，而HTTP的响应报文结构与请求结构体一致。

##### 1.HTTP响应报文结构

|    HTTP响应报文结构    |
| :--------------------: |
|         相应行         |
|         响应头         |
| 空行（表示请求体开始） |
|         响应体         |
| 空行（表示请求体结束） |

##### 2.响应状态码

响应报文中我们重点关注下：服务器的响应状态码

​                                                                               HTTP状态码分类

| 分类 | 分类描述                                       |
| :--- | ---------------------------------------------- |
| 1**  | 信息，服务器收到请求，需要请求者继续执行操作   |
| 2**  | 成功，操作被成功接受并处理                     |
| 3**  | 重定向，需要进一步的操作已完成操作             |
| 4**  | 客户端错误，请求包含语法错误或无法完成请求     |
| 5**  | 服务器错误，服务器在处理请求的过程中发生了错误 |

##### 六. 断开连接

------

在服务器响应完毕后，一次会话纠结束了，这时候连接会断开吗？



##### 1.长连接

是否断开我们需要区分HTTP版本：

* 在HTTP/1.0版本的时候，客户端与服务器完成一次请求/响应之后，会将之前建立的`TCP连接断开`，下次请求的时候又要重新建立TCP连接，这也称为短链接。

* 在HTTP1.0发布仅半年后（1997年1月），HTTP/1.1版本发布带来一个新功能：

  在客户端与服务器完成一次请求/响应之后，允许不断开TCP连接，这意味着下次请求就直接使用这个TCP连接而不在需要重新握手建立连接，，这也被称为长连接。

> 注意： 长连接是指一次TCP连接允许多次HTTP会话，HTTP永远都是一次请求/响应，会话结束，HTTP本身不存在长连接之说。

早在1999年HTTP/1.1就推广普及，所以现在浏览器在请求时请求头中都会携带一个参数：`Connection:Keep-alive`,这表示浏览器要求与服务器建立长连接，而服务器也可以设置是否愿意建立长连接。

##### 2.长连接的优点

对于服务器来说建立长连接有优点也有缺点：

* 优点：

  当网站中有大量的静态资源（css,js,图片等）就可以使用长连接，这几个图片就可以通过一次TCP连接发送。

* 缺点：

  当客户端请求一次不在的请求，而服务器却开着长连接资源被占用着，这是很严重的资源浪费。所以是否开启长连接，长连接时间都需要根据网站自身来合理设置。

##### 3.断开连接过程

在建立TCP连接时是三次握手，而断开是四次挥手！

|   客户端   |                                    |   server   |
| :--------: | :--------------------------------: | :--------: |
| FIN_WAIT_1 |  ->           FIN  M          ->   | CLOSE_WAIT |
| FIN_WAIT_2 |   <-          ack   M+1       <-   |            |
| TIME_WAIT  | <-            FIN   N           <- |  LAST_ACK  |
|            |     ->   ACK=1  ack=K+1    ->      |   CLOSE    |

前面讲TCP/IP协议时我们说过标志位：`FIN表示通知对方本端要关闭连接了，那段开连接为何需要四次挥手呢？`

#####  七. 相关知识

##### 1.http2.0

1. HTTP/0.9   1991年
2. HTTP/1.0   1996年
3. HTTP/1.1   1999年
4. HTTP/2      2015年

HTTP/1.1已经为我们服务了20年，而HTTP/2.0其实在2015年就发布了，但是还没有推广开来。

##### 2.http&rpc

因为http`响应慢`、`请求头体积大`等缺点，所以在为服务时代，大家都使用rpc来调用服务。

##### 3. http&https

http还有两个很大的缺点就是`明文`且`不能保证完整性`,所以目前会渐渐被HTTPS代替