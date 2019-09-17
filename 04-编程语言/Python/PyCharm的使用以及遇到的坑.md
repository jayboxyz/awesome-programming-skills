## 一、PyCharm 的使用

**Python 的使用：**

- [集成开发环境Pycharm的使用方法和技巧](https://www.cnblogs.com/anliven/p/6038449.html)
- [pycharm快捷键及一些常用设置](http://blog.csdn.net/fighter_yy/article/details/40860949)
- [配置PyCharm(背景色+字体大小+解释器选择)](http://blog.csdn.net/vernice/article/details/50934869)



**PyCharm 的调试：**

- [最全Pycharm教程（10）——Pycharm调试器总篇](https://blog.csdn.net/u013088062/article/details/50214459)

pycharm-代码调试快捷键：

``` xml
F8：单步
F7：单步，进函数里面（无函数时同F8）
Shift+F8：单步跳出
Alt+F9：运行到光标所在位置处
Alt+F8：测试语句
F9：重新运行程序（实际是到下个断点）
Ctrl+F8：切换断点
Ctrl+F8：查看断点
```

**一些使用技巧：**

- [一招解决Pycharm里安装包慢的问题，助你飞快10倍](https://mp.weixin.qq.com/s/L1C_2Tcbc8zddVsHPm3rjw)

  > PyCharm 里封装了一个私有的环境包 venv，这样每个项目都有自己的环境成为了可能。不过，我们都可能感觉到 PyCharm 里安装包有些慢，有时实在太慢。
  >
  > 可以试试该方法：
  >
  > 1）找到 Pycharm安装目录，默认为：`C:\Program Files\JetBrains\PyCharm Community Edition 2017.3.3\helpers`
  >
  > 2）只需要：里面添加个文件夹`pip`，然后再该文件夹下新建一个`pip.ini`文件。



## 二、记录遇到 PyCharm 的坑

**坑 1：**

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-7-13-26181152.jpg)

可以看到如上导入模块在 PyCharm 下竟然提示有错，尝试在本地 cmd 下直接 `Python test2.py` 可以正常运行，并且在 PyCharm IDE 下直接点击 run 也是可以正常运行。

但出现这么个波浪线提示有错，到底咋地回事？先说下怎么解决的，如下把该目录设置为`Sources Root`即可：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-7-13-64586916.jpg)

延伸：了解下 Python 搜索模块的路径的问题。

Python搜索模块的路径：

1. 程序的主目录
2. PTYHONPATH目录（如果已经进行了设置）
3. 标准连接库目录（一般在/usr/local/lib/python2.X/）
4. 任何的`.pth`文件的内容（如果存在的话）。新功能，允许用户把有效果的目录添加到模块搜索路径中去。`.pth`后缀的文本文件中一行一行的地列出目录。

在没把目录 `Make Directory as Sources Root` ，可以看到`print(sys.patyh);`只打印了如下路径：

``` xml
'D:\\python-practice\\exer02\\a', 
'D:\\python-practice\\exer02', 
'D:\\devInstall\\Anaconda3\\envs\\zb\\python36.zip', 'D:\\devInstall\\Anaconda3\\envs\\zb\\DLLs', 
'D:\\devInstall\\Anaconda3\\envs\\zb\\lib', 
'D:\\devInstall\\Anaconda3\\envs\\zb', 
'D:\\devInstall\\Anaconda3\\envs\\zb\\lib\\site-packages'
```

把 b 目录设置为 `...as Source Root` ，可以看到 `print(sys.path);` 打印了如下路径：（多打印了 b 目录）

``` xml
'D:\\python-practice\\exer02\\a', 
'D:\\python-practice\\exer02', 
'D:\\python-practice\\exer02\\b', 
'D:\\devInstall\\Anaconda3\\envs\\zb\\python36.zip', 'D:\\devInstall\\Anaconda3\\envs\\zb\\DLLs', 
'D:\\devInstall\\Anaconda3\\envs\\zb\\lib', 
'D:\\devInstall\\Anaconda3\\envs\\zb', 
'D:\\devInstall\\Anaconda3\\envs\\zb\\lib\\site-packages'
```

另外发现：

设置电脑的用户变量 PYTHONPATH 如下：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-7-13-4904073.jpg)

并在 PyCharm 中未把 b 目录设置为 `...as Source Root`，可以看到如下结果：

![](https://img-1256179949.cos.ap-shanghai.myqcloud.com/18-7-13-57136098.jpg)

是可以的。（但在 PyCharm IDE 中： `from test3 import fun3` 是提示有错的。可见在 PyCharm 下，一定要 `Make Directory as Sources Root`  它才肯认。）