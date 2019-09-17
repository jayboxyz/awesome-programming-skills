## 一、爬虫基础及实战

### 1.1 环境配置

- Python3+Pip环境安装

  > 进行爬虫工作，如果只是单纯安装 Python 还好，到后面要安装一些解析库等等就比较麻烦，推荐安装 Anaconda 来管理你的安装环境和各种工具包。Anaconda 是专注于数据分析的 Python 发行版本，包含了 conda、Python 等190多个科学包及其依赖项。

- MongoDB 安装：非关系数据库，key-value 存储。再安装可视化界面软件，安装 [Robomongo](https://robomongo.org/download) ，查看 MongoDB 数据库内容。详解介绍：[MongoDB简介](http://www.runoob.com/mongodb/mongodb-intro.html)

  > MongoDB 是由 C++ 语言编写的，是一个基于分布式文件存储的开源数据库系统。
  >
  > 在高负载的情况下，添加更多的节点，可以保证服务器性能。
  >
  > MongoDB 旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。
  >
  > MongoDB 将数据存储为一个文档，数据结构由键值（key=>value）对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。

- Redis 安装：非关系数据库，key-value 存储。再安装可视化界面软件，安装 redis-desktop，查看 Redis 数据库内容。详细介绍：[Redis简介](http://www.runoob.com/redis/redis-intro.html)

- MySQL 安装：关系型数据库。

- 爬虫常用库的安装：

  - 请求库：Python内置库 urllib、第三方 requests 库、selenium 库（支持浏览器驱动，自动化测试）
  - 解析库：Python 内置库 re、lxml、beautifulsoup（依赖于 lxml 库，请先安装 lxml 库）、pyquery、
  - 存储库&其他：pymysql、pymongo、redis、flask、django、jupyter 

Requests 的安装：

> 由于 Requests 属于第三方库，也就是 Python 默认不会自带这个库，需要我们手动去安装。在命令行下输入 `pip3 install requests` 即可安装。

Selenium 的安装：

> Selenium 是什么？一句话，自动化测试工具。它支持各种浏览器，包括 Chrome，Safari，Firefox 等主流界面式浏览器，如果你在这些浏览器里面安装一个 Selenium 的插件，那么便可以方便地实现 Web 界面的测试。换句话说叫 Selenium 支持这些浏览器驱动。
>
> 我们在请求网页的时候，很多网页可能是 JS 渲染后的数据，我们怎么获取这些渲染后的数据呢，可以使用 Selenium 可以得到渲染后页面。

ChromeDriver 的安装：

> 安装好了 Selenium 库，但是它是一个自动化测试工具，需要浏览器来配合它使用。
>
> 我们需要安装一个 ChromeDriver 才能驱动 Chrome 浏览器完成相应的操作，在这介绍下 Chrome 浏览器及 ChromeDriver 驱动的配置，先下载 Chrome 浏览器，然后需要安装一个 ChromeDriver 才能驱动。ChromeDriver 参考网上教程，这里就不赘述了。如果想要实现 Selenium 驱动 Firefox 浏览器需要 GeckoDriver 的安装。

PhantomJS 的安装：

> 如果我们使用 Chrome 或 Firefox 进行网页抓取的话，每次抓取的时候，都会弹出一个浏览器，比较影响使用。这里介绍一个无界面浏览器，叫做 PhantomJS。
>
> PhantomJS 是一个无界面的，可脚本编程的 WebKit 浏览器引擎。它原生支持多种 web 标准：DOM 操作，CSS 选择器，JSON，Canvas 以及 SVG。
>
> Selenium 支持 PhantomJS，这样在运行的时候就不会再弹出一个浏览器了，而且其运行效率也是很高的，还支持各种参数配置，使用非常方便，下面我们就来了解一下 PhantomJS 的安装过程。

Aiohttp 的安装：

> 之前介绍的 Requests 库是一个阻塞式 HTTP 请求库，当我们发出一个请求后，程序会一直等待服务器的响应，直到得到响应后程序才会进行下一步的处理，其实这个过程是比较耗费资源的。如果程序可以在这个等待过程中做一些其他的事情，如进行请求的调度、响应的处理等等，那么爬取效率一定会大大提高。

LXML 的安装：

> LXML 是 Python 的一个解析库，支持 HTML 和 XML 的解析，支持 XPath 解析方式，而且解析效率非常高。

BeautifulSoup 的安装：

> BeautifulSoup 是 Python 的一个 HTML 或 XML 的解析库，我们可以用它来方便地从网页中提取数据，它拥有强大的 API 和多样的解析方式。
>
> BeautifulSoup 的 HTML 和 XML 解析器是依赖于 LXML 库的，所以在此之前请确保已经成功安装好了 LXML 库。

PyQuery 的安装：

> PyQuery 同样是一个强大的网页解析工具，它提供了和 jQuery 类似的语法来解析 HTML 文档，支持 CSS 选择器，使用非常方便。

PyMySQL 的安装：

> 在 Python3 中如果想要将数据存储到 MySQL 中就需要借助于 PyMySQL 来操作。Pip 安装方式：`pip3 install pymysql`

PyMongo 的安装：

> 在 Python 中如果想要和 MongoDB 进行交互就需要借助于 PyMongo 库。Pip 安装方式：`pip3 install pymongo` 

RedisPy 的安装：

> 对于 Redis 来说，我们要使用 RedisPy 库来与其交互。Pip 安装方式：`pip3 install redis`  

Flask 的安装：

> Flask 是一个轻量级的 Web 服务程序，简单、易用、灵活，在本书中我们主要用它来做一些 API 服务。Pip 安装方式：`pip3 install flask`

Django 的安装：

> Django 是一个 Web 服务框架，提供了完整的后台管理，提供了一些模板引擎、接口、路由啊。Pip 安装方式：`pip3 install django`

Jupyter 的安装：

> 相当于一个 notebook 记事本，但是这个记事本功能是比较强大的，它是运行在网页端，然后你可以在这个记事本里写代码，进行调试，也可以进行在线运行。Pip 安装方式：`pip3 install jupyter`  它的依赖库还是比较多的，安装过程需要耐心等待下。



如果一个一个安装嫌麻烦，可以执行下面这条命令直接安装所需库：`pip3 install requests selenium beautifulsoup4 pymysql pymongo redis flask django jupyter`

### 1.2 爬虫基础

什么是爬虫：**请求**网站并**提取**数据的**自动化**程序。

爬虫基本流程：

1. 发起请求：通过 HTTP 库向目标站点发起请求，即发送一个 Request，请求可以包含额外的 headers 等信息，等待服务器响应。
2. 获取响应内容：如果服务器能正常响应，会得到一个 Response，Response 的内容便是所要获取的页面内容，类型可能是 HTML、Json 字符串，二进制数据（如图片、视频）等类型。
3. 解析内容：得到的内容可能是 HTML，可以用正则表达式、网页解析库进行解析，可能是 Json，可以直接转为 Json 对象解析，可能是二进制数据，可以作为保存或者进一步处理。
4. 保存数据：保存形式多样，可以存为文本，也可以保存至数据库，或者保存特定格式的文件。

Request 包含的内容：

- 请求方式：主要有 GET、POST 两种类型，另外还有 HEAD、PUT、DELETE、OPTIONS 等。
- 请求 URL：URL 全称统一资源定位符，如一个网页文档、一张图片、一个视频等都可以用 URL 唯一确定。
- 请求头：包含请求时的头部信息，如 User-Agent、Host、Cookies 等信息。
- 请求体：请求时额外携带的数据，如表单提交时的表单数据。

Response 包含的内容：

- 响应状态：有多种响应状态，如 200 代表成功、301 代表跳转、404 找不到页面、502 服务器错误。
- 响应头：如内容类型、内容长度、服务器信息、设置 Cookie 等等。
- 响应体：最主要的部分，包含了请求资源的内容，如网页 HTML、图片二进制数据等。

①能抓怎样的数据？

- 网页文本：如 HTML 文档、Json 格式文本等
- 图片：获取到的是二进制文件，保存为图片格式
- 视频：同为二进制文件，保存为视频格式即可
- 其他：只要是能请求到的，都能获取

②怎样解析？--> 解析方式：

- 直接解析
- Json 解析
- 正则表达式
- BeautifulSoup
- PyQuery
- XPath

③为什么我抓到的和浏览器看到的不一样？--> 因为有些数据是 JS 调用的后台接口再加载出来的。

怎样解决 JavaScript 渲染的问题？

- 分析 Ajax 请求
- Selenium / WebDriver 来驱动浏览器
- Splash ： 也是和 Selenium 一样，也是来模拟 JS 渲染的。
- PyV8、Ghost.py

④怎样保存数据？

- 文本：纯文本、Json、XML 等
- 关系型数据库：如 MySQL、Oracle、SQL Server 等具有结构化形式存储
- 非关系型数据库：如 MongoDB、Redis 等 key-value 形式存储
- 二进制文件：如图片、视频、音频等等直接保存成特定格式即可




## 二、爬虫框架及分布式爬取

### 2.1 PySpider框架

> PySpider 是国人 binux 编写的强大的网络爬虫框架，它带有强大的 WebUI、脚本编辑器、任务监控器、项目管理器以及结果处理器，同时它支持多种数据库后端、多种消息队列，另外它还支持 JavaScript 渲染页面的爬取，使用起来非常方便，本节介绍一下它的安装过程。
>
> PySpider 是支持 JavaScript 渲染的，而这个过程是依赖于 PhantomJS 的，所以还需要安装 PhantomJS，所以在安装之前请安装好 PhantomJS。Pip 安装：`pip3 install pyspider` 命令执行完毕即可完成安装。安装完成之后，可以直接在命令行下启动 PySpider：`pyspider all`  正常启动后，这时 PySpider 的 Web 服务就会在本地 5000 端口运行，直接在浏览器打开：<http://localhost:5000/> 即可进入 PySpider 的 WebUI 管理页面。

### 2.2 Scrapy框架

> Scrapy 是一个十分强大的爬虫框架，依赖的库比较多，至少需要依赖库有 Twisted 14.0，lxml 3.4，pyOpenSSL 0.14。而在不同平台环境又各不相同，所以在安装之前最好确保把一些基本库安装好，尤其是 Windows。安装方式两种，① Anaconda 下的安装 ② Windows下的安装，具体的安装教程参考：[Scrapy的安装](https://germey.gitbooks.io/python3webspider/content/1.8.2-Scrapy%E7%9A%84%E5%AE%89%E8%A3%85.html) 

- Windows 下直接安装：
  1. wheel：`pip install wheel`
  2. lxml：①使用 Pip 安装 `pip3 install lxml` ②也可以采用 Wheel 方式安装，推荐直接到这里下载对应的 Wheel 文件，链接为 <http://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml>，找到本地安装 Python 版本和系统对应的 LXML 版本，再利用 Pip 安装即可
  3. PyOpenssl：<https://pypi.python.org/pypi/pyOpenSSL#downloads>  下载 Wheel 文件利用 Pip 安装 即可
  4. Twisted：到 <http://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted> 下载 Wheel 文件，利用 Pip 安装即可
  5. Pywin32：从官方下载对应版本的安装包<https://sourceforge.net/projects/pywin32/files/pywin32/Build%20221/>， 下载完毕之后双击安装即可
  6. Scrapy：前面是一些依赖库的安装，完毕之后，再使用 Pip 安装 Scrap 框架 `pip3 install Scrapy`  


PS：关于网络爬虫，除了上面还有很多需要学习，如部署分布式方式抓取数据、如何应对网站的反爬虫策略（如使用代理处理反爬）等等。

---

*学习于： [崔庆才_Python3.6网络爬虫实战视频教程](https://www.bilibili.com/video/av19057145/?p=2 ) | [崔庆才《Python3网络爬虫开发实战》](https://germey.gitbooks.io/python3webspider/content/)* 

