我们已经讲解了物理层、链路层和网络层。链路层协议种类繁多(Ethernet、Wifi、ARP等等)。到了**网络层**，只有一个 **IP协议**。进入到传输层(transport layer)，协议的种类又多起来(比如 TCP、UDP、SCTP 等)。这就好像下面的大树，根部(链路层)分叉很多，然后统一到一个树干上(网络层)，到了树冠(传输层)部分又开始开始分叉，而每个树枝上长出更多的树叶(应用层)。

![](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgLfdvGj6PaiaQvAX5DPHorqGegicl1nnZsJlicMBO2mHIaW35WM1Zlf7f4tCx57dsUTDZY8oHJj4iaCow/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

​									`网络分层的“艺术”观点`

传输层最重要的协议为 TCP 协议和 UDP 协议。这两者走了两个极端，TCP 协议复杂，但传输可靠。UDP 协议简单，但传输不可靠。其他的各个传输层协议在某种程度上都是这两个协议的折中。我们先来看传输层协议中比较简单的 UDP 协议。我们将参考许多之前文章的内容。

### UDP协议简介

UDP(User Datagram Protocol)传输与IP传输非常类似。你可以将 UDP 协议看作 IP 协议暴露在传输层的一个接口。UDP 协议同样以数据包(datagram)的方式传输，它的传输方式**也是"Best Effort"的**，所以 UDP 协议也是不可靠的(unreliable)。那么，我们为什么不直接使用 IP 协议而要额外增加一个 UDP 协议呢？  

一个重要的**原因是 IP 协议中并没有端口(port)**的概念。IP 协议进行的是 IP 地址到 IP 地址的传输，这意味者两台计算机之间的对话。但每台计算机中需要有多个进程同时运行，当前到底和哪一个进程通信了？**端口**就是用来区分这个的。一个端口就代表了一个进程。UDP 协议实现了端口，从而让数据包可以在送到 IP 地址的基础上，进一步可以送到某个端口。

尽管 UDP 协议非常简单，但它的产生**晚于更加复杂的 TCP 协议**。早期的网络开发者开发出的 IP 协议和 TCP 协议分别位于网络层和传输层，所有的通信都要先经过 TCP 封装，再经过 IP 封装(应用层->TCP->IP)。开发者将 TCP/IP 视为相互合作的套装。

但很快，网络开发者发现，IP协议的功能和TCP协议的功能是相互独立的。对于一些简单的通信，我们只需要“Best Effort”式的IP传输就可以了，而不需要TCP协议复杂的建立连接的方式(特别是在早期网络环境中，如果过多的建立TCP连接，会造成很大的网络负担，而UDP协议可以相对快速的处理这些简单通信)。UDP协议随之被开发出来，作为IP协议在传输层的"傀儡"。

这样，网络通信可以通过**应用层->UDP->IP**的封装方式，绕过 TCP 协议。由于 UDP 协议本身异常简单，实际上只为 IP 传输起到了桥梁的作用。我们将在 TCP 协议的讲解中看到更多 TCP 协议和 UDP 协议的对比。

### UDP协议头部

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgLfdvGj6PaiaQvAX5DPHorqG7luNSIdE2oeDNTXZmQIg1I8zBVjX4ia9tSPia4mjY40ZBsbv6SQNKq5A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

上面的 `source port` 和 `destination port` 分别为 UDP 包的出发端口和目的地端口。`Length` 为整个 UDP 包的长度。 

checksum的算法与[IP协议的header checksum算法（戳我）](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483832&idx=1&sn=284e7411601b53c0c90f2c0b2907f272&chksm=972ad0c2a05d59d428e5da16f7f11c2c369349bd2eb58793bb64d5dca4352922aa042639ecc8&mpshare=1&scene=21&srcid=0914XjO4nIEYeylQwKCiu9Uo#wechat_redirect)相类似。不同的是，UDP的checksum所校验的序列包括了整个UDP数据包，以及封装的IP头部的一些信息(主要为出发地IP和目的地IP)。这样，checksum就可以校验IP：端口的正确性了。在IPv4中，checksum可以为0，意味着不使用checksum。IPv6要求必须进行checksum校验。

### 端口与socket

端口(port)是伴随着传输层诞生的概念。它可以将网络层的IP通信分送到各个通信通道。UDP 协议和 TCP 协议尽管在工作方式上有很大的不同，但它们**都建立了从一个端口到另一个端口的通信**。

![img](http://mmbiz.qpic.cn/mmbiz_jpg/FWANMMXDrgLfdvGj6PaiaQvAX5DPHorqGO7NxXVMspDwh5nXCFNLZv9krxWicWT5fl8FjiaY14GY6cFPdlgntAElw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)

随着我们进入传输层，我们也可以**调用操作系统中的API，来构建socket**。Socket是操作系统提供的一个编程接口，它用来代表某个网络通信。应用程序通过socket来调用系统内核中处理网络协议的模块，而这些内核模块会负责具体的网络协议的实施。

这样，我们可以让内核来接收网络协议的细节，而我们只需要提供所要传输的内容就可以了，**内核会帮我们控制格式**，并进一步向底层封装。因此，在实际应用中，我们并不需要知道具体怎么构成一个UDP包，而只需要提供相关信息(比如IP地址，比如端口号，比如所要传输的信息)，操作系统内核会在传输之前会根据我们提供的相关信息构成一个合格的UDP包(以及下层的包和帧)。

### 总结

端口是传输层带来的最重要的概念。我们进一步了解了 UDP 协议。如果已经掌握了 IP 协议，那么 UDP 协议就没有任何困难可言，它只是 IP 协议暴露在传输层上的接口。



### 本来来源：

- 公众号：[傀儡 (UDP协议)](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483838&idx=1&sn=f2cdf52b834bacb635e5abf78a5aabe6&chksm=972ad0c4a05d59d21248f90f195e74f39e1fcee83c19bd3dc7c40b9fa2d108d33e95c22825da&mpshare=1&srcid=1007CjqfQS2tcE8pLj8cmIms&scene=21#wechat_redirect)
- 作者原文：https://read.douban.com/reader/column/1788114/chapter/22120452/

