# IOS SDK Development Documentation

The purpose of this document is to explain how to use LumiSDK for the development of bind a gateway device. (OC version)



## Preparation

1. Login in [AIOT Open Cloud Platform](https://opencloud.aqara.cn/). After create an application, you can get “AppId” and “AppKey” from “Application Management" - "Application Overview" page.
2. Get openID. From detail, see “OAuth 2.0” chapter in [Cloud Development Manual](http://docs.opencloud.aqara.cn/development/cloud-development/#oauth20).
3. Download and decompress [IOS SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/aiot_sdk_fastlink_ios_v0.3.zip), you can find the main SDK files in LHSDKLib folder: libLumiSDK.a, LHOAuthAIOT.h and LHFastLink.h; and see Demo in LHSDKDemo folder.
   - **libLumiSDK.a**: Static link library.
   - **LHOAuthAIOT.h**: Authorization interface. (The interface authorization needs to connect network. It may take a while to wait for it. It is recommended to use it in the AppDelegate.)
   - **LHFastLink.h**: Fastlink interface.
4. Download and install Xcode or other IOS integrated development environment.



## Steps

#### 1. Configure IOS project, add static link library. (Take Xcode as an example)

1) Open Finder and select libLumiSDK.a, LHOAuthAIOT.h and LHFastLink.h file, right-drag into the project specified directory.

2) Click .xcodeproj file in Xcode, make sure that the libLumiSDK.a file you just added is visible under Build Phases->Link Binary With Libraries in the targets project. 

![a文件](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/sdk/ios-sdk.png)

> Note: Before using these static library functions, add "import "xxxx.h" to work properly.



#### 2. Bind gateway

1) Telephone connect Wi-Fi, call the authorization interface(LHOAuthAIOT.h) to authorize and return the authorization result.

| Parameter | Description                         |
| --------- | ----------------------------------- |
| AppID     | Third-party application ID, AppID   |
| AppKey    | Third-party application key, AppKey |
| OpenId    | Authorized user's unique identifier |

2) After the authorization is successful, press and hold the gateway button until yellow light keeps flashing and gateway voice prompt "waiting for connection", then gateway will create a hot spot named ”lumi-gateway-xxxx“.

3) Phone connect gateway hot spot, call fastlink interface(LHFastLink.h) , and wait gateway voice prompt "connect successfully".

> Note: It takes a time to bind gateway, please wait 30s.

| Parameter    | Is required? | Description                             |
| ------------ | ------------ | --------------------------------------- |
| ssid         | Yes          | Wi-Fi name                              |
| password     | Yes          | Wi-Fi password                          |
| lang         | No           | Language. The default is zh-CN.         |
| postionId    | Yes          | Position ID                             |
| positionType | No           | Position Type, such as home, room.      |
| longitude    | No           | The longitude of the device's location. |
| latitude     | No           | The latitude of the device's location.  |

> Note:
>
> 1. The system can't identify Wi-Fi that contains special characteristics. If a Wi-Fi contains special characteristics, please change the name and then reconnect. If the connection failed again, please reset the device or change the Wi-Fi and reconnect. If the Wi-Fi is unstable, it is suggested to use a 4G phone as the Wi-Fi hotspot.
> 2. The system doesn't support 5G Wi-Fi.
> 3. When you call the interface, you may get the correct or incorrect return result. For the detail about error code, see “Description of Return codes” chapter in [Cloud Development Manual](http://docs.opencloud.aqara.cn/development/cloud-development/#_14).



## Demo

Download and decompress [IOS SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/aiot_sdk_fastlink_ios_v0.3.zip). In the lumi_iOS_SDK\LHSDKDemo\LHSDKDemo folder, view AppDelegate.m file and ViewController.m file.

Step 1: Call the LHOAuthAIOT interface for authorization.

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    
    [[LHOAuthAIOT sharedInstance] setAppId:@""];      //Third-party application ID, AppID
    [[LHOAuthAIOT sharedInstance] setAppKey:@""];     //Third-party application key, AppKey
    [[LHOAuthAIOT sharedInstance] setOpenId:@""];     //Authorized user's unique identifier
    
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

Step 2: Call LHFastLink interface for binding gateway operation.

```
-(void)connect
{
    //Optional parameter settings
    [[LHFastLink sharedInstance] setLongitude:@"113.9884345186"];
    [[LHFastLink sharedInstance] setLatitude:@"22.5913863688524"];
    [[LHFastLink sharedInstance] setLang:@"zh-CN"];
    
    [[LHFastLink sharedInstance] cnnGateway:@"APP-TEST" withPwd:@"00000000"];  
    //ssid and password
}
```