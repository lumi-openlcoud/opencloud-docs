# SDK Development Documentation

The purpose of this document is to explain how to use LumiSDK for the development of bind a gateway device, which is only used for Android mobile phone APP applications.



## Preparation

1. Login in [AIOT Open Cloud Platform](https://opencloud.aqara.cn/). After create an application, you can get “AppId” and “AppKey” from “Application Management" - "Application Overview" page.
2. Get openID and positopnId. From detail, see “OAuth 2.0” chapter in [Cloud Development Manual](http://docs.opencloud.aqara.com/en/development/cloud-development/#oauth20).
3. Download and decompress [SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/SDK-Build-2019-03-15.zip)。
4. Download and install Andriod and IOS integrated development environment.




## Steps

#### 1. Configure Andriod project (Take Android Studio as an example)

1) Create Android project. choose File->New->New Module->Import .JAR/.AAR Package from the main menu, select the path of LumiSDK.aar, click "Finish".

2) Rebuild project, the icon of LumiSDK Module will become a folder with a cup, and create a .iml folder automatically.

![模块](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/sdk/lumisdk.png)

3) Choose File->Project Structure->app->Dependencies from the main menu, click "+" on the upper right corner, select "Module Dependency", add LumiSDK module.

![依赖](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/sdk/dependencies.png)

4) Configure src/main/AndroidManifest.xml to add networking permissions for project. If not, the authorization operation cannot be completed.

```
<uses-permission android:name="android.permission.INTERNET"/>
```



#### 2. Bind gateway

1) Telephone connect Wi-Fi, call the authorization interface(aiotAuth) to authorize and return the authorization result.

Authorization interface: **LumiSDK.aiotAuth(appID,appKey,openID, new CallBack())**

| Parameter | Description                         |
| --------- | ----------------------------------- |
| appID     | Third-party application ID, AppID   |
| appKey    | Third-party application key, AppKey |
| openID    | Authorized user's unique identifier |
| CallBack  | Callback function                   |

Callback function code is as follows:

```
package lumisdk;

public interface CallBack {
    void onFaied(long var1, String var3);
    void onSuccess(String var1);
}
```



2) After the authorization is successful, press and hold the gateway button until yellow light keeps flashing and gateway voice prompt "waiting for connection", then gateway will create a hot spot named ”lumi-gateway-xxxx“.

3) Phone connect gateway hot spot, call fastlink interface(gatewayFastLink) , and wait gateway voice prompt "connect successfully".

> Note: It takes a time to bind gateway, please wait 30s.

Fastlink interface: **Lumisdk.gatewayFastLink(params.toString(), new CallBack()**

- "param" using the **JSON format**, including cid, ssid, passwd, positionId, country_domain.


```
{
  cid: xxxxxxxxxxxxxxxx, 
  ssid：test_123, 
  passwd: 12345678, 
  positionId: xxxxxxxxxxxxxxxxxx 
  country_domain: xxxx
}
```

Detailed sub-parameters are described in the following table.

| Parameter      | Is required? | Description                                                  |
| -------------- | ------------ | ------------------------------------------------------------ |
| cId            | No           | Telephone's unique identifier. If not set, you will not receive any messages. |
| ssid           | Yes          | Wi-Fi name                                                   |
| passwd         | Yes          | Wi-Fi password                                               |
| positionId     | Yes          | Position ID, you can query positionId by calling API.        |
| country_domain | Yes          | Domain.                                                      |



> Note:
>
> 1. The system can't identify Wi-Fi that contains special characteristics. If a Wi-Fi contains special characteristics, please change the name and then reconnect. If the connection failed again, please reset the device or change the Wi-Fi and reconnect. If the Wi-Fi is unstable, it is suggested to use a 4G phone as the Wi-Fi hotspot.
> 2. The system doesn't support 5G Wi-Fi.
> 3. Because the network requests in the SDK are synchronous, the call must be invoked in a non-UI thread. For details, refer to DEMO.
> 4. When you call the interface, you may get the correct or incorrect return result. For the detail about error code, see “Description of Return codes” chapter in [Cloud Development Manual](http://docs.opencloud.aqara.com/en/development/cloud-development/#_14).
