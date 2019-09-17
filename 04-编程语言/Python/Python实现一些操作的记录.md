

**（1）关于创建目录（文件夹）**

Python 对文件操作主要会用到 os 模块，主要涉及到三个函数：

1. `os.path.exists(path)` 判断一个目录是否存在
2. `os.makedirs(path)` 创建多级目录
3. `os.mkdir(path)` 创建单级目录

区别：`os.mkdir(path)`与`os.makedirs(path)`的区别是，当父目录不存在的时候`os.mkdir(path)`不会创建，`os.makedirs(path)`则会创建父目录。

参考：[python创建目录（文件夹）](https://blog.csdn.net/MuWinter/article/details/77215768)

递归创建目录：`os.makedirs()`   参考：[Python os.makedirs() 方法](http://www.runoob.com/python/os-makedirs.html)

os.makedirs() 方法用于递归创建目录。像 mkdir()，但创建的所有 intermediate-level 文件夹需要包含子目录。

makedirs() 方法语法格式如下：`os.makedirs(path, mode=0o777)`

实例：

``` python
path = "/tmp/home/monthly/daily"
os.makedirs( path, 0755 );
```

