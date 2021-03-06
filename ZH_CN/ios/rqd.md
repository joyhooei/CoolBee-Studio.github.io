MSDK Crash上报模块
===
##概述
Crash上报在MSDK2.6.1i之前（不包括MSDK2.6.1i)使用的是RQD上报，上报成功后具体crash详细堆栈只能在 http://rdm.wsd.com/ 上查看，必须是腾讯公司员工用RTX登录进行查看，非自研游戏查看起来非常不方便。自MSDK2.6.1i及以后使用的bugly上报，此时可以在 http://bugly.qq.com/ 上进行查看。可使用QQ账号绑定相关应用，这样，非自研游戏可以方便的查看。当然，在 http://rdm.wsd.com/ 同样是可以查看的,游戏无需额外操作。

##上报开关设置
打开和关闭数据上报的设置函数:
```
/**
 * @param bRDMEnable 是否开启RDM(Bugly)的crash异常捕获上报
 * @param bMTAEnable 是否开启MTA的crash异常捕获上报
 */
void WGEnableCrashReport(bool bRDMEnable, bool bMTAEnable);
```
在WGPlatform里面有这个函数，如果将bRdmEnable设为false（bMtaEnable可设为false），则关闭rdm crash上报。

##在RDM平台上查看Crash数据
####注册绑定

如果是DEV注册的游戏会自动注册RDM，不需要手动注册; 手动注册，直接登录RDM，点击异常上报模块，配置一下产品 BoundID 即可。

步骤：登录[http://rdm.wsd.com/](http://rdm.wsd.com/), 进入你们的产品 -> 异常上报，如果未注册会提醒如图：

![rdmregister](./rmdregister.png)

其中，boundID 就是你的AndroidManifest中的packageName。未注册的产品，数据上报时直接丢弃。

具体请咨询rdm小秘书，android问题可联系spiritchen

####如何查看上报数据
- 网址:[http://rdm.wsd.com/](http://rdm.wsd.com/)->异常上报->问题列表

![rdmwsd](./rdmwsd.png)
![rdmdetail](./rdmdetail.png)


##在bugly平台上查看Crash数据
网址:[http://bugly.qq.com/](http://bugly.qq.com/)->用QQ账号登录->选择相应的App

![rqd](./bugly1.png)

##Crash上报添加额外业务日志
当程序Crash时，有时需要添加一些额外的业务日志，随crash日志一起上报到http://rdm.wsd.com/ 平台，这样可以更好的定位造成crash的原因,最终可以在rdm平台上查看错误详情。

![rqd](./rqd_extramsg.png)

要完成此功能只需要在全局observer中添加回调函数OnCrashExtMessageNotify：
```
std::string MyObserver::OnCrashExtMessageNotify()
{
	return "dsafdasfsafdasdfasdf";
}
```

##Bugly自定义日志上报
除了Crash时上报添加额外业务日志，游戏还可添加游戏过程中的日志信息，当程序Crash时可以精准定位到Crash场景，以便方便快捷的定位crash的原因,最终结果可以在Bugly平台上查看错误详情。

```
/**
 * 上报日志到bugly
 * @param level 日志级别，默认eBuglyLogLevel_I
 * 	eBuglyLogLevel_S (0), //Silent
 *  eBuglyLogLevel_E (1), //Error
 *  eBuglyLogLevel_W (2), //Warning
 *  eBuglyLogLevel_I (3), //Info
 *  eBuglyLogLevel_D (4), //Debug
 *  eBuglyLogLevel_V (5); //Verbose
 *
 * @param log 日志内容
 *
 *  该日志会在crash发生时进行上报，上报日志最大30K
 */
void WGBuglyLog(eBuglyLogLevel level, unsigned char* log);
```
Bugly平台Crash自定义日志示例：

![rqd](./bugly_extramsg.png)
