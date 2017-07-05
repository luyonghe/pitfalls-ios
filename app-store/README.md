# ios-app-store-review-guidelines-tips

**************  
#### 2017-07-04
**问题原文如下：**      
Guideline 2.5.1 - Performance - Software Requirements

Your app uses or references the following non-public APIs:

"LSApplicationWorkspace, defaultWorkspace, openSensitiveURL:withOptions:, __NSArrayM, __NSCFConstantString, __NSCFString, __NSDictionaryM, LSApplicationWorkspace, NSConcreteAttributedString, NSConcreteMutableAttributedString, UIStatusBarDataNetworkItemView" 

The use of non-public APIs is not permitted on the App Store because it can lead to a poor user experience should these APIs change.

**问题描述：**    
应用中使用了以下的私有 APIs ，包括以下：

"LSApplicationWorkspace, defaultWorkspace, openSensitiveURL:withOptions:, __NSArrayM, __NSCFConstantString, __NSCFString, __NSDictionaryM, LSApplicationWorkspace, NSConcreteAttributedString, NSConcreteMutableAttributedString, UIStatusBarDataNetworkItemView" 

使用私有 APIs 是不允许上架 App Store 的，因为可能会导致非常不好的用户体验。

**解决方案**    
也许是 Apple 加强了对私有 API 的审核，之前使用字符串加密调用私有 API 的方法已不再有效。需要查找项目中使用了对应私有 API 的地方并移除掉对应代码。经查找此次被拒的私有 API 主要分布在两个功能模块，包括 iOS 10+ 跳转到到系统蓝牙界面和第三方库 AvoidCrash 中。需要移涉及相关私有 API 的模块。

注意：另外可能有部分无法在代码中查找到，如 defaultWorkspace, openSensitiveURL:withOptions: 。针对此类查找其中部分代码如 Workspace ，会发现有相关代码包含 Workspace，如此部分无实质性功能，最好也一并移除掉。


**************  
#### 2016-08-16
AppStore被驳回问题：   

问题描述：   
发件人 Apple
Design Preamble

Your app includes an update button or alerts the user to update the app. To avoid user confusion, app version updates must utilize the iOS built-in update mechanism. 

Specifically, your app has a tappable version number section.

We've attached screenshot(s) for your reference.

Next Steps

Please remove the update feature from your app. To distribute a new version of your app, upload the new app binary version into the same iTunes Connect record you created for the app's previous version. Updated versions keep the same Apple ID, iTunes Connect ID (SKU), and bundle ID as the original version, and are available free to customers who purchased a previous version. 

Resources

To create new versions of your app, please see Replacing Your App with a New Version in the iTunes Connect Developer Guide.   

被拒原因：        
   界面上的版本号显示位置可以点击，容易误导用户以为可以点击升级。   
 
解决版本：   
   将该位置设置为不可点击状态。    


#### 2016-12-01
###iOS plist配置文件中存在未使用权限字段被拒问题
* 第一次被拒：
被拒原文：
```
Hardware required


We began the review of your app but are not able to continue because we need the associated hardware to fully assess your app features.

At your earliest convenience, please send the necessary hardware/accessory to the address below.

NOTE: Please include your app name and app ID in the shipment; failure to provide this information can delay the review process.

Additionally, it may take several business days for us to receive the hardware once it has been delivered to Apple.

Once you've shipped the hardware, please reply to this message with the shipping carrier and tracking information. We will resume the review of your app upon receiving the hardware.

IMPORTANT: for non-US Developers

To avoid delays with US Customs, please provide the following information with your shipment (required for all radio-frequency devices imported in the US):

Please use FCC form 740 for details on how to provide this information.

Please make sure you also provide any required demo account information, including passwords, in the App Review Information section for your app in iTunes Connect.

To provide demo account information:

```
大致意思是需要寄硬件设备到美国配合审核。
由于之前审核都是不需要寄设备审核，经沟通后发现info.plist 中存在对后台音乐播放的声明，然后去掉了权限字段：
```
<key>NSAppleMusicUsageDescription</key>
<string>此 App 需要您的同意才能获取本地音乐</string>
```

* 第二次被拒：  
被拒原文：
```

Upon further review, we still found that your app declares support for audio in the UIBackgroundModes key in your Info.plist but did not include features that require persistent audio.

Next Steps

The audio key is intended for use by apps that provide audible content to the user while in the background, such as music player or streaming audio apps. Please revise your app to provide audible content to the user while the app is in the background or remove the "audio" setting from the UIBackgroundModes key.

Additional Information

If you have difficulty reproducing a reported issue, please try testing the workflow described in Technical Q&A QA1764: How to reproduce bugs reported against App Store submissions.

If you have code-level questions after utilizing the above resources, you may wish to consult with Apple Developer Technical Support. When the DTS engineer follows up with you, please be ready to provide:
- complete details of your rejection issue(s)
- screenshots
- steps to reproduce the issue(s)
- symbolicated crash logs - if your issue results in a crash log

```

大致意思是当前APP仍支持后台音乐。   
经查找发现，```Required background modes``` 中确实存在```App plays audio or streams audio/video using AirPlay``` 字段 删除后即可。   
####总结：
  在提交app store版本时，应确认使用的功能，一定要加上对应的字段。此外，应确保配置文件plist中没有多余的权限字段，即没有使用到的功能，应及时删除对应字段，否则app会被拒。
  
  
  
#### 2016-10-17


iOS 10 却少对隐私权限的描述被 app store 被拒
app store 上传app 时因为没有对使用功能的权限进行描述而被拒解决方法总结
拒绝时而收到的邮件内容

您好：
 
感谢您就上传 App 构建版本的问题联系 Apple Developer Program Support。

从 iOS 10 开始，访问任何应用中受保护的数据类都需要使用目的字符串（purpose string），包括所有第三方库在您的应用里使用这些受保护的数据类的用途。如果您收到有关您无法识别的数据类缺少目的字符串（purpose string）的提示，请咨询您的第三方库供应商，了解其对该受保护数据类的使用。
有关更多详细信息，请参阅以下资源：
App Programming Guide for iOS 中的 ”Supporting User Privacy“
https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/ExpectedAppBehaviors/ExpectedAppBehaviors.html#//apple_ref/doc/uid/TP40007072-CH3-SW6
Information Property List Key Reference 中的 “Cocoa Keys”
https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html
WWDC 2016: Engineering Privacy for Your Users (usage description keys discussed at 29:14)
https://developer.apple.com/videos/play/wwdc2016/709/?time=1754
希望以上信息对您有帮助。如果您需要再次联系我们，欢迎致电或通过电邮的方式跟我们联系，并提供案例编号：100045823076。我们的办公时间是北京时间周一至周五，09:00 至 17:00，电话号码是 4006-701-855。
我们很乐意给您提供帮助，感谢您参与我们的开发者计划。 
Queenie
Apple Inc. 

在工程的info.plist文件中需要添加获取用户隐私权限的key ，并且要对key 进行描述，因为没有对权限的key 进行描述，所以才会被拒，在plist文件中代码如下
<key>NSAppleMusicUsageDescription</key>
<string>此 App 需要您的同意才能获取本地音乐</string>
<key>NSBluetoothPeripheralUsageDescription</key>
<string>App需要您的同意,才能访问蓝牙</string>


这样就不会被拒了

## 15. 应用中包含其他平台信息
被拒原文：
```
We noticed that your app or its metadata includes irrelevant third-party platform information. Specifically, non-iOS device status bar is mentioned in the app.

Referencing third-party platforms in your app or its metadata is not permitted on the App Store unless there is specific interactive functionality.

We've attached screenshot(s) for your reference.

Next Steps

Please remove all instances of this information from your app and its metadata, including the app description, What's New info, previews, and screenshots.

Since your iTunes Connect status is Rejected, a new binary will be required. Make the desired metadata changes when you upload the new binary.
```

原因: iOS 应用中不能包含任何其他平台的图标、文字信息、甚至是其他平台风格样式的控件也会导致审核被拒。

下面是上面被拒的问题所在：虽然状态栏的电池、时间、运营商等没有问题，状态栏和导航栏颜色不一样，这不符合 iOS 的风格，所以导致被拒。

![image](https://github.com/Lewanny/review-guidelines-tips-app-store-ios/blob/master/pictures/StatuBarProblem.png)   


***

## 14 、plist配置文件中存在未使用权限字段被拒问题    
**************  
* 第一次被拒：
被拒原文：
```
Hardware required


We began the review of your app but are not able to continue because we need the associated hardware to fully assess your app features.

At your earliest convenience, please send the necessary hardware/accessory to the address below.

NOTE: Please include your app name and app ID in the shipment; failure to provide this information can delay the review process.

Additionally, it may take several business days for us to receive the hardware once it has been delivered to Apple.

Once you've shipped the hardware, please reply to this message with the shipping carrier and tracking information. We will resume the review of your app upon receiving the hardware.

IMPORTANT: for non-US Developers

To avoid delays with US Customs, please provide the following information with your shipment (required for all radio-frequency devices imported in the US):

Please use FCC form 740 for details on how to provide this information.

Please make sure you also provide any required demo account information, including passwords, in the App Review Information section for your app in iTunes Connect.

To provide demo account information:

```
大致意思是需要寄硬件设备到美国配合审核。
由于之前审核都是不需要寄设备审核，经沟通后发现info.plist 中存在对后台音乐播放的声明，然后去掉了权限字段：
```
<key>NSAppleMusicUsageDescription</key>
<string>此 App 需要您的同意才能获取本地音乐</string>
```

* 第二次被拒：  
被拒原文：
```

Upon further review, we still found that your app declares support for audio in the UIBackgroundModes key in your Info.plist but did not include features that require persistent audio.

Next Steps

The audio key is intended for use by apps that provide audible content to the user while in the background, such as music player or streaming audio apps. Please revise your app to provide audible content to the user while the app is in the background or remove the "audio" setting from the UIBackgroundModes key.

Additional Information

If you have difficulty reproducing a reported issue, please try testing the workflow described in Technical Q&A QA1764: How to reproduce bugs reported against App Store submissions.

If you have code-level questions after utilizing the above resources, you may wish to consult with Apple Developer Technical Support. When the DTS engineer follows up with you, please be ready to provide:
- complete details of your rejection issue(s)
- screenshots
- steps to reproduce the issue(s)
- symbolicated crash logs - if your issue results in a crash log

```

大致意思是当前APP仍支持后台音乐。   
经查找发现，```Required background modes``` 中确实存在```App plays audio or streams audio/video using AirPlay``` 字段 删除后即可。   
####总结：
  在提交app store版本时，应确认使用的功能，一定要加上对应的字段。此外，应确保配置文件plist中没有多余的权限字段，即没有使用到的功能，应及时删除对应字段，否则app会被拒。


**************  

## 13 、App Store 上传被拒问题总结之未提供登陆账号给审核人员导致后台播放无法查实被拒的问题    

**************  

#### 1、问题描述：在被拒的原因里面有如下表述  


Information Needed


We began the review of your app but aren't able to continue because we need additional information about your app.

At your earliest opportunity, please review the following question(s) and provide as much detailed information as you can. The more information you can provide upfront, the sooner we can complete your review.

1) What is the purpose of declaring Audio background mode? Please explain the need for this background mode and where the usage can be found in your binary.

Once you reply to this message in Resolution Center with the requested information, we can proceed with your review.

另外，附上了App 的登陆首页


#### 2、问题分析：   

从表述上来看似乎和背景播放有关，而之前也出现过类似因为后台播放被拒的问题，这已早有所述，查看了上传视频，确实有后台播放相关的录像。说明这和后台播放这块没关系，在仔细阅读第一条最后7个的单词和附件的图片的时候，应该是由于未提供测试账号导致的问题。

#### 2、解决方法：   
再次打包，附上测试登陆账号和密码。
************** 

## 12.Xcode8 iOS10上传AppStore构建版本无效
1.问题描述: 使用Xcode8打包上传AppStore后出现构建版本无效, 无法构建版本   
![image](https://github.com/Lewanny/review-guidelines-tips-app-store-ios/blob/master/pictures/unBuild_01.png)   
2.问题原因: iOS10更加注重用户隐私保护, 使用内容必须要在plist文件中做对应的声明   
![image](https://github.com/Lewanny/review-guidelines-tips-app-store-ios/blob/master/pictures/unBuild_02.png)   
####注意: 可能应用中没有真正使用到对应的权限, 也会出现这个问题(可能调用了API), 具体要看Apple反馈的邮件, 里面会详细的注明需要添加哪些权限
***

Tips:
1.个人App Store的审核似乎没有企业App Store账号审核严格。

2.上传app store时，提示兼容64位失败。   
  奇怪的是，虽然上传app store时失败并提示兼容64位设备失败，但是在真机调试运行时，无论是64位的iphone 6还是32位的iphone 5设备，都能正常运行，我也不知道什么原因。   
  后来修改了配置中的other linker Flags 中的参数，把 -all load ,改成 -all load -lz 却能成功上传app store。   
  虽然问题解决了，但是感觉还是瞎蒙的，上网找了下资料，有对上述参数的描述，顺便贴出来，方便大家了解。
  
  下面逐个介绍几个常用参数：
－ObjC：加了这个参数后，链接器就会把静态库中所有的Objective-C类和分类都加载到最后的可执行文件中

－all_load :会让链接器把所有找到的目标文件都加载到可执行文件中，但是千万不要随便使用这个参数！假如你使用了不止一个静态库文件，
然后又使用了这个参数，那么你很有可能会遇到ld: duplicate symbol错误，因为不同的库文件里面可能会有相同的目标文件，所以建议在遇到-ObjC失效的情况下使用-force_load参数。

-force_load：所做的事情跟-all_load其实是一样的，但是-force_load需要指定要进行全部加载的库文件的路径，这样的话，你就只是完全加载了一个库文件，不影响其余库文件的按需加载   

##### 3.PLA 3.3.12   Advertising Identifier 审核被拒解决方法 。
  分析如下：
    由于Apple修改了审核标准，苹果禁止不使用广告而采集IDFA的APP上架，IDFA只能用于广告服务。
    "You and Your Applications (and any third party with whom you have contracted to serve advertising) may us the Advertising Identifier, and any information obtained through the use of the Advertising Identifier, only for the purpose of serving advertising. If a user resets the Advertising Identifier, then You agree not to combine, correlate, link or otherwise associate, either directly or indirectly, the prior Advertising Identifier and any derived information with the reset Advertising Identifier.We also found that your app uses the Advertising Identifier but does not include ad functionality. This does not comply with the terms of the Apple Developer Program License Agreement, as required by the App Store Review Guidelines.If your app does not serve ads, please check your code - including any third-party libraries - to remove any instances of:class: ASIdentifierManager
selector: advertisingIdentifier framework: AdSupport.framework ......

报这条错误的原因如下

1 使用了第三方的库，第三方的库根据IDFA进行跟踪用户，同时APP没有加载广告。

2 使用了第三方的库，第三方的库根据IDFA进行跟踪用户，同时加载了iAD广告。

3 同时使用了iAD+ADMOB等广告

对应的解决方法：

第一种情况解决方法：
需要把和IDFA相关的代码和接口去除，因为IDFA只可以用于广告服务。

第二种情况解决方法：
iAD不使用IDFA，具体怎么实现的，iOS内部搞的，所以要解决这个问题需要把iAD换成类似Admob一类的广告服务，或者按照第一种情况来解决，就是去除第三方中IDFA相关的代码和接口。

第三种情况解决方法:
大概比较费解，明明加了Admob等广告，为啥还是给我拒绝了呢，这种情况要看广告的加载机制，一般开发者会优先加载iAD,如果没有广告源，则加载Admob（Admob是使用了IDFA），问题就出现了在这里，审核人员一般在美国，那里是有iAD的，或者现在app的状态还没有上线，iad属于测试状态，所以iAD的广告是可以获取，这样就给审核人员一个印象：app使用了IDFA（admob中），但是只是展示了iAD的广告，没有看到其他的广告服务，他们会怀疑你使用IDFA做了其他的事情，所以拒了！！！

解决方式：1.用终端命令在项目中查找那个文件中带有advertisingIdentifier、ASIdentifierManager等字样的字符串 strings LangQin/libMobClickLibrary.a | grep advertisingIdentifier ，在友盟统计中找到了带有advertisingIdentifier标识的字符串，而我们的应用没有加载任何广告，显然属于第一种情况，对应这种情况，在友盟官方提供了两套的SDK（即有无获取IDFA版的）。

解决方式:2.另外，官方还提供另外一种方法，正确填写在Appstore上填写IDFA选项。IDFA选项有四个（汉字是对这个四个选项的说明）

1.serve advertisements within the app
服务应用中的广告。如果你的应用中集成了广告的时候，你需要勾选这一项。

√2.Attribute this app installation to a previously served advertisement.
跟踪广告带来的安装。

√3.Attribute an action taken within this app to a previously served advertisement
跟踪广告带来的用户的后续行为。

√4.Limit Ad Tracking setting in iOS
这一项下的内容其实就是对你的应用使用idfa的目的做下确认，只要你选择了采集idfa，那么这一项都是需要勾选的。

如果应用没有使用广告而采集IDFA，则2，3，4项必须选上，但官方最后还有一句话“如果您仍因为采集IDFA被Appstore审核拒绝，建议您集成任意一家广告或选用友盟无IDFA版SDK”。这么做还是有可能被拒，即使没这么做过，我最后还是替换了无IDFA版的友盟SDK。



##### 4、 后台播放模式没录视屏审核被拒之解决之道-----问题盘查之比较分析法。

  问题描述：
  
  朗琴项目迭代版本提交审核报了这样的问题 “We began the review of your app but are not able to continue because we need access to a video that demonstrates your app in use on an iOS device. Specifically, since your app utilizes the background audio mode, please include the demonstration of such background mode(s) in use on an actual iOS device within your demo video (app running in the background)”.需要我们提供带有后台播放的Video再上传上去。
    
  朗琴中性版本首版本提交审核以后报了这样的问题“Multitasking Apps may only use background services for their intended purposes: VoIP, audio playback, location, task completion, local notifications, etc.Your app declares support for audio in the UIBackgroundModes key in your Info.plist but did not include features that require persistent audio.The audio key is intended for use by applications that provide audible content to the user while in the background, such as music player or streaming audio applications. Please revise your app to provide audible content to the user while the app is in the background or remove the "audio" setting from the UIBackgroundModes key.
”

  问题分析：
    
  这两个问题，虽然说法不一样，给人的映像却是一样的问题，只是暂时说不出个所以然，令我感到奇怪的是，为什么朗琴的首版本不报这个问题，非要到迭代版本了，才说要带有后台播放的视频，既然它要，那么给它就是了，对于第二个问题，通过百度、bing\stackOverFlow等等网页式搜索法也还是没找到确切的问题所在。很苦恼，“咋办”两字一直浮现在脑海里面，想过很多，一开始我还想他不就是想证明一下能后台播放吗？索性我第一次一打开应用，就让应用播歌，它播歌时如果回到后台就可以看到后台播放了，但这种方法是个程序员都知道不好，.....。后来娄哥提醒看到字面上的意思说后台播放好像还没有配置好，如果是这个原因，找其他的项目来看看，看看他们是不是和中性版本设置的一样，如果一样并且成上架了，那就不是这个问题，反之就是，如果不是，再找其它的可能，一个一个像if()elseif那样把所有的可能都考虑进去再比较排除，总有一个是对的，由于自身能力有限，暂时就只找到了后台视频的问题了，不管怎样，如果还有其它的问题再后补吧，先录下视频，发版本验证下自己的思考，最后找找之前的应用是不是都没有录后台播放的视频。如果没有录的也上传失败了，那就是这个原因。重录视屏。
    
  问题解决：
    
  把车载项目找来了看后台配置的代码，确实不一样，车载项目，在启动成功的方法里面，设置了后他播放的代码AVAudioSession *session = [AVAudioSession sharedInstance]; [session setActive:YES error:nil];[session setCategory:AVAudioSessionCategoryPlayback error:nil];而朗琴项目一个也没有，有点欣喜若狂的感觉，又找来了蜗灯项目，蜗灯项目和朗琴一样，没有在启动成功的方法里面设置，也成功了，在找了其它的几个项目，不管设置与否，都成功了。否决了这一命题，接下来就是有没有视频的事儿了，确实，问了下测试，之前的项目都有录后台播放视屏的事，但是后来的这几个视频没录，之前有几个项目也因为没有录后台播放视频，也是审核不过，被驳回了，朗琴合并版和中性版的一样，都没有录后台播放的视频，其它被驳回的APP后面录了视屏以后才成功上传。经过一番思考，还说不定就是这样的问题，貌似也只有这个可能了，录下视频。把应用传上去了再继续修改这里所写的东西。


##### 5、 含云音乐或下载功能但无相关证书被拒。   
  被拒原因描述：   
        8.6 - Apps that include the ability to download music or video content from third party sources (e.g. YouTube, SoundCloud, Vimeo, etc) without explicit authorization from those sources will be rejected 
8.6 Details 

We found that your app allows users to download music or video content without authorization from the relevant third-party sources. 

We’ve attached screenshot(s) for your reference. 

Next Steps 

Please provide documentary evidence of your rights to allow music or video content download from third-party sources. If you do not have the requested permissions, please remove the music or video download functionality from your app.   

  解决办法：   
        利用公司服务器做一个是否显示和隐藏云音乐功能的开关，在审核期间，服务器将云音乐显示开关设置为关闭状态，即移除改云音乐功能；待app store审核通过后，再打开服务器的云音乐开关，显示云音乐按钮，此时云音乐功能恢复。   
        

##### 6、苹果根证书过期、“此证书签发者无效”、企业包打不了的问题

  问题描述：
  
  打企业包的时候，意外的报了这样的错误,Xcode attempted to locate or generate matching signing assets and failed to do so becaue of the following issues
  
  Missing IOS Distribution signing identity for XXXXXX Co.,Ltd
  
   “missing ios distribution signing identity for XXX interactive marketing planning co ltd”或“wildcard APP IDS can not be used to create in house provisioning profiles please use an explicit app id”
   
   问题解决：
   通过查找stackoverFlow 得到了这样的回答
   
  1、 Download https://developer.apple.com/certificationauthority/AppleWWDRCA.cer
  
  2、Double-click to install to Keychain.
  
  3、Then in Keychain, Select View -> "Show Expired Certificates" in Keychain app.It will list all the expired certifcates.
  
  4、Delete "Apple Worldwide Developer Relations Certificate Authority certificates" from "login" tab And also delete it from "System" tab.
  
  也就是说重新下载根证书，双击打开，打开钥匙串，点击显示按钮，选中显示过期证书，从“登录”和“系统”选中删除过期的“Apple Worldwide Developer Relations Certificate Authority certificates”证书，你会发现“此证书签发者无效”变成了“此证书有效”，不出意外，打包就没问题了。
  
  附上截图
  
  双击运行下载过来的根证书。
  
  ![image](https://raw.githubusercontent.com/Arsenals/review-guidelines-tips-app-store-ios/master/pictures/screenshot3.png)
  
  从登录和系统中找到失效的根证书"Apple Worldwide Developer Relations Certificate Authority certificates"并删除
  
  ![image](https://raw.githubusercontent.com/Arsenals/review-guidelines-tips-app-store-ios/master/pictures/screenshot2.png)
  
  点击新的下载安装好的根证书你会看见
  
  ![image](https://raw.githubusercontent.com/Arsenals/review-guidelines-tips-app-store-ios/master/pictures/screenshot4.png)
  
  ![image](https://raw.githubusercontent.com/Arsenals/review-guidelines-tips-app-store-ios/master/pictures/solve.png)

 现在，不出意外的话，打包就没问题了。

##### 7.发布ad hoc 测试包客户无法安装的问题总结：
a.确认是否添加了客户的uuid,可以进入开发者中心 (https://developer.apple.com) 查看后台是否已经添加。   
b.确认客户手机是否已经安装了该应用的app store 版本或企业版本而导致安装失败的原因。   
解决办法：卸载之前的应用即可。   


***********

2.1 - Apps that crash will be rejected
2.16 - Multitasking Apps may only use background services for their intended purposes: VoIP, audio playback, location, task completion, local notifications, etc.
2.16 Details

Your app declares support for location in the UIBackgroundModes key in your Info.plist file but does not declare any features that require persistent location. Apps that declare support for location in the UIBackgroundModes key in your Info.plist file must have features that require persistent location.

Next Steps

Please revise your app to include features that require the persistent use of real-time location updates while the app is in the background. Please also add the following battery use disclaimer in your Application Description:
"Continued use of GPS running in the background can dramatically decrease battery life."

If your app does not require persistent real-time location updates, please remove the "location" setting from the UIBackgroundModes key. You may wish to use the significant-change location service or the region monitoring location service if persistent real-time location updates are not required for your app features.

Resources

For more information, please review the Starting the Significant-Change Location Service and Monitoring Shape-Based Regions. 

2.1 Details

During review, your app crashed on iPhone running iOS 9.2.1 when we tapped on the 我 tab.

This occurred when your app was used: 
- On Wi-Fi
- On cellular network

We have attached detailed crash logs to help troubleshoot this issue.

Next Steps

Please revise your app and test it on a device to ensure that it runs as expected.

Resources

For information on how to symbolicate and read a crash log, please see Tech Note TN2151 Understanding and Analyzing iPhone OS Application Crash Reports.

If you have difficulty reproducing this issue, please try testing the workflow described in Testing Workflow with Xcode's Archive feature.

If you have code-level questions after utilizing the above resources, you may wish to consult with Apple Developer Technical Support. When the DTS engineer follows up with you, please be ready to provide:
- complete details of your rejection issue(s)
- screenshots
- steps to reproduce the issue(s)
- symbolicated crash logs - if your issue results in a crash log

If you have difficulty reproducing a reported issue, please try testing the workflow described in Technical Q&A QA1764: How to reproduce bugs reported against App Store submissions.

If you have code-level questions after utilizing the above resources, you may wish to consult with Apple Developer Technical Support. When the DTS engineer follows up with you, please be ready to provide:
- complete details of your rejection issue(s)
- screenshots
- steps to reproduce the issue(s)
- symbolicated crash logs - if your issue results in a crash log


**************
通过Application Loader上传.ipa文件到App Store发现以下错误：

```
Package Summary:
1 package(s) were not uploaded because they had problems:
	/var/folders/2b/lksn_kq118n_sttybl8s25vw0000gn/T/B16954D4-43BA-49A9-BE6E-E99F431CA82A/1092025201.itmsp - Error Messages:
		ERROR ITMS-4238: "Redundant Binary Upload. There already exists a binary upload with build version '1' for train '1.2.1'" at SoftwareAssets/PreReleaseSoftwareAsset
```

说明：以上错误是没有注意version 和 build的升级命名问题。参照以下标准：http://www.ifeegoo.com/recommended-mobile-application-version-name-management-specification.html


后台音乐播放需要出示演示视频。

##### 8.发布app store 被拒(云音乐未隐藏)
被拒理由：

2.1 - Apps that crash will be rejected
8.6 - Apps that include the ability to download music or video content from third party sources (e.g. YouTube, SoundCloud, Vimeo, etc) without explicit authorization from those sources will be rejected
8.6 Details

We found that your app still allows users to download music or video content without authorization from the relevant third-party sources.

We’ve attached screenshot(s) for your reference.

Next Steps

Please provide documentary evidence of your rights to allow music or video content download from third-party sources. If you do not have the requested permissions, please remove the music or video download functionality from your app. 


2.1 Details

During review, your app crashed on iPhone running iOS 9.3.1 when we tapped on the download button. 

This occurred when your app was used: 
- On Wi-Fi
- On cellular network

We have attached detailed crash logs to help troubleshoot this issue.

Next Steps

Please revise your app and test it on a device to ensure that it runs as expected.

Resources

For information on how to symbolicate and read a crash log, please see Tech Note TN2151 Understanding and Analyzing iPhone OS Application Crash Reports.

If you have difficulty reproducing this issue, please try testing the workflow described in Testing Workflow with Xcode's Archive feature.

If you have code-level questions after utilizing the above resources, you may wish to consult with Apple Developer Technical Support. When the DTS engineer follows up with you, please be ready to provide:
- complete details of your rejection issue(s)
- screenshots
- steps to reproduce the issue(s)
- symbolicated crash logs - if your issue results in a crash log
    
   
解决办法：   在提交app store审核期间，隐藏云音乐及所有涉及第三方播放器的控件和相关操作，例如下载等功能。
	

***

## 关于 支持 IPv6 DNS64/NAT64 网络的说明

#### 支持 IPv6 DNS64/NAT64 网络的说明

由于IPv4地址池中的地址即将消耗殆尽，企业和电信提供商加快发布 IPv6 DNS64/NAT64 网络。DNS64/NAT64 网络是仅支持 IPv6 的网络，并且继续通过转化的形式提供 IPv4 内容的访问。依据你应用的不同，这种转换有不同的影响：

如果你采用高级别的网络 API，例如 `NSURLSession` 和 `CFNetwork` 框架，而且是通过域名来连接，针对于 IPv6 地址来说，你的客户端应用不需要做任何修改，如果你不是通过域名来连接，你可能需要做一定的修改。参见[在连接之前避免解析DNS名](https://developer.apple.com/library/mac/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/CommonPitfalls/CommonPitfalls.html#//apple_ref/doc/uid/TP40010220-CH4-SW20)，更多关于  `CFNetwork` 信息，请参见 CFNetwork Framework Reference 。

如果你开发的是服务端应用或者其他低级别的网络应用，你需要确保你的套接字码需要同时兼容 IPv4 和 IPv6 地址。请参考：RFC4038: Application Aspects of IPv6 Transition.

#### 什么驱动 IPv6 的采纳

主要的网络服务提供商，包括美国大多数的电信运营商，都在积极的促进和部署 IPv6。这是由于多种因素导致的。

#### app 适配IPv6 DNS64/NAT64 网络方案    

反复阅读官方和其他相关文档，关于如何支持IPV6-Only，官方提出了如下几点：    
1. Use High-Level Networking Frameworks;    
2. Don’t Use IP Address Literals;    
3. Check Source Code for IPv6 DNS64/NAT64 Incompatibilities;    
4. Use System APIs to Synthesize IPv6 Addresses;    

第一点：使用高级别API作为我们的网络请求框架;    

第二点：不是用IP地址字面量：    
针对第二点，目前使用了IP地址字面量的接口仍然可以正常访问网络，官方文档指出：In iOS 9 and OS X 10.11 and later, NSURLSession and CFNetwork automatically synthesize IPv6 addresses from IPv4 literals locally on devices operating on DNS64/NAT64 networks. However, you should still work to rid your code of IP address literals.    
所以需要将IP字面量改为域名。

第三点：检查IPv6 DNS64/NAT64 的兼容性    
构建NAT64 网络，检查APP 在此网络下是否可以正常访问，打开系统偏好设置，长按住Alt 打开共享，选择创建NAT64 网络，共享来源连接选择Thunderbolt 以太网,并用WIFI端口共享给电脑，设置下WIFI名称以及密码相关，启动互联网共享，用手机连接该网络，如果App 可以正常访问网络，说明NAT64 网络环境下App 可以正常访问网络，不需要做任何事情，反正，需要做NAT64 网络的兼容性处理（目前微信是这样的情况，无法正常访问）。

第四点：使用同步了IPv6 地址系统API；    

特别指出：[文章：](http://www.jianshu.com/p/a6bab07c4062)讨论了Reachability是否需要修改支持IPV6的问题,特别指出，Reachability不需要做任何修改，在iOS9上就可以支持IPV6和IPV4，但是在iOS9以下会存在bug ，日后需要引起注意。    


***

## 在TARGETS －》Capabilities －》Background Modes 中选中了Voice over IP，开启了VOIP服务，而app没有相关的业务app 审核被拒

#### 提交app store被拒说明
2.16 - Multitasking Apps may only use background services for their intended purposes: VoIP, audio playback, location, task completion, local notifications, etc.
2.16 Details

Your app declares support for VoIP in the UIBackgroundModes key in your Info.plist, but does not include any Voice over IP services.

Next Steps

Please revise your app to either add VoIP features or remove the "voip" setting from the UIBackgroundModes key.

We recognize that VoIP can provide "keep alive" functionality that is useful for many app features. However, using VoIP in this manner is not the intended purpose of VoIP.

Resources

If you have difficulty reproducing a reported issue, please try testing the workflow described in Technical Q&A QA1764: How to reproduce bugs reported against App Store submissions.

If you have code-level questions after utilizing the above resources, you may wish to consult with Apple Developer Technical Support. When the DTS engineer follows up with you, please be ready to provide:
- complete details of your rejection issue(s)
- screenshots
- steps to reproduce the issue(s)
- symbolicated crash logs - if your issue results in a crash log


#### 解决方法 ：在TARGETS －》Capabilities －》Background Modes 中把选中选中的Voice over IP 的勾选去掉

## 11.国际化获取当前语言不标准，导致闪退被拒

问题说明：	
iOS9的国际化语言和iOS8的国际化语言有一定的差异。比如中国简体在中国地区在iOS8显示为zh-hans，在iOS9显示为zh-hans-CN。在iOS9上有了地区的概念，选择不同的地区会有不同的后缀，直接用获取的语言去适配国际化的话，是一个非常繁琐且工作量非常大的工作。所以，国际化的时候要去掉地区的概念，来显示获取当前语言（这里获取的语言是指和plist里匹配的语言且用plist里没有地区概念的语言）的问题或者图片。处理方式就是放入一个plist文件，内容为没有地区后缀的要适配的语言。

问题原因：	
这个问题原因是因为每个地区都会有后缀，我的处理方式是减去地区的标识，按照这个逻辑是没有问题的。但有特例（根据返回的日志得出的答案，目前测试不到这个特例），就是在iOS9上出现了没有地区后缀的标识。这样减去后缀就会出现异常。

解决方法：
处理方式修改为把获取的语言跟plist的语言做一个对比，如果包含plist的国际化的语言就显示plist里面的语言（不能用获取的语言，因为有地区概念，如果不这样处理的话，就要对应的每个地区都要处理），否则就显示英文。




