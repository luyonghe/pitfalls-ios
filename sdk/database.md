# 数据库

### 1.iOS 数据库升级导致程序崩溃 

#### 环境参数：

```
*
```

#### 问题分析：

应用程序更新到最新版本，运行出现程序崩溃

为了使应用程序文件命名规范化，工程里的.h 和.m等文件类前缀改为LX，当应用重新启动后，程序读取残留下来的，未更改类前缀名字的数据，
与更改名字后的类有冲突，导致程序崩溃。

#### 解决方法：

由于该数据只是用作缓存，不包含有用户的信息，因此在新版本程序中，在读取数据之前，可以使用NSFileManager把旧的数据库文件删掉，重新创建新的数据库文件，即可解决崩溃问题。