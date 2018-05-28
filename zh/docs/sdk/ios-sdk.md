# IOS SDK OC版开发文档

本文档旨在说明如何使用LumiSDK进行网关设备快联入网的开发。



## 前期准备

1、访问并登录[AIOT开放平台](https://opencloud.aqara.cn/)网站，创建新应用后，在“应用管理”-“应用概览”页面获取“AppId”和“AppKey”。

2、根据[云端开发手册](http://docs.opencloud.aqara.cn/development/cloud-development/#oauth20)中“OAuth 2.0”章节，获取openID。

3、下载并解压[IOS SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/aiot_sdk_fastlink_ios_v0.3.zip)，在LHSDKLib文件夹下可查看SDK主要文件：libLumiSDK.a、LHOAuthAIOT.h和LHFastLink.h；在LHSDKDemo文件夹可查看Demo。

- **libLumiSDK.a**：静态连接库


- **LHOAuthAIOT.h**：授权接口，授权后才可使用快联功能。（LHOAuthAIOT.h接口授权需要连接网络，期间可能需要等待一段时间，建议将其放在AppDelegate中使用。）
- **LHFastLink.h**：快联接口。

4、下载并安装Xcode或其他IOS集成开发环境。



## 使用说明

#### 1、配置IOS项目，添加静态链接库。（以Xcode为例）

1）打开Finder，选中libLumiSDK.a、LHOAuthAIOT.h和LHFastLink.h文件，右键拖动到项目指定目录中。

2）在Xcode界面点击.xcodeproj文件，确保targets项目的Build Phases->Link Binary With Libraries下能看到刚刚添加的libLumiSDK.a文件。

![a文件](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/sdk/ios-sdk.png)

> 注意：在使用这些静态库函数之前，需添加“`import "xxxx.h"`“，才可正常使用。



#### 2、网关入网操作说明。

1）手机连接外网，调用授权接口（LHOAuthAIOT.h），进行授权。

| 参数     | 说明               |
| ------ | ---------------- |
| AppID  | 第三方应用ID，AppID    |
| AppKey | 第三方应用秘钥，AppKey   |
| OpenId | 通过OAuth授权获得的用户ID |

2）授权成功后，长按网关按钮至指示灯为黄色闪烁，且语音提示”等待连接中“，网关会创建一个以”lumi-gateway-xxxx“开头的热点。

3）手机Wi-Fi连接此网关热点，调用快联接口（LHFastLink.h），等待网关语音提示”连接成功“。

> 注意：网关成功入网需要一点时间，请耐心等待30s。

| 参数           | 是否必须 | 说明                |
| ------------ | ---- | ----------------- |
| ssid         | 是    | 网关需加入的外网网络Wi-Fi名称 |
| password     | 是    | 对应的Wi-Fi密码        |
| lang         | 否    | 语言设置，默认中文zh-CN    |
| postionId    | 否    | 位置ID              |
| positionType | 否    | 位置类型              |
| longitude    | 否    | 设备所在位置的经度         |
| latitude     | 否    | 设备所在位置的纬度         |

> 注意：1、系统不识别有特殊字符的Wi-Fi，且不支持5G频段的Wi-Fi。若Wi-Fi包含特殊字符，请先更换Wi-Fi名称或更换其他Wi-Fi尝试。
>
> 2、调用接口时，可能获得正确或错误的返回结果，可参考[云端开发手册](http://docs.opencloud.aqara.cn/development/cloud-development/#_14)中“返回码说明”章节排查错误。



## Demo示例

下载[IOS SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/aiot_sdk_fastlink_ios_v0.3.zip)，并解压，在lumi_iOS_SDK\LHSDKDemo\LHSDKDemo文件夹下，查看AppDelegate.m文件和ViewController.m文件。

步骤一：调用LHOAuthAIOT接口进行授权操作，示例代码：

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    
    [[LHOAuthAIOT sharedInstance] setAppId:@""];      //第三方应用ID，AppID
    [[LHOAuthAIOT sharedInstance] setAppKey:@""];     //第三方应用秘钥，AppKey
    [[LHOAuthAIOT sharedInstance] setOpenId:@""];     //通过OAuth授权获得的用户ID
    
    [[LHOAuthAIOT sharedInstance] registerAIOP:^(NSDictionary *result) {
        if ([[result objectForKey:@"code"] integerValue] == 200) {
            NSLog(@"success");
        }else {
            NSLog(@"failure");
        }
    }];
    return YES;
}
```

步骤二：调用LHFastLink接口进行快联操作，示例代码：

```
-(void)connect
{
    //可选参数设置
    [[LHFastLink sharedInstance] setLongitude:@"113.9884345186"];
    [[LHFastLink sharedInstance] setLatitude:@"22.5913863688524"];
    [[LHFastLink sharedInstance] setLang:@"zh-CN"];
    
    [[LHFastLink sharedInstance] cnnGateway:@"APP-TEST" withPwd:@"00000000"];  //当前手机所连接的外网Wi-Fi的ssid和password
}
```