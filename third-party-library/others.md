
1.最近公司一个程序中包含了对和服务器通信的加密和解密方法，里面引入到了zlib，但是编译的时候老是提醒以下错误：  
```
Undefined symbols for architecture armv7:
  "_compress", referenced from:
      +[ZipStr Compress:length:] in ZipStr.o
  "_inflate", referenced from:
      _ggzread in ZipStr.o
  "_inflateReset", referenced from:
      _ggzread in ZipStr.o
  "_inflateInit2_", referenced from:
      _Init in ZipStr.o
  "_crc32", referenced from:
      _ggzread in ZipStr.o
      _Init in ZipStr.o
  "_inflateEnd", referenced from:
      _destroy in ZipStr.o
ld: symbol(s) not found for architecture armv7
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

结果发现是没有导入对应的`libz.tbd`文件，导入即可。另外，现在Xcode 7中选择 `.tbd`文件替换之前的`.dylib`文件，相关参考资料：  
1.[Why Xcode 7 shows *.tbd instead of *.dylib?](http://stackoverflow.com/questions/31450690/why-xcode-7-shows-tbd-instead-of-dylib)  
2.[Xcode 7 : 浅析 .tbd 与 .dylib](http://www.meniny.cn/2015/09/22/00-00-01-iOS_Xcode_7_tbd/)  
3.[关于Xcode7中的tbd文件](http://www.cocoachina.com/ios/20160506/16141.html)



2.公司蓝牙封装库 Demo 编译的时候出现以下编译错误：

```
  "_OBJC_CLASS_$_LXBluetoothDeviceFMSenderManagerController", referenced from:

      objc-class-ref in mainViewController.o

ld: symbol(s) not found for architecture arm64

clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

以上编译错误，是有一个一个.m文件没有被引入，可能是之前注销了引用，然后在`Build Phases`的`Compile Sources`中添加相关的.m文件即可。

3.当APP处于后台的时候，如果没有一下参数的配置，蓝牙断开没有回调：

```
Required background modes

1.App communicates using CoreBluetooth
2.App shares data using CoreBluetooth
```

另外，还需要注意在`Capablities`里面的`Background Modes`里面选择：
```
1.Uses Bluetooth LE accessories
2.Acts as a Bluetooth LE accessory
```

1.同事购买了一个日版的iPhone5，有一个有趣的特性：  
手机静音的情况下，拍照依然有声音！

2.今天删除了本地的全部Xcode中使用的`.mobileprovisioning`文件，然后也删除了用于iOS的全部开发证书。然后重新创建了一个`.cer`证书文件，准备双击安装的时候，提示以下内容：  
**不能修改“System Roots”钥匙串。**  
*若要更改根证书是否会被信任，请在“钥匙串访问”中打开它，然后修改它的信任设置。新根证书应被添加到当前用户的登录钥匙串，如果它将被这台机器的所有用户共享，则应被添加到系统钥匙串。*  
解决方法：我们点击确定之后，直接在当前窗口：`钥匙串 -> 登录`，将`.cer`文件直接拖拽进来即可！


1.iOS应用的图标黑边的问题，有一种原因：就是设计师将图做了圆角处理，iOS系统本事会自动做圆角处理，由于角度不对应，就会出现酱的问题。

2.上传到App Store的应用信息中的图标，不能存在Alpha通道或者透明度。