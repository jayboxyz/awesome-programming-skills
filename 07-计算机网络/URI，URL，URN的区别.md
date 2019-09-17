

``` html
URI：Uniform Resource Identifier，统一资源标识符
URL：Uniform Resource Locator，统一资源定位符
URN：Uniform Resource Name，统一资源名称
```

![](http://ww4.sinaimg.cn/mw690/6941baebgw1evu0o8swewj20go0avq3e.jpg)

URI 是统一资源标识符，用来**唯一的标识一个资源**。而 URL 是统一资源定位器，它是一种具体的 URI，即 URL 可以用来标识一个资源，而且还指明了如何 locate 这个资源。而 URN，统一资源名称，是通过名字来标识资源，比如 `mailto:java-net@java.sun.com`，也就是说，URI 是以一种**抽象的、高层次概念定义统一资源标识**，而 URL 和 URN 则是**具体的资源标识的方式**。

让 URI 能成为 URL 的当然就是那个“访问机制”、“网络位置” ` http://` or `ftp://`。URN 是唯一标识的一部分，就是一个特殊的名字。

**组成**

1. URI

   Web 上可用的每种资源如 HTML 文档、图像、视频片段、程序等都是一个来URI来定位的；URI一般由三部组成：

   ``` html
   ①访问资源的命名机制
   ②存放资源的主机名
   ③资源自身的名称，由路径表示，着重强调于资源。
   ```

2. URL

   URL 是 Internet 上用来描述信息资源的字符串，主要用在各种 www 客户程序和服务器程序上。采用 URL 可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。URL一般由三部组成：

   ``` html
   ①协议(或称为服务方式)
   ②存有该资源的主机IP地址(有时也包括端口号)
   ③主机资源的具体地址。如目录和文件名等
   ```

参考：

- [URI、URL和URN的区别](https://segmentfault.com/a/1190000006081973)
- [URI和URL的区别](https://www.jianshu.com/p/ba15d066f777)

