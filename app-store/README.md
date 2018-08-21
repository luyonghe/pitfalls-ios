# App Store 审核


### 1.Potential Loss of Keychain Access - The previous version of software has an application-identifier value of [‘xxxxxxxxxxxxx’] and the new version of software being submitted has an application-identifier of [‘xxxxxxxxxxxx’]. This will result in a loss of keychain access.

#### 环境参数：

[参数描述]

#### 问题分析：

提交到 App Store 选择了自动匹配证书打包上传选项，一般情况下选择自动匹配证书是没有问题，但是由于该应用之前是从一个账号转移到另一个账号，选择自动匹配证书有可能会在老的账号上自动创建 App 信息，导致上传时会发出以上警告。

#### 解决方法：

登录老账号，把该 App 信息从老账号中删掉。

### 2.Missing Info.plist key - This app attempts to access privacy-sensitive data without a usage description. The app's Info.plist must contain an NSPhotoLibraryUsageDescription key with a string value explaining to the user how the app uses this data.


#### 环境参数：

```
iTunes Connect @ App Store
```

#### 问题分析：

提交到 iTunes connect 后台不成功提醒：
***
We have discovered one or more issues with your recent delivery for "XXXX". To process your delivery, the following issues must be corrected:

Missing Info.plist key - This app attempts to access privacy-sensitive data without a usage description. The app's Info.plist must contain an NSPhotoLibraryUsageDescription key with a string value explaining to the user how the app uses this data.

Once these issues have been corrected, you can then redeliver the corrected binary.

Regards,

The App Store team
***

使用了相册的使用权限声明，缺少相册的使用权限的相关描述，会被拒绝提交。

#### 解决方法：

添加相册的使用权限和相关描述，问题可得到解决。

备注：

另外一个隐私权限，Privacy - Photo Library Addtions Usage Description 和相册使用权限Privacy - Photo Library Usage Description 长得很样，在输入 Photo 词语的时候往往出来的是第一个，容易填错，填错导致的结果和没填导致的结果是一样的。

如果程序中还使用了其他的权限，只是该功能被隐藏了，不易发现，这时候也需要确保把其他隐藏的使用权限也申明。

### 3.Guideline 5.1.1 - Legal - Privacy - Data Collection and Storage - We noticed that your app requests the user’s consent to access their Apple Music, music and video activity, media library but does not clarify the use of this feature in the permission modal alert.Please see attached screenshots for details.￼

#### 环境参数：

```
App Store Review
```

#### 问题分析：

应用需要访问用户的媒体库资料的权限。虽然已经做了权限申请的提示框，但却没有给用户明确的说明，这个权限会用在什么地方。

#### 解决方法：

把原来的「此 App 需要访问你的媒体资料库」

改成：

「此 App 需要访问您的媒体资料库，用以播放音乐」

### 4.Guideline 5.1.2 - Legal - Privacy - Data Use and Sharing - Your app accesses user data from the device but does not have the required precautions in place. - To collect personal data with your app, you must make it clear to the user that their personal data will be uploaded to your server and you must obtain the user's consent before the data is uploaded. You must also have a Privacy Policy URL and ensure that the URL you provide directs users to your privacy policy.


#### 环境参数：

```
App Store Review
```

#### 问题分析：

应用需要访问用户的通讯录的权限。虽然已经做出提示框，给了详细的权限使用说明，但苹果要求增加用户隐私协议界面，这样保证用户的私有信息不会上传到服务器或者被用作商用。

#### 解决方法：

放置一个 HTML 文本在服务器上或者本地利用 UIWebView 或者 WKWebView 加载。
在使用到通讯录权限时弹出协议，同时在软件内部帮助或者更多界面里增加可点击访问的连接入口。


### 5.Missing Info.plist key - This app attempts to access privacy-sensitive data without a usage description. The app's Info.plist must contain an NSContactsUsageDescription key with a string value explaining to the user how the app uses this data.

#### 环境参数：

```
iTunes Connect @ App Store
```

#### 问题分析：

用到了联系人权限，而没有详细描述和声明使用场景，有时候代码中没有用到这些权限，没调用这些方法，只是第三方应用在库里声明了，然后开发人员还未察觉。

#### 解决方法：

每次使用第三方库的时候，观察第三方库的依赖库是否包含了使用了隐私权限的库，观察第三方库是否包含一些调用联系人，照片，地理位置等等需要用到权限的方法。


### 6.We began the review of your app but still are not able to continue because we need access to a video that demonstrates your app in use on an iOS device. - Specifically, please provide a demo video to demonstrate the interactive features associating with the hardware. - To provide a link to a demo video: - Log in to iTunes Connect - Click on “My Apps” - Select your app - Scroll down to “App Review Information” - Provide demo video access details in the “Notes” section - Click Save - Click Submit for Review - Once this information is available, we can continue with the review of your app.

#### 环境参数：

```
App Store Review
```

#### 问题分析：

由于我们的应用使用到实体硬件设备，所以需要苹果需要我们拍个视频演示如何使用我们的应用。

#### 解决方法：

上传一个视频到开放的视频网站，把链接放在 App 提交审核的页面备注里。
PS.视频播放最好不要有广告，不然苹果会提示「are not able to access the provided demo」,并需要你再次提交审核。

### 7.This type of app has been identified as one that may violate one or more of the following App Store Review Guidelines. Specifically, these types of apps often: - 1.1.6 - Include false information, features, or misleading metadata. - 2.3.0 - Undergo significant concept changes after approval - 2.3.1 - Have hidden or undocumented features, including hidden "switches" that redirect to a gambling or lottery website - 3.1.1 - Use payment mechanisms other than in-app purchase to unlock features or functionality in the app - 4.3.0 - Are a duplicate of another app or are conspicuously similar to another app - 5.2.1 - Were not submitted by the legal entity that owns and is responsible for offering any services provided by the app - 5.3.4 - Do not have the necessary licensing and permissions for all the locations where the app is used - Before we can continue with our review, please confirm that this app does not violate any of the above guidelines. You may reply to this message in Resolution Center or the App Review Information section in App Store Connect to verify this app’s compliance.  - Given the tendency for apps of this type to violate the aforementioned guidelines, this review will take additional time. If at any time we discover that this app is in violation of these guidelines, the app will be rejected and removed from the App Store, and it may result in the termination of your Apple Developer Program account. - Should you choose to resubmit this app without confirming this app’s compliance, the next submission of this app will still require a longer review time. Additionally, this app will not be eligible for an expedited review until we have received your confirmation.



#### 环境参数：

```
App Store Review
```

#### 问题分析：

一般机审会出现此类的打回重审问题。在他提到的几项问题中排除原因，如果没有就可以不用做任何处理再次提交审核，如果存在某项问题，则手动处理，再次排除。我们的问题在于账号底下存在功能类似，但 UI 不同的面向不同客户的软件，所以苹果需要确认不是另一个软件的拷贝版本。

#### 解决方法：

在 App 提交审核的页面备注里，我们做了如下解释：该应用主要用于控制特定的智能设备，是为其他客户量身定制的，并没有存在上述问题。

### 8.Guideline 4.3 - Design (This app duplicates the content and functionality of other apps submitted by you or another developer to the App Store, which is considered a form of spam. - Apps that simply duplicate content or functionality create clutter, diminish the overall experience for the end user, and reduce the ability of developers to market their apps. -The next submission of this app may require a longer review time, and this app will not be eligible for an expedited review until this issue is resolved. -Next Steps  - Review the Design section of the App Store Review Guidelines.  - Ensure your app is compliant with all sections of the App Store Review Guidelines and the Terms & Conditions of the Apple Developer Program.   - Once your app is fully compliant, resubmit your app for review. - Submitting apps designed to mislead or harm customers or evade the review process may result in the termination of your Apple Developer Program account. Review the Terms & Conditions of the Apple Developer Program to learn more about our policies regarding termination. - If you believe your app is compliant with the App Store Review Guidelines, you may submit an appeal. Alternatively, you may provide additional details about your app by replying directly to this message.)

#### 环境参数：

```
App Store Review
```

#### 问题分析：

因为上传了功能或者 UI 相似的应用在 App Store，可能还包括但不限于相同类型产品功能较一致，以及上传马甲或者分包灯问题，导致审核被拒。

#### 解决方法：

这里推荐有两种方法可以参考。

第一个处理方法：修改元数据内容 ①修改应用程序价格，打造与原产品不同的价格级别； ②修改应用程序发布地区，打造与原产品不同的售卖地区或分不同地区运营； ③修改产品分类，打造与原产品不同的产品侧重属性分类； ④回复苹果产品设计理念等，表述产品情怀，希望打造独一无二的产品，比如功能目前会跟其他类似，会有相同情况；然后提出产品内某功能加以细节性说明，比如功能在市场上其他人还没做等等（此做法请慎重，描述好了 ok，描述差了打脸）。 修改后回复内容可参考以下内容： 尊敬的苹果开发者审核， 您好，针对于贵方提出的 4.3 相关问题，我方目前已修改“地区/售价/分类”，主要目的在于针对不同的人群属性做运营方面的区分，我们希望给予用户不同的产品体验，包括应用程序内的功能侧重点，展现给用户的内容等等；希望贵方能重新审核，及时给予我方 App 通过审核并发布至 App Store。 诚挚的问候！

第二个处理方法：修改 App 二进制代码 ①升级 version，升级一个版本号提交审核； ②换 bundle id，换一个包再提交审核； ③换开发者账号，换不同账号提交审核； ④修改素材及 UI 色调等，修改 logo，修改主色调； ⑤修改功能界面等，此处可改功能也可做小开关； ⑥修改内部文件类名前缀，添加垃圾代码或者注释块，此处主要防苹果机审扫描。

第三个处理方法：直接和苹果审核部门沟通。之前是可以直接通过打电话（Tel：4006 701 855）的方式联系到苹果审核部门的，但可能由于苹果公司内部机制变更，这个电话打过去，对方会给你提供一个网站：[联系他们](https://developer.apple.com/contact/)。你需要做的是在“申诉 App 被拒或 App 移除”的页面中认真详细的描述被拒 App 的不同点，提出合理有力的申诉。这里分享一段被拒了一个月的 App 重新上架的申诉文本供大家参考：

对于贵方提出的 Guideline 4.3  问题，我方想反映的情况如下：

该应用不同于其他应用的主要特征在于，通过蓝牙地址过滤，它被用于控制特定的智能设备。
以及软件内的 UI，比如当前应用的不同灯光控制界面，引导界面，是否有语音助手功能以及语音助手的服务来源等。

本应用被限定为只针对特定的硬件设备进行控制，目的是想对不同用户群属性进行运营方面的区分。

我们希望给予用户获取到我们不同的硬件产品的同时，也能从移动端上获得不同的产品体验，包括应用程序内的功能侧重点，展现给用户的内容等。

希望贵方能重新审核，及时给予我方应用程序通过审核并发布至App Store。诚挚的问候！

For the Guideline 4.3 question you have raised, we would like to reflect the following:

The main feature of this application, unlike other applications, is that it is used to control specific smart devices through Bluetooth address filtering.
And the UI in the software, such as the different lighting control interface of the current application, the guiding interface, whether there is a voice assistant function and the service source of the voice assistant.

This application is limited to control only for specific hardware devices in order to differentiate the operational aspects of different user group attributes.

We want to give users access to our different hardware products, as well as different product experiences from the mobile side, including the functional focus within the application, the content presented to the user, and so on.

I hope that you can re-review and give us the application to review and publish to the App Store in time. sincere regards!

（Translation is taken directly from Google, please bear with me.）

第二个方法涉及到需要大量的修改代码，这里给推荐一个造好的轮子，可以直接修改素材，类名前缀，和增加垃圾代码：[KLGenerateSpamCode 垃圾代码生成器](https://github.com/klaus01/KLGenerateSpamCode)

顺便说下，苹果的三种审核机制：1，预审核 --- 扫描 api，及 plist 文件字符缺失等；此处分两步，第一步为上传时苹果 Application Loador 等应用对于适配 icon 等的检查，第二步为上传后苹果的功能性检查，例如配置了 Push 功能但有缺失或者未打开功能，则会邮件提示等等； 2，机审--- 此处扫描支付 SDK 等，及马甲情况，机器扫描主要看代码块，可参考百度蜘蛛抓取网站模块原理；如遇部分无法过机审情况可尝试加速绕过机审（不是 100% 成功）； 3，人工审核--- 此处主要检测功能或者 App 体验测试，例如用测试账号登录 App 体验功能，或其他是否明显 bug 等，ipv6 也在此处检测。

### 8.SDK 值无效。在 App Name 中为 LC_VERSION_MIN_IPHONEOS 的 SDK 部分提供的值为 12.0，该值大于允许的最大值 11.4。

#### 环境参数：

```
App Store Review
```

#### 问题分析：

上传 App 到应用商店时使用的 Xcode，被修改了内部文件用以支持最新的测试版系统，导致上传后无法提交应用审核。

#### 解决方法：

用纯净无修改的 Xcode 重新上传可解决问题。

### 9.Missing Push Notification Entitlement - Your app appears to register with the Apple Push Notification service, but the app signature's entitlements do not include the "aps-environment" entitlement. If your app uses the Apple Push Notification service, make sure your App ID is enabled for Push Notification in the Provisioning Portal, and resubmit after signing your app with a Distribution provisioning profile that includes the "aps-environment" entitlement. 

#### 环境参数：

```
App Store Review
```

#### 问题分析：

上传 App 到应用商店时，注册了推送通知证书，但提交时在工程内部未勾选推送通知功能。

#### 解决方法：

勾选通知功能，重新上传即可解决问题。
