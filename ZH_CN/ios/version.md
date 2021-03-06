变更历史
===
## 2.14.0
- 【代码变更】
  1.【MSDK】
    * 新增[Bugly自定义日志上报功能接口](rqd.md#Bugly自定义日志上报)。
    * 修正登录时无网络或网络错误时本地票据被清空的问题。
    * 修正微信加绑群返回码错误的问题。
    * 修正切换账号Bugly上报Open ID错误的问题。
    * 修正新注册游客数据上报无HID的问题。
    * 授权登录接口强制回到主线程中调用。
    * 关系链接口性别统一返回男、女。
    
  2.【MSDKMarketing】
    * 新增[内置浏览器JS关闭接口](InnerBrowser.md#JS关闭内置浏览器)。
    * 新增[内置浏览器导航栏可隐藏配置开关](InnerBrowser.md#配置导航栏可隐藏开关)。
    * 新增内置浏览器User Agent增加MSDK标识。
    
## 2.13.0
- 【代码变更】
  1.【MSDK】
    * 支持iOS9编译，支持bitcode。
    * 新增打开浏览器接口支持mqqapi协议。
    * 微信加绑群更能更新为拉起微信客户端方式。
    * 修正首次启动后启动页杀进程，再次启动时偶现分享无权限的问题。
  
  2.【MSDKXG】
    * 新增[信鸽标签推送功能](Push.md#标签推送)。
    
  3.【MSDKFoundation】
    * App Log格式更新。
- 【组件更新】
    * 【更新】更新信鸽SDK2.4.5版本。
    * 【更新】更新微信SDK1.6.2版本，修正打开bitcode Archive打包报错的问题。
    * 【更新】更新MTA SDK1.4.6版本，修正打开bitcode Archive打包报错的问题。
    
## 2.12.0
- 【代码变更】
  1.【MSDK】
    * 支持iOS9编译。
    * 新增微信DeepLink功能。
    * 本地票据针对设备加密功能。
    * 扫码登录支持横屏界面展示。
    * 修正后端分享给微信好友、后端分享给微信游戏中心、后端分享给手Q好友三个分享接口分享结果未携带msdkExtInfo字段的问题。
    * 由于微信H5页面分享不再维护，MSDK过期相关微信H5分享接口。
    
  2.【MSDKMarketing】
    * 修正内置浏览器偶现横竖屏展示半屏问题。
- 【组件更新】
    * 【更新】更新openSDK2.9.4版本。
    * 【更新】更新微信SDK1.6.1版本。
    * 【更新】更新信鸽SDK2.4.4版本。
    * 【更新】更新灯塔SDK1.9.0版本。
    * 【更新】更新MTA SDK1.4.6版本。
    * 【更新】更新bugly SDK1.4.0版本。
- 【接入注意事项】
    * 1.使用Xcode7.0及以上版本iOS9.0及以上SDK编译。
    * 3.所有.dylib结尾的系统依赖库需更换为对应的.tdb结尾的依赖库。
    * 4.info.plist需配置跳转scheme以及https声明，[点击此处](http://wiki.dev.4g.qq.com/v2/ZH_CN/ios/index.html#!index.md#)查看详情。
    * 5.添加libc++.tdb系统依赖库，否则部分游戏可能会出现类似以下类型的报错：
    	std::__1::__vector_base_common<true>::__throw_length_error() const
    	std::__1::__basic_string_common<true>::__throw_length_error() const    
    	std::__1::ios_base::getloc() const
    	std::runtime_error::runtime_error(char const*)
    
## 2.11.0
- 【代码变更】
  1.【MSDK】
    * 删除微信授权多余scope。
    * 新增游戏关键场景定位功能。新增设置游戏当前场景开始点接口[WGStartGameStatus](gameStatus.md#接口说明)和结束点接口[WGEndGameStatus](gameStatus.md#接口说明)。
    
  2.【MSDKXG】
    * 新增本地推送功能。新增添加本地推送接口[WGAddLocalNotification](Push.md#添加本地推送)、添加本地前台推送接口[WGAddLocalNotificationAtFront](Push.md#添加本地前台推送)、清除未生效本地推送接口[WGClearLocalNotification](Push.md#清除未生效本地推送)、清空所有本地推送接口[WGClearLocalNotifications](Push.md#清空所有本地推送)。
        
  3.【MSDKFoundation】
    * 修正eFlag_Checking_toke为eFlag_Checking_Token。
- 【组件更新】
    * 更新bugly1.2.8版本，支持添加场景，可准确区分登陆、分享、游戏特殊场景的Crash。
    
## 2.10.0
- 【代码变更】
  1.【MSDK】
    * 新增[创建公会微信群](WX.md#创建公会微信群)、[加入公会微信群](WX.md#加入公会微信群)、[查询公会微信群信息](WX.md#查询公会微信群信息)、[分享结构化消息到公会微信群](WX.md#分享结构化消息到公会微信群)功能。
    * 新增[Url添加加密票据](tool.md#Url添加加密票据)接口。
    
  2.【MSDKMarketing】
    * 新增[内置浏览器Javascript分享接口](InnerBrowser.md#Javascript分享接口)功能。

## 2.9.7
- 【代码变更】
  1.【MSDK】
    * 修正切换账号Bugly上报Open ID错误的问题。
    
## 2.9.6
- 【代码变更】
  1.【MSDK】
    * 支持iOS9编译，支持bitcode。
    * 登陆增加内网ip上报，用于qos加速。

- 【组件更新】
    * 【更新】更新微信SDK1.6.2版本，修正打开bitcode Archive打包报错的问题。
    * 【更新】更新MTA SDK1.4.9版本，修正打开bitcode Archive打包报错的问题。
    
## 2.9.5
- 【代码变更】
  1.【MSDK】
    * 支持iOS9编译。
    * 扫码登录支持横屏界面展示。
    
  2.【MSDKMarketing】
    * 修正内置浏览器偶现横竖屏展示半屏问题。
- 【组件更新】
    * 【更新】更新openSDK2.9.4版本。
    * 【更新】更新微信SDK1.6.1版本。
    * 【更新】更新信鸽SDK2.4.4版本。
    * 【更新】更新灯塔SDK1.9.0版本。
    * 【更新】更新MTA SDK1.4.6版本。
    * 【更新】更新bugly SDK1.4.0版本。
- 【接入注意事项】
    * 1.使用Xcode7.0及以上版本iOS9.0及以上SDK编译。
    * 3.所有.dylib结尾的系统依赖库需更换为对应的.tdb结尾的依赖库。
    * 4.info.plist需配置跳转scheme以及https声明，[点击此处](http://wiki.dev.4g.qq.com/v2/ZH_CN/ios/index.html#!index.md#)查看详情。
    * 5.添加libc++.tdb系统依赖库，否则部分游戏可能会出现类似以下类型的报错：
    	std::__1::__vector_base_common<true>::__throw_length_error() const
    	std::__1::__basic_string_common<true>::__throw_length_error() const    
    	std::__1::ios_base::getloc() const
    	std::runtime_error::runtime_error(char const*)
    
## 2.9.4
- 【代码变更】
  1.【MSDK】
    * 支持iOS9编译，暂不支持bitcode。
    * 修正iOS9编译手Q WebView授权页白屏问题。
- 【组件更新】
    * 【更新】更新openSDK2.9.4版本，修正iOS9编译手Q WebView授权页白屏、iOS8.0以下系统crash的问题。
- 【接入注意事项】
    * 1.使用Xcode7.0及以上版本iOS9.0及以上SDK编译。
    * 2.在build settings中将Enable Bitcode设置为NO。
    * 3.所有.dylib结尾的系统依赖库需更换为对应的.tdb结尾的依赖库。
    * 4.info.plist需配置跳转scheme以及https声明，[点击此处](http://wiki.dev.4g.qq.com/v2/ZH_CN/ios/index.html#!index.md#)查看详情。
    
## 2.9.3
- 【代码变更】
  1.【MSDK】
    * 优化游客数据迁移算法，防止迁移失败。
    * 修改与iOS私有API重名的变量及方法名，防止审核被拒。
    * 修改票据解密算法，兼容票据未加密版本升级后解密问题。
    
## 2.9.2
- 【代码变更】
  1.【MSDK】
    * 修改游客数据优先继承2.3.*版本的，经历过2.4.0~2.8.0版本的游戏无需关注该版本。
    * 优化游客数据迁移算法，防止迁移失败。
    * 修改与iOS私有API重名的变量及方法名，防止审核被拒。
    
## 2.9.1
- 【代码变更】
  1.【MSDK】
    * 修正手Q本地票据登陆接口未校验payToken的问题。
    * 修正微信大图分享图片过大卡死的问题。
	
## 2.9.0
- 【代码变更】
  1.【MSDK】
    * 新增[微信扫码登录](WX.md#微信扫码登录)功能，二维码显示界面只支持竖屏显示，游戏需在info.plist中配置支持竖屏UIInterfaceOrientationMaskPortrait。
    * 新增游客模式数据上报功能。
    * 最低版本修正为支持到iOS6.0。
    
  2.【MSDKMarketing】
    * 新增[内置浏览器支持自定义方向](InnerBrowser.md#指定屏幕方向打开浏览器)功能。
    
- 【组件更新】
	* 【更新】更新微信SDK，支持扫码登陆。
	
## 2.8.2
- 【代码变更】
  1.【MSDK】
    * 修正票据未加密版本覆盖安装升级后首次使用本地票据登录票据缺失的问题。
    
- 【组件更新】
    * 【更新】更新灯塔1.8.7版本，修正磁盘满时可能导致的crash隐患。

## 2.8.1
- 【代码变更】
  1.【MSDKFoundation】
    * 删除WXAppKey（QQAppKey由于注册信鸽需要暂时保留），新增MSDKKey，游戏需在plist中配置String类型的MSDKKey字段，具体MSDKKey值可在[飞鹰系统](http://dev.ied.com)游戏管理->游戏详情页查询。
    
  2.【MSDK】
    * 本地票据信息修改为加密存储。
	
## 2.8.0
- 【代码变更】
  1.【MSDK】
    * 修正手Q平台登录获取附近的人接口图片链接问题。
    * 新增微信结构化消息、URL消息、音乐消息缩略图自动压缩功能。
    
  2.【MSDKMarketing】
    * 修正内置浏览器URL拼接AppID、AppKey拼反的问题。
	* 新增支持公告展示优先级(公告置顶)功能。
	
## 2.7.0
- 【代码变更】
  1.【MSDK】
    * 修正修正快速登录场景下，首次登陆没有登陆回调和pf的bug。
    * 修正手Q（微信）授权页面不授权，切入后台，再次点击QQ（微信）登录，不需要授权就登录成功的bug。
    * 修正获取附近的人返回时错误回调的bug。
    * 优化游客模式注册算法，注册失败将自动重试一次。
    * 新增结构化消息、URL消息至会话和朋友圈、大图分享至朋友圈微信Web接口，其他接口暂不支持Web分享。
    
  2.【MSDKMarketing】
    * 修正内置浏览器中打开AppStore链接卡死的问题。
	* 拆分内置浏览器分享开关，原有plist配置项MSDK_WebView_Share_SWITCH不再使用，改为由MSDK管理端下发，游戏可根据需要申请打开或关闭。
	* 新增内置浏览器通过JS在Safiri中打开URL功能，说明详见[通过JS在Safiri中打开指定URL](InnerBrowser.md#通过JS在Safiri中打开指定URL)。 
	* 新增内置浏览器通过JS打开iOS图库、相机获取照片功能，说明详见[通过JS打开iOS图库、相机获取照片](InnerBrowser.md#通过JS打开iOS图库、相机获取照片)。 
	
## 2.6.6
- 【代码变更】
  1.【MSDK】
    * 登录接口新增内网ip上报。
	
## 2.6.5
- 【代码变更】
  1.【MSDK】
    * 修正特殊情况下游客数据丢失的问题。
- 【组件更新】
    * 【更新】更新灯塔1.8.7版本，修正磁盘满时可能导致的crash隐患。
    
  

## 2.6.4
- 【代码变更】
  1.【MSDK】
    * 修正手Q平台登录获取附近的人接口图片链接问题。
    
  2.【MSDKMarketing】
    * 修正内置浏览器URL拼接AppID、AppKey拼反的问题。

## 2.6.3
- 【代码变更】
  1.【MSDKFoundation】
	* 修正MSDKLog特殊情况下（如log中包含特殊字符）crash的隐患。
- 【组件更新】
    * 【更新】更新信鸽1.0.1版本，修正部分appid号段注册信鸽失败的问题。

## 2.6.2
- 【代码变更】
  1.【MSDK】
	* 修正WGGetNoticeData接口无法获取到公告数据的bug；
	* 修正分享url消息到微信好友，Android端点击分享消息打开url界面错误的bug；
	* 票据自动刷新流程梳理和修正；

## 2.6.1
- 【代码变更】
  1.【MSDKFoundation】
	* 新增游戏时长统计上报AppID、OpenID、IDFA等字段。

  2.【MSDK】
	* 修正2.6.0Crash上报未自动开启的bug；
	* 修正2.6.0自动登录时crash上报无userID的bug；
	* 修正2.6.0通过游戏中心携带票据快速登录时本地票据未同步刷新的bug；
	* 修正2.6.0异账号情况下通过分享消息拉起游戏无异账号提醒的bug；
- 【组件更新】
	* 【更新】OpenSDK更新到2.8.2，修正非H5白名单游戏不能分享大图到Qzone的bug。
	* 【更新】RQD更新为Bugly1.2.2。

## 2.6.0
- 【代码变更】
  1.【MSDKFoundation】
	* 修正日志记录（MSDKLogger）对NSUserdefaults的频繁访问，减少crash隐患。
	* 增加idfa的获取和上报，后续游戏在提审时需要要iTC勾选iAD相关项（[说明](http://km.oa.com/articles/show/234073)），在工程中需要导入AdSupport.framework；
	
  2.【MSDK】
	* 修正2.5.0自动刷新票据机制的bug；
	* 修正2.5.0没有登录时上报获取appid不正确的问题；
	* 票据刷新机制优化，说明详见[MSDK2.6.0i及以后的票据自动刷新流程](login.md#MSDK2.6.0i以后的票据自动刷新流程) ；
	* 手Q可通过内置浏览器形式分享，如需使用这个功能，游戏需向OpenApiHelper申请开通手Q H5分享的权限。开通之后，分享操作将在游戏内部浏览器完成，避免审核风险；
- 【组件更新】
	* 【更新】OpenSDK更新到2.8.1，以支持手Q H5分享功能。

## 2.5.0
- 【代码变更】
1.【新增】新增在线时长上报统计。
2.【新增】新增内置浏览器分享入口开关，可在info.plist文件中配置MSDK_WebView_Share_SWITCH，YES时内置浏览器会显示分享按钮，NO时不显示。
3.【修改】Guest模式优化，进行keychain数据备份。
4.【修改】修复内置浏览器在iOS5.1.1系统iPad端不能拉起微信分享的bug。
- 【组件新增】
1.【新增】在demo中集成手游宝助手SDK，游戏可根据需要集成。

## 2.4.0
- 【代码变更】
1.【修改】MSDK模块化，按功能拆分为四个模块：
  1. MSDKFoundation：基础依赖库，若要使用其他库均需先导入该框架。
  2. MSDK:手Q和微信登录、分享功能；
  3. MSDKMarketing：提供交叉营销、内置浏览器功能。公告、内置浏览器所需的资源文件放置在WGPlatformResources.bundle文件中。
  4. MSDKXG：提供信鸽Push功能。
  以上四个组件同时提供C99和C11语言标准，其中**_C11包为C11的版本。
  ![linkBundle](./2.4.0_structure_of_framework.png)
  
  如果只想使用C++接口，只需要导入以下几个头文件即可：
```
<MSDKFoundation/MSDKStructs.h>
<MSDK/WGInterface.h>
<MSDK/WGPlatform.h>
<MSDK/WGPlatformObserver.h>
```  
    另：模块化版本除了支持2.3.4及之前版本的Observer回调外还新增了delegate回调，此处以手Q授权登陆为例（其他接口详见各自接口说明文档）：
    原授权调用代码如下：
```
WGPlatform* plat = WGPlatform::GetInstance();//初始化MSDK
MyObserver* ob = new MyObserver();
plat->WGSetObserver(ob);//设置回调
plat->WGSetPermission(eOPEN_ALL);//设置授权权限
plat->WGLogin(ePlatform_QQ);//调用手Q客户端或web授权
```
    原授权回调代码如下：
```
void MyObserver::OnLoginNotify(LoginRet& loginRet)
{
if(eFlag_Succ == loginRet.flag)
{
…//login success
std::string openId = loginRet.open_id;
std::string payToken;
std::string accessToken;
if(ePlatform_QQ == loginRet.Platform)
{
for(int i=0;i< loginRet.token.size();i++)
{
TokenRet* pToken = & loginRet.token[i];
if(eToken_QQ_Pay == pToken->type)
{
paytoken = pToken->value;
}
else if (eToken_QQ_Access == pToken->type)
{
accessToken = pToken->value;
}
}
}
else if (ePlatform_Weixin == loginRet.platform)
{
….
}
} 
else
{
…//login fail
NSLog(@"flag=%d,desc=%s",loginRet.flag,loginRet.desc.c_str()); 
}
}
```
    2.4.0i及以后版本还可使用delegate方式，代码如下：
```
[MSDKService setMSDKDelegate:self];
MSDKAuthService *authService = [[MSDKAuthService alloc] init];
[authService setPermission:eOPEN_ALL];
[authService login:ePlatform_QQ];
```
    回调代码如下：
```
-(void)OnLoginWithLoginRet:(MSDKLoginRet *)ret
{
//内部实现逻辑同void MyObserver::OnLoginNotify(LoginRet& loginRet)
}
```

## 2.3.6
 - 【组件更新】
1.【修改】更新信鸽1.0.1版本，修正部分手Q Appid号段注册信鸽失败的问题。

## 2.3.5
 - 【代码变更】
1.【修改】修正MSDKLog特殊情况下crash的隐患。

## 2.3.4
 - 【组件更新】
1.【修改】更新OpenSDK2.5.1，修正在5.1.1下crash的问题。

## 2.3.3
 - 【代码变更】
1.【修改】修正没有登录时，手Q快速启动导致的crash。
2.【修改】修正游客模式第一次登录没有pf和pfkey的问题。
 - 【组件更新】
1.【修改】更新MTA1.4.2，修正在SDK8下编译的工程crash的隐患。

## 2.3.2
 - 【代码变更】
1.【修改】更新OpenSDK2.5.1，修正在iOS8.1.1上，没有安装手Q时使用webView无法正常登录的问题。
 - 【编译变更】
1. Tencent_MSDK_IOS_V2.3.2i(支持arm32, iOS SDK7编译)：由于部分游戏使用的游戏引擎对iOS SDK8支持较差，MSDK使用iOS SDK7编译了32位包供这部分游戏使用。
2. Tencent_MSDK_IOS_V2.3.2i(支持arm64, iOS SDK8编译)：使用iOS SDK8编译的支持arm64的包。
---

## 2.3.1
 - 【代码变更】
1.【修改】更新RQD和灯塔，修正Crash上报本身导致的Crash隐患。
---

## 2.3.0

 - 【代码变更】
1.【修改】资源文件打包至WGplatformResources.bundle
2.【删除】米大师解耦，删除如下接口：
```
void WGRegisterPay(unsigned char* offerId, unsigned char* openId, unsigned char* openKey, unsigned char* sessionId, unsigned char* sessionType, unsigned char* custom);// since 1.2.6
void WGPay(unsigned char* offerId, unsigned char* openId, unsigned char* openKey, unsigned char* sessionId, unsigned char* sessionType, unsigned char* payItem, unsigned char* productId, bool isDepositGameCoin, uint32_t productType, uint32_t quantity, unsigned char* zoneId, unsigned char* varItem, unsigned char* custom);
void WGIAPRestoreCompletedTransactions(unsigned char* offerId, unsigned char* openId, unsigned char* openKey, unsigned char* sessionId, unsigned char* sessionType, unsigned char* payItem, unsigned char* productId, bool isDepositGameCoin, uint32_t productType, uint32_t quantity, unsigned char* zoneId, unsigned char* varItem, unsigned char* custom);
void WGIAPLaunchMpInfo(unsigned char* offerId, unsigned char* openId, unsigned char* openKey, unsigned char* sessionId, unsigned char* sessionType, unsigned char* payItem, unsigned char* productId, bool isDepositGameCoin, uint32_t productType, uint32_t quantity, unsigned char* zoneId, unsigned char* varItem, unsigned char* custom);
void WGDipose();//since 1.2.6
bool WGIsSupprotIapPay();//since 1.2.6
void WGSetOfferId(unsigned char* offerId);//since 1.2.6
void WGSetIapEnvirenment(unsigned char* envirenment);
void WGSetIapEnalbeLog(bool enabled);
```
3、【新增】微信分享URL接口：
        void WGSendToWeixinWithUrl(
                        const eWechatScene& scene,
                        unsigned char* title,
                        unsigned char* desc,
                        unsigned char* url,
                        unsigned char* mediaTagName,
                        unsigned char* thumbImgData,
                        const int& thumbImgDataLen,
                        unsigned char* messageExt
                        );
4、【新增】推送总开关，需在plist配置MSDK_PUSH_SWITCH(string)为ON，若为其他值或不配置，则推送失效
5、【修改】删除滚动公告的配置项中(top,left,width)的配置，公告置顶占满屏幕宽度，若不设置高度则默认30pt
6、【新增】iOS8支持，LBS接口需要增加plist字段requestWhenInUseAuthorization，公告广告等显示的异常修复
7、【修改】优化Guest的存储，每个App存储在不同的key，避免企业证书写入Guest数据，互相覆盖。同时增加了迁移逻辑，避免进度丢失。
8、【修改】增加两个try－catch保护，避免user default读写时导致crash

 - 【修复BUG】
1、【修改】修复手Q游戏内加好友，好友备注和申请信息颠倒的问题
2、【修改】修复广告拉取os=1(安卓)的问题
3、【修改】修复AHAlertView以及子类在横屏展现错误的问题
4、【修改】修复WGRotationView以及子类在横竖屏ios7,8展现错误的问题
5、【修改】修复游客模式同证书不同应用相互覆盖guestid的问题
6、【修改】修复内置浏览器分享页面在iOS7,8的显示问题
7、【修改】修复内置浏览器广告按钮在没有广告时的显示问题
8、【修改】修复游客模式发送注册消息有概率失败的问题
9、【修改】修正RDM Crash上报时，AppID没有上报的问题。

---

## 2.2.0
- 【代码变更】
1、本地关键日志云端控制上报
2、MSDK在http报头的User-agent字段加上终端来源信息的需求			
3、信鸽PUSH发送全量用户	新需求，需plist配置MSDK_XGPUSH_URL			
4、MSDK 封装微信个人信息接口

---

##2.1.0
- 【代码变更】
1、增加广告特性，通过调用WGShowAD(_eADType& scene)接口展示广告，增加WGADObserver，用于广告点击回调；广告的相关配置放在MsdkResources/AdvertisementResources/AdvertisementConfig.plist
2、增加WGGetLocationInfo接口和OnLocationGotNotify回调，获得用户GPS地址并上报到MSDK后台；
3、WGGetNearBy接口增加gpsCity返回；
4、内置浏览器链接附带明文openid；
5、增加LoginInfo类，提供反射形式获取登录票据，减少耦合性。使用代码示例：
```
 Class loginInfoClass = NSClassFromString(@"LoginInfo");
    if (loginInfoClass) {
        id obj = [[[loginInfoClass alloc]init]autorelease];
        if ([obj respondsToSelector:@selector(description)]) {
            NSLog(@"Login info:%@",[obj description]);
        }
    }
```

---

## 2.0.7
 - 【代码变更】
1.【修改】更新OpenSDK2.5.1，修正在iOS8.1.1上，没有安装手Q时使用webView无法正常登录的问题。
---

## 2.0.6
 - 【代码变更】
1.【修改】增加Crash上报时的AppID和OpenId上报。
2.【修改】更新RQD和灯塔，修正Crash上报本身导致的Crash隐患
---

##2.0.5
- 【代码变更】
1.删除了使用苹果私有接口的代码

---

##2.0.4
- 【代码变更】
该版本合并2.0.2i与2.0.3i，无新增功能点。

---

##2.0.3
- 【代码变更】
1. 【新增】WGPlatform.h新增如下接口：
```
   /**
     *  获取游客模式下的id
     * 
     *
     */
    std::string WGGetGuestID();
    
    /**
     *  刷新游客模式下的id
     *
     *
     */
    void WGResetGuestID();
```
2. 【删除】删除如下接口：
```
    void WGRegisterAPNSPushNotification(NSDictionary *dict);
    void WGSuccessedRegisterdAPNSWithToken(NSData *data);
    void WGFailedRegisteredAPNS();
    void WGCleanBadgeNumber();
    void WGReceivedMSGFromAPNSWithDict(NSDictionary* userInfo);
```
3. 【修改】修改WGPublicDefine.h错误的#endif宏位置
4. 【新增】新增公共文件WGApnsInterface，包含如下接口：
```
    + (void)WGRegisterAPNSPushNotification:(NSDictionary*)dict;
    + (void)WGSuccessedRegisterdAPNSWithToken:(NSData *)data;
    + (void)WGFailedRegisteredAPNS;
    + (void)WGCleanBadgeNumber;
    + (void)WGReceivedMSGFromAPNSWithDict:(NSDictionary*) userInfo;
```
5. 【新增】增加内部文件GuestInterface处理游客模式逻辑

【文档调整】
1. 【新增】第13章：游客模式相关说明；
2. 【新增】第1章：增加一次性出C99和C11包的说明；
2. 【新增】第12章：修好APNS相关说明，改为调用WGApnsInterface

---

##2.0.2
- 【代码变更】
1、增加游戏内好友的三个接口，更新OpenSDK2.5，相应的需要使用手Q新版本：
```
    /**
	 * 游戏内加群,公会成功绑定qq群后，公会成员可通过点击“加群”按钮，加入该公会群
	 * @param cQQGroupKey 需要添加的QQ群对应的key，游戏server可通过调用openAPI的接口获取，调用方法可RTX 咨询 OpenAPIHelper
	 */
	void WGJoinQQGroup(unsigned char* cQQGroupKey);
	/**
	 * 游戏群绑定：游戏公会/联盟内，公会会长可通过点击“绑定”按钮，拉取会长自己创建的群，绑定某个群作为该公会的公会群
	 * @param cUnionid 公会ID，opensdk限制只能填数字，字符可能会导致绑定失败
	 * @param cUnion_name 公会名称
	 * @param cZoneid 大区ID，opensdk限制只能填数字，字符可能会导致绑定失败
	 * @param cSignature 游戏盟主身份验证签名，生成算法为openid_appid_appkey_公会id_区id 做md5.
	 * 					   如果按照该方法仍然不能绑定成功，可RTX 咨询 OpenAPIHelper
	 *
	 */
	void WGBindQQGroup(unsigned char* cUnionid, unsigned char* cUnion_name,
                       unsigned char* cZoneid, unsigned char* cSignature);
	/**
	 * 游戏内加好友
	 * @param cFopenid 需要添加好友的openid
	 * @param cDesc 要添加好友的备注
	 * @param cMessage 添加好友时发送的验证信息
	 */
	void WGAddGameFriendToQQ(unsigned char* cFopenid, unsigned char* cDesc,
                             unsigned char* cMessage);
```
---

##2.0.1
- 【代码变更】
1、公告增加图片公告类型，公告结构体增加图片数据，详见MSDK接入文档2.0
2、增加LoginWithLocalInfo接口来校验票据，游戏启动或从后台切到前台调用此方法。

---

##2.0.0
- 【代码变更】
1. 浏览器功能点优化，修正内置浏览器；
2. 增加图片、网页公告类型；定时下载公告数据;
3. 增加自动登陆流程，进行票据校验,定时刷新accessToken等票据；
4. 更新手Q sdk1.1.1版本，修复手Q授权游戏被回收导致的授权失败bug；
5. 本地日志方案，在info.plist配置MSDK_LOG_TO_FILE为YES，将记录MSDK的输出日志到Caches/msdk.log；
