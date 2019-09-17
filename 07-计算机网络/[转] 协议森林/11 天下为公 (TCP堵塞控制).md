在TCP协议中，我们使用连接记录TCP两端的状态，使用编号和分段实现了TCP传输的有序，使用滑窗来实现了发送方和接收方处理能力的匹配，并使用重复发送来实现TCP传输的可靠性。

有了上面的机制，我们只需要将TCP片段包装成IP包，扔到网络中就可以了。TCP协议的相关模块会帮我们处理各种可能出现的问题(比如排序，比如TCP片段丢失等等)。最初的TCP协议就是由上述的几大块构成的。

然而进入上世纪八十年代，网络变的繁忙。许多网络中出现了大量的堵塞。堵塞类似于现实中的堵车。网络被称为“信息高速公路”。许多汽车(IP包)在网络中行驶，并经过一个一个路口 (路由器)，直到到达目的地。

一个路由器如果过度繁忙，会丢弃一些IP包。UDP协议不保证传输的可靠性，所以丢失就丢失了。而TCP协议需要保证传输的可靠性，当包含有TCP片段的IP包丢失时，TCP协议会重复发送TCP片段。于是，更多的“汽车”进入到公路中，原本繁忙的路由器变得更加繁忙，更多的IP包丢失。这样就构成了一个恶性循环。

这样的情况被称为堵塞崩溃(congestion collapse)。每个发送方为了保证自己的发送质量，而不顾及公共领域现状，是造成堵塞崩溃的主要原因。当时的网络中高达90%的传输资源可能被堵塞崩溃所浪费。

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgKYzcRldqwzhzRBtLSDETXwSCArJ3zSaqhgicOv0CEXxN7dy8B4bgj9v1Zia9OLsYt505lQpcrOvELQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

为了解决这一缺陷，从八十年代开始，TCP协议中开始加入堵塞控制(congestion control)的功能，以避免堵塞崩溃的出现。多个算法被提出并实施，大大改善了网络的交通状况。直到今天，堵塞控制依然是互联网研究的一个活跃领域。

### 公德

现实中，当我们遇到堵车，可能就会希望兴建立交桥和高架，或者希望有一位交警来疏导交通。而TCP协议的堵塞控制是通过约束自己实现的。当TCP的发送方探测到网络交通拥堵时，会控制自己发送片段的速率，以缓解网络的交通状况，避免堵塞崩溃。简言之，TCP协议规定了发送方需要遵守的“公德”。

我们先来说明堵塞是如何探测的。在[TCP重新发送（请戳我）](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483899&idx=1&sn=6161ad3586512489a40383f3ad88eb25&chksm=972ad081a05d59977ebb36a2f149164f08be99f04806276463e540925116c96a78a2cab76dee&mpshare=1&scene=21&srcid=0927tsBPu1PD5vACiNaXslUy#wechat_redirect)中，我们已经总结了两种推测TCP片段丢失的方法：ACK超时和重复ACK。一旦发送方认为TCP片段丢失，则认为网络中出现堵塞。

另一方面，TCP发送方是如何控制发送速率呢？TCP协议通过控制滑窗(sliding window)大小来控制发送速率。在[TCP滑窗管理（请戳我）](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483876&idx=1&sn=0752259af9324c4ef907d086673953e6&chksm=972ad09ea05d59881d5fb086cd898b7752912b0dd99a087fd276a51f55b920ae6a959ee7a621&mpshare=1&scene=21&srcid=0927LHIEXoLwAOMwXvfk9xPP#wechat_redirect)中，我们已经见到了一个窗口限制，就是advertised window size，以实现TCP流量控制。TCP还会维护一个congestion window size，以根据网络状况来调整滑窗大小。真实滑窗大小取这两个滑窗限制的最小值，从而同时满足两个限制 (流量控制和堵塞控制)。

### Congestion Window

congestion window总是处于两种状态的一个。这两种状态是: **慢起动**(slow start)和**堵塞避免**(congestion avoidance)。

![img](http://mmbiz.qpic.cn/mmbiz_png/FWANMMXDrgKYzcRldqwzhzRBtLSDETXwZ7hflSv71epGSibkwcaBzXglXFibEeoSjcdsOGazTbBzKiaL89DalVnFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

上图是概念性的。实际的实施要比上图复杂，而且根据算法不同会有不同的版本。cwnd代表congestion window size。我们以片段的个数为单位，来表示cwnd的大小 (同样是概念性的)。

Congestion window从slow start的状态开始。Slow start的特点是初始速率低，但速率不断倍增。每次进入到slow start状态时，cwnd都需要重置为初始值1。发送方每接收到一个正确的ACK，就会将congestion window增加1，从而实现速率的倍增(由于累计ACK，速率增长可能会小于倍增)。

当congestion window的大小达到某个阈值ssthresh时，congestion进入到congestion avoidance状态。发送速率会继续增长。发送方在每个窗户所有片段成功传输后，将窗口尺寸增加1(实际上就是每个RTT增加1)。所以在congestion avoidance下，cwnd线性增长，增长速率慢。

如果在congestion avoidance下有片段丢失，重新回到slow start状态，并将ssthresh更新为cwnd的一半。

我们看到，sshthresh是slow start到congestion avoidance的切换点。而片段丢失是congestion avoidance到slow start的切换点。一开始sshthresh的值一般比较大，所以slow start可能在切换成congestion avoidance之前就丢失片段。这种情况下，slow start会重新开始，而ssthresh更新为cwnd的一半。

总的来说，发送速率总是在增长。如果片段丢失，则重置速率为1，并快速增长。增长到一定程度，则进入到慢性增长。快速增长和慢性增长的切换点(sshthred)会随着网络状况(何时出现片段丢失)更新。通过上面的机制，让发送速率处于动态平衡，不断的尝试更大值。初始时增长快，而接近饱和时增长慢。但一旦尝试过度，则迅速重置，以免造成网络负担。

### 总结

阻塞控制有效的提高了互联网的利用率。阻塞控制的算法多种多样，并且依然不完善。一个常见的问题是cwnd在接近饱和时线性增长，因此对新增的网络带宽不敏感。互联网利用“公德”来实现效率。“公德”和效率似乎可以并存。



### 本文来源：

- 公众号：[天下为公 (TCP堵塞控制)](https://mp.weixin.qq.com/s?__biz=MzIwNTc4NTEwOQ==&mid=2247483914&idx=1&sn=5904f3dd4a6f18dbe6d1ef8314aff3cf&chksm=972ad370a05d5a669ab1febee39aae51768403d19547d8a419ac8c4b76a9b268ad39efdbd28d&mpshare=1&scene=21&srcid=10075mVozZxUHEfBu4O0xroE#wechat_redirect)
- 作者原文：https://read.douban.com/reader/column/1788114/chapter/22405963/