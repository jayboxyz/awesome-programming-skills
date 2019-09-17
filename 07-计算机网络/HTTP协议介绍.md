## 一、HTTP简介

1. HTTP 协议（HyperText Transfer Protocol，超文本传输协议）是因特网上应用最为广泛的一种网络传输协议，所有的 WWW 文件都必须遵守这个标准。

2. HTTP 是基于 TCP/IP 通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）

3. HTTP协议通常承载于 TCP 协议之上，有时也承载于 TLS 或 SSL 协议层之上，这个时候，就成了我们常说的HTTPS。如下图

   ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-5-52391331.jpg)



4. HTTP 是一个应用层协议，由请求和响应构成，是一个标准的客户端服务器模型。HTTP是一个无状态的协议。
5. HTTP 默认的端口号为80，HTTPS的端口号为443。

总结来说，HTTP 是一个用于传输超媒体文档（hypermedia documents）的应用层协议，其主要使用场景是网络浏览器和网络服务器之间进行通信。

另外说几点：

1. 什么叫无状态(stateless)协议？

   > HTTP 使用 TCP 作为传输协议，Server 与 Client 交互过程中会建立 TCP 连接，但并**不会保存任何 Client 的状态信息**，因此 HTTP 也成为无状态(stateless)协议。

2. 计算机相互之间进行通讯通常有两种方式：

   > 1、请求响应（request-response）
   >
   > 向对端主机**发送请求后需等待响应**，这类通信过程就是由一系列的单次通信构成，典型应用如网页浏览
   >
   > 2、单向（one-way ）
   >
   > 向对端主机**发送消息后无需等待回复**，典型应用如邮件

3. TCP 协议简单介绍：

   > HTTP 通信方式属于请求响应类型，因此单次通信流程通常为：
   >
   > - 建立 TCP 连接
   > - 发送 HTTP 消息（message, 应用层信息单元）
   > - 读取服务器响应
   > - 关闭 TCP 连接
   >
   > 由于 HTTP 为无状态协议（即每次通信间无关联），因此通常来讲，HTTP 每次通信都是使用独立 TCP 连接的。大部分网络应用中，client 和 server 会在一定时间内**通信多次**，从每次 request 和 response 是否通过同一 TCP 链接发送的角度来看，可以划分为两种：
   >
   > ① **短连接（non-persistent connections）**：每个 request/response 对均使用单独 TCP 链接
   >
   > ② **长连接（persistent connections）**：每个 request/response 对均使用同一 TCP 链接
   >
   > 在 HTTP 0.9 和 1.0 中，连接均会在单词请求响应后关闭，而 TCP 连接的建立和消亡需要经过**三路握手、四路断开**，因此需要一定额外时间开销。有鉴于此，**从 HTTP 1.1 开始**，所有连接默认为长连接。更进一步地，如果不必每个单次通信都需等待响应后才能继续下一个通信，则可以实现流水线技术，提高并发量。
   >
   > ![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-5-6-18803151.jpg)


## 二、HTTP 协议工作流程

 一次HTTP操作称为一个事务，其工作过程大概如下：

1. 用户在浏览器中键入需要访问网页的URL或者点击某个网页中链接；

2. 浏览器根据URL中的域名，通过DNS解析出目标网页的IP地址；

   > 浏览器请求这个页面：http://hackr.ip/index.html
   > 在这一步，需要域名系统 DNS 解析域名 hackr.ip，得主机的IP地址 20X.189.105.112。
   > 然后将上面结合本机自己的信息，封装成一个 http 请求数据包

3. 在HTTP开始工作前，客户端首先会通过TCP/IP协议来和服务端建立链接（TCP三次握手）

4. 建立连接后，客户机发送一个请求给服务器，请求方式的格式为：统一资源标识符（URL）、协议版本号，后边是 MIME 信息包括请求修饰符、客户机信息和可内容。

5. 服务器接到请求后，给予相应的响应信息，其格式为一个状态行，包括信息的协议版本号、一个成功或错误的代码，后边是 MIME 信息包括服务器信息、实体信息和可能的内容。

6. 一般情况下，一旦 Web 服务器向浏览器发送了请求数据，它就要关闭 TCP 连接，然后如果浏览器或者服务器在其头信息加入了这行代码：`Connection:keep-alive`，TCP 连接在发送后将仍然保持打开状态，于是，浏览器可以继续通过相同的连接发送请求。保持连接节省了为每个请求建立新连接所需的时间，还节约了网络带宽。

   ![](https://user-gold-cdn.xitu.io/2018/4/16/162cd61e94d0245d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)


   ​

   ![](https://user-gold-cdn.xitu.io/2018/4/16/162cd522ab8157c0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**2.1 短连接：**

短连接的操作步骤是： ***建立连接——数据传输——关闭连接...建立连接——数据传输——关闭连接。***

如果客户请求频繁，将在 TCP 的建立和关闭操作上浪费较多时间和带宽。

![](https://user-gold-cdn.xitu.io/2018/4/16/162cd71c10945b10?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**2.2 长连接：**

> 长链接,指在一个连接上可以连续发送多个数据包，在连接保持期间，如果没有数据包发送，需要双方发链路检测包。

长链接操作步骤：***建立连接——数据传输...（保持连接）...数据传输——关闭连接***

长连接可以省去较多的TCP建立和关闭的操作，减少浪费，节约时间。

长链接分为 without pipelining 和 with pipelining，下图中是 without pipelining，客户端只在收到前一个请求的响应后，才发出新的请求。

![](https://user-gold-cdn.xitu.io/2018/4/16/162cd71f7d780410?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**2.3 管道化**

下图是 with pipelining，每次建立链接后无需等待请求回来就可以发送下一个请求：

![](https://user-gold-cdn.xitu.io/2018/4/16/162cd81efecac01f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 三、HTTP 请求报文

客户端发送一个HTTP请求到服务器的请求消息包括以下格式：

请求行（request line）、请求头部（header）、请求体组成，下图给出了请求报文的一般格式。

![](https://user-gold-cdn.xitu.io/2018/4/16/162cdac93015af8f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

``` html
请求行:
    方法:
        GET 获取资源
        POST 向服务器端发送数据，传输实体主体
        PUT 传输文件
        HEAD 获取报文首部
        DELETE 删除文件
        OPTIONS 询问支持的方法
        TRACE 追踪路径
    协议/版本号
    URL
    
请求头:
    通用首部(General Header)
    请求首部(Request Header)
    响应首部(Response Header)
    实体首部(Entity Header Fields)
    
请求体
```

请求报文拆解：

![](https://user-gold-cdn.xitu.io/2018/4/16/162cdb040f143019?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**3.1 get请求**

![](https://user-gold-cdn.xitu.io/2018/4/16/162cdadfefe0687c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**3.2 post请求**

![](https://user-gold-cdn.xitu.io/2018/4/16/162cdaf4f6a0357b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 四、HTTP 响应报文

HTTP 响应组成：响应行、响应头、响应体。 

![](https://user-gold-cdn.xitu.io/2018/4/16/162cdb6691dac805?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![](https://user-gold-cdn.xitu.io/2018/4/16/162cdb4cb4794f5e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 五、HTTP 状态码

| 类别 |              原因              |
| :--: | :----------------------------: |
| 1.XX |  Informational(信息性状态码)   |
| 2.XX |      Success(成功状态码)       |
| 3.XX |      Redirection(重定向)       |
| 4.XX | Client Error(客户端错误状态码) |
| 5.XX | Server Error(服务器错误状态吗) |

**5.1 2XX 成功**

> 200(OK 客户端发过来的数据被正常处理 
> 204(Not Content 正常响应，没有实体 
> 206(Partial Content 范围请求，返回部分数据，响应报文中由Content-Range指定实体内容 

**5.2 3XX 重定向**

> 301(Moved Permanently) 永久重定向 
> 302(Found) 临时重定向，规范要求，方法名不变，但是都会改变 
> 303(See Other) 和302类似，但必须用GET方法 
> 304(Not Modified) 状态未改变， 配合(If-Match、If-Modified-Since、If-None_Match、If-Range、If-Unmodified-Since) 
> 307(Temporary Redirect) 临时重定向，不该改变请求方法

**5.3 4XX 客户端错误**

> 400(Bad Request) 请求报文语法错误 
> 401 (unauthorized) 需要认证 403(Forbidden) 服务器拒绝访问对应的资源 
> 404(Not Found) 服务器上无法找到资源 

**5.4 5XX 服务器端错误**

> 500(Internal Server Error)服务器故障 
> 503(Service Unavailable) 服务器处于超负载或正在停机维护 



## 参考来源：

- [5分钟让你明白HTTP协议](https://juejin.im/post/5ad4465d6fb9a028da7d0117)
- [简述HTTP](https://juejin.im/post/5a584c146fb9a01cb508c14f)