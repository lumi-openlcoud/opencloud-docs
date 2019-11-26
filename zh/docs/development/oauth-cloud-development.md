# 云端开发手册


## 首页

### 概述

AIOT是提供设备管理、数据采集与远程控制等服务的物联网云平台。通过智能设备采集的数据存储在AIOT云端数据库中，比如：功率、温湿度、开关状态、门窗状态等。同时，AIOT云端可以通过下发控制命令来远程控制设备。

为了满足开发者的需求，AIOT开放平台提供云云对接方式。通过Open API和消息推送服务，可以从云端采集数据和远程控制设备。目前提供如下接口：

- 位置管理
- 设备管理
- 场景管理
- 资源管理
- 联动管理

另外，AIOT开放平台提供了消息推送服务，支持把设备上报的实时数据推送到第三方服务器，满足了开发者对数据实时性的要求。


### 开发流程

一个好的软件开发流程加上一个好的软件系统架构，会给软件开发带来事半功倍的效果。对于云对接方式，可按照以下流程进行开发。

![应用开发流程](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/oauth-v1.0.png)

一个稳定和高效的应用需要考虑API调用性能、消息处理性能、Access-Token的高可用性等方面，建议参考如下架构图。

![应用参考架构图](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/application-reference-architecture.png)

如果开发者还不熟悉API的调用方法，推荐使用Web API测试工具[Postman](https://www.getpostman.com/)。

> 注意：使用Postman时请关闭SSL验证（SSL certifacate verification）。


## 创建应用

登录[AIOT开放平台](https://opencloud.aqara.cn/)，在右上角点击进入“控制台”。选择“应用管理”-“新建应用”，“应用类型”选择“OAuth2授权接入”，上传应用图标、输入应用名称、行业类型、简介等信息，完成创建。应用审核后，系统会自动分配**Appid**和**Appkey**。


## 资源权限

资源是指由设备产生的数据，包括设备的设置参数与实时状态。一个设备具有多个资源，不同的资源表示不同含义的数据。例如，开关状态（plug_status）是智能插座的一个资源，表示智能插座当前是否通电。通过访问该资源，开发者可以查询插座的当前状态和远程开关插座。

登录[AIOT开放平台](https://opencloud.aqara.cn/)，进入控制台，选择“设备资源列表”，即可查看当前已开放的设备资源列表。

## API接口

API是AIOT开放平台对外提供数据的接口，用于查询、控制设备和配置自动化、场景等信息。目前开放的API接口默认为已授权状态。

登录[AIOT开放平台](https://opencloud.aqara.cn/)，进入控制台，展开APi参考菜单栏，选择授权接入API，即可查看接口介绍和进行接口调试。

##添加设备

添加设备目前有两种方式：使用Aaqara Home APP添加设备 和 使用SDK添加设备。

**使用Aaqara Home APP添加设备**

在应用商店或Apple Store搜索“Aaqara Home”，下载安装后，选择对应的服务器，注册Aqara账号，也可以直接登录AIOT开放平台账号。
登陆Aqara账号后，可在APP上按照提示先添加网关设备，再添加子设备。

**使用SDK添加设备**

下载设备入网SDK添加网关设备，详细操作请参考[SDK开发手册](http://docs.opencloud.aqara.com/development/sdk-v1.0/)。


## 账号授权

只有获得AIOT账号的授权许可，第三方应用才能访问与控制该账号下的设备，同时AIOT开放平台才会把设备消息推送到第三方服务器。


### OAuth2.0

OAuth2.0 是一个开放标准，允许用户让第三方应用访问该用户在某一网站上存储的私密资源（如用户信息、照片、视频、设备数据等），而无需将用户名和密码提供给第三方应用。

AIOT开放平台采用OAuth 2.0标准的授权码（authorization_code）模式，适用于拥有server端的应用。OAuth 2.0 授权流程简单、安全，时序图如下所示，完成授权流程后获得访问令牌（AccessToken）。此后，开发者使用访问令牌来调用接口，获取用户信息或操作用户的设备。

![Oauth2.0授权码模式时序图](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/auth-sequence-diagram.png)

OAuth 2.0 详细授权流程如下：

> 注意：本章节URL示例以“中国大陆地区（https://aiot-oauth2.aqara.cn/）”为例进行说明，若在其他地区，请参考[国家（地区码）](http://docs.opencloud.aqara.com/development/region_code)文档更改域名。

#### 步骤1 请求授权码

通过浏览器将用户重定向到AIOT OAuth 2.0服务。使用Aqara账号和密码成功登录后，会返回用户的授权码（code）。授权码的有效期为10分钟。

- **URL：**  https://aiot-oauth2.aqara.cn/authorize?client_id=xxx&response_type=code&redirect_uri=xxxx&state=xxx&theme=x
- **请求方式：** HTTP GET
- **请求参数** 

| 参数名           | 是否必须 | 描述                            |
| ------------- | ---- | ----------------------------- |
| client_id     | 是    | 第三方应用ID，AppID                 |
| response_type | 是    | 返回类型，按照OAuth 2.0 标准，取值为`code` |
| redirect_uri  | 是    | 第三方应用注册的重定向URI                |
| state         | 否    | 取值为任意字符串，认证服务器将原样返回该参数        |
| theme         | 否    | 页面主题，目前支持0、1两套主题，默认为主题0       |

- **返回说明**
```
GET  HTTP/1.1 302 Found
Location: https://redirect_uri?code=xxx&state=xxx
```

#### 步骤2 获取访问令牌
获得授权码后，访问以下URL，用授权码换取访问令牌（AccessToken）。

- **URL：** https://aiot-oauth2.aqara.cn/access_token
- **请求方式：** HTTP POST (application/x-www-form-urlencoded)
- **请求参数**

| 参数名           | 是否必须 | 描述                                     |
| ------------- | ---- | -------------------------------------- |
| client_id     | 是    | 第三方应用ID，AppID                          |
| client_secret | 是    | 第三方应用秘钥，AppKey                         |
| grant_type    | 是    | 根据OAuth 2.0 标准，取值为`authorization_code` |
| code          | 是    | 上一步请求获得的授权码                            |
| redirect_uri  | 是    | 上一步请求中设置的redirect_uri参数                |

- **返回参数**

| 参数名           | 描述                         |
| ------------- | -------------------------- |
| access_token  | 访问令牌，第三方应用访问AIOT开放服务的凭证    |
| expires_in    | 访问令牌的剩余有效时间，单位为秒           |
| token_type    | 根据OAuth 2.0 标准，取值为`bearer` |
| openId        | 授权用户的唯一标识                  |
| refresh_token | 刷新令牌，用于刷新访问令牌，有效期为30天      |
| state         | 取值为任意字符串，认证服务器将原样返回该参数     |

- **返回示例**
```
{
    "access_token": "xxxxx", 
    "expires_in": 7200, 
    "token_type": "bearer", 
    "openId": "xxx", 
    "refresh_token": "xxxxx", 
    "state": "abcde"
}
```


#### 步骤3 刷新访问令牌
在access_token过期前使用refresh_token进行刷新，获取新的access_token，可设置定时循环刷新。

- **URL：** https://aiot-oauth2.aqara.cn/access_token
- **请求方式：** HTTP POST (application/x-www-form-urlencoded)
- **请求参数**

| 参数名           | 是否必须 | 描述                                |
| ------------- | ---- | --------------------------------- |
| client_id     | 是    | 第三方应用ID，AppID                     |
| client_secret | 是    | 第三方应用秘钥，AppKey                    |
| grant_type    | 是    | 根据OAuth 2.0 标准，取值为`refresh_token` |
| refresh_token | 是    | 上一步请求获得的刷新令牌                      |

- **返回参数**

| 参数名           | 描述                         |
| ------------- | -------------------------- |
| access_token  | 访问令牌，第三方应用访问AIOT开放服务的凭证    |
| expires_in    | 访问令牌的剩余有效时间，单位为秒           |
| token_type    | 根据OAuth 2.0 标准，取值为`bearer` |
| openId        | 授权用户的唯一标识                  |
| refresh_token | 新的刷新令牌，有效期30天，旧的刷新令牌立即作废         |
| state         | 取值为任意字符串，认证服务器将原样返回该参数     |

- **返回示例**
```
{
    "access_token": "xxxxx", 
    "expires_in": 7200, 
    "token_type": "bearer", 
    "openId": "xxx", 
    "refresh_token": "xxxxx", 
    "state": "abcde"
}
```


## API调用

### API调用说明

1. 请求URL格式：https:// + 域名 + 3rd + API，如：https://aiot-open-3rd.aqara.cn/3rd/v1.0/open/device/query
2. 为了保证数据传输的安全，AIOT开放平台对外提供的API均采用HTTPS协议
3. 接口的请求body和返回结果都采用**JSON格式**。


### 请求head参数说明

| 名称             | 类型     | 必填                       | 描述 |
| -------------- | ------ | ------------------------ | ---- |
| Appid      | String | 是 | 第三方应用的Appid   |
| Sign | String | 是 | 签名   |
| Accesstoken      | String | 是 | 通过OAuth授权获取的访问Token   |
| Time  | Long  | 是 | 系统当前时间戳，单位毫秒   |
| Content-Type | application/json | 是 | 返回结果采用JSON格式，如：application/json |

Sign生成说明：

1）head请求的参数先按照ASSIC码做排序，然后进行拼接；

   拼接方式：key1=value1&key2=value2

   例如：accesstoken=xxx&appid=xxx&time=xxx

2）对第一步产生的字符串，全部小写处理；

3）对第二步产生的字符串拼接Appkey；

   由AIOT开放平台生成的Appid对应的Appkey的值进行拼接；

   例如：accesstoken=xxx&appid=xxx&time=xxx&(Appkey的value值)

4）最后对第三步产生的字符做MD5（32位），生成的数即为Sign的值（小写）。


### 调用示例

查询设备列表，调用方法如下：

- 请求URL：https://aiot-open-3rd.aqara.cn/3rd/v1.0/open/device/query
- 请求方式： HTTP POST （application/json）
- 请求header示例

| Key          | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| Appid        | 应用的唯一标识                                               |
| Sign         | 签名，拼接方式：accesstoken=xxx&appid=xxx&time=xxx&(Appkey的value值) |
| Accesstoken  | 通过OAuth授权获得的访问令牌                                  |
| Time         | 系统当前时间戳，单位毫秒                                     |
| Content-Type | 返回结果采用JSON格式，如：application/json                   |

- 返回结果示例
```
  {
    "result": {
        "bindDate": "2017-11-13",
        "chipVersion": "",
        "bindTime": "22:35:18",
        "name": "卧室红外光照传感器",
        "model": "lumi.sensor_motion.xxx",
        "isOnline": 1,
        "firmwareVersion": "1",
        "did": "lumi.158d0001123454",
        "parentId": "lumi.158d00011234a9"
    },
    "code": 0,
    "isBytesData": 0,
    "requestId": "1234"
  }
```


## 消息推送

消息推送目前支持推送设备属性消息和事件消息。用户需提供服务器URL来接收设备属性消息和事件消息。AIOT使用HTTPS请求，将设备属性和事件消息发送到第三方的服务器URL。

- 设备属性消息：设备属性的变化消息，比如温度变化、功率变化等；
- 事件消息：设备的事件消息，比如设备在线离线、绑定解绑等。

如需接收设备消息，请按如下步骤启用消息推送功能：

1. 填写服务器配置；
2. 验证服务器地址；
3. 调用订阅资源接口（v1.0/open/subscriber/resource），订阅需要推送的资源；
4. 根据消息格式实现业务逻辑。

### 服务器配置

1、登录AIOT开放平台，在右上角点击进入“控制台”；

2、选择“应用管理”-“消息推送”；

3、在服务器配置中，输入“URL”，选择“消息加解密方式”和是否开启“事件消息”推送。

   **URL**：用于接收设备属性数据和事件消息的服务器URL；

   **消息加解密方式**：支持明文模式和安全模式；

   **事件消息**：是否开启设备事件消息，如设备在线离线等消息。

4、单击“提交”，完成配置。


### 验证服务器

**明文模式**

在明文模式下，消息体以明文形式传输，数据安全性低，但服务器验证方法很简单。开发者保存服务器配置后，AIOT服务器会发送一个POST请求到开发者设置的服务器地址（URL），请求将携带一个参数echostr（JSON格式），它是一个随机的字符串。如果第三方服务器收到该请求，请原样返回echostr参数内容，则验证服务器成功，否则验证失败。

发送报文格式：
```
{
	"echostr": "jdlfialjf8i"
}
```

返回报文格式：
```
{
	"code": 0,
	"result": "jdlfialjf8i"
}
```

**安全模式**

在安全模式下，AIOT服务器会发送一个POST请求到开发者设置的服务器地址（URL），请求head参数采用签名校验，用于验证推送数据是否来源于AIOT服务器。
Body加密方式：body数据体采用AES/CBC/PKCS5Padding加密，密钥长度为16byte(128bit)，密钥和偏移量均为Appkey的前16位。

请求head参数说明

| 名称             | 类型     | 必填                       | 描述 |
| -------------- | ------ | ------------------------ | ---- |
| Appid      | String | 是 | 第三方应用的Appid   |
| Sign | String | 是 | 签名   |
| Time  | Long  | 是 | 系统当前时间戳，单位毫秒   |

Sign生成说明：

1）head请求的参数先按照ASSIC码做排序，然后进行拼接；

   拼接方式：key1=value1&key2=value2

   例如：appid=xxx&time=xxx

2）对第一步产生的字符串，全部小写处理；

3）对第二步产生的字符串拼接Appkey；

   由AIOT开放平台生成的Appid对应的Appkey的值进行拼接；

   例如：appid=xxx&time=xxx&(Appkey的value值)

4）最后对第三步产生的字符做MD5（32位），生成的数即为Sign的值（小写）。

### 消息格式

目前，AIOT开放平台支持以下两类消息：
- 资源消息：资源的变化消息，比如温度变化、功率变化等；
- 设备消息：设备的事件消息，比如设备在线离线、绑定解绑等。

**资源消息**

| 参数    | 说明                                                   |
| ------- | ------------------------------------------------------ |
| msgType | 消息类型，取值为`resource`                             |
| time    | 时间戳，单位：毫秒                                     |
| attr    | 资源别名，资源别名的含义请参考“控制台-设备资源管理”页面 |
| value   | 资源属性值                                          |
| did     | 设备ID                                                 |
| model  | 设备模型值                                   |
| attach  | 附加信息-推送时带上                                    |


消息示例
```
{
    "msgType": "resource", 
    "data": [
        {
            "time": "1503556533000", 
            "attr": "load_power", 
            "value": "3.93", 
            "did": "lumi.xxxxxxxxxx",
            "model": "xxxxxxxxxxx",
            "attach": "xxxx"
        }
    ]
}
```



**设备消息**

| 参数     | 说明                                     |
| -------- | ---------------------------------------- |
| msgType  | 消息类型，取值为`device`                 |
| openId   | 用户唯一标识，用于区分来自于哪个账号            |
| name     | 设备名称                                 |
| model    | 设备模型值                                 |
| time     | 时间戳，单位：毫秒                       |
| event    | 事件类型，详细参数说明见下表             |
| did      | 设备ID                                   |
| parentId | 父设备（网关）ID，如果是网关，该字段为空 |

目前支持推送的事件类型如下：

| 事件类型           | 描述         |
| :----------------- | :----------- |
| GW\_BIND           | 网关入网     |
| GW\_UN\_BIND       | 网关解绑     |
| GW\_ONLINE         | 网关在线     |
| GW\_OFFLINE        | 网关离线     |
| SUB\_DEV\_BIND     | 子设备入网   |
| SUB\_DEV\_UN\_BIND | 子设备解绑   |
| SUB\_DEV\_ONLINE   | 子设备在线   |
| SUB\_DEV\_OFFLINE  | 子设备离线   |
| DEV_INFO_CHANGED   | 设备名称变更 |

```
{
    "msgType": "device", 
    "data": [{
        "openId": "xxxxxx", 
        "name": "空调伴侣", 
        "model": "lumi.acpartner.xxx", 
        "time": 1503560767000, 
        "event": "DEV_INFO_CHANGED", 
        "did": "lumi.xxxxxxx", 
        "parentId": ""
    }]
}
```



## 返回码说明

第三方应用每次调用接口时，可能获得正确或错误的返回码，开发者可以根据返回码信息调试接口以及排查错误。

| 返回码  | 描述                              | 说明              |
| ---- | ------------------------------- | --------------- |
| 0    | Success                         | 成功              |
| 100  | Timeout                         | 超时              |
| 104  | Server busy                     | 服务器繁忙           |
| 105  | Data package has expired        | 数据包过期           |
| 106  | Invalid sign                    | 无效的签名           |
| 107  | Illegal appKey                  | 无效的Appkey       |
| 108  | Token has expired               | Token过期         |
| 109  | Token is absence                | Token丢失         |
| 301  | Request path error              | 请求路径错误          |
| 302  | Params error                    | 参数错误            |
| 303  | Request params type error       | 请求参数类型错误        |
| 304  | Request method not support      | 请求方式不支持         |
| 305  | Header Params error             | 头部参数错误          |
| 701  | Position not exist              | 位置不存在           |
| 703  | Position name duplication       | 位置名称重复          |
| 706  | Device permission denied        | 设备拒绝访问或不存在      |
| 707  | Ifttt permission denied         | 自动化拒绝访问或不存在     |
| 708  | Scene permission denied         | 场景拒绝访问或不存在      |
| 713  | Position not real position      | 不是顶级位置          |
| 1003 | Resource attr illegal           | 资源attr不合法       |
| 1005 | Resource subscribe not register | 资源订阅未注册         |
| 1206 | Delete local linkage failed     | 删除本地自动化失败       |
| 1207 | Operation failed                | 操作失败            |
| 2001 | Get developer list error        | 获取开发者列表错误       |
| 2002 | Appid or Appkey illegal         | appid或appkey不合法 |
| 2003 | AuthCode incorrect              | authcode错误      |
| 2004 | AccessToken incorrect           | access token错误  |
| 2005 | AccessToken expired             | access token过期  |
| 2006 | RefreshToken incorrect          | refresh token错误 |
| 2007 | RefreshToken expired            | refresh token过期 |
| 2008 | Permission denied               | 拒绝访问            |
| 2009 | Invalid OpenId                  | 无效的openid       |
| 2010 | Unauthorized user               | 未授权账号           |
| 2011 | The query result is empty       | 查询结果为空          |
| 2012 | Invalid apply                   | 无效的申请           |
| 2013 | Developer Permission denied     | 开发者拒绝访问         |
| 2014 | Resource Permission denied      | 资源拒绝访问          |
| 2015 | subscriber faild                | 订阅失败            |

## 资源定义

云端开发的完整资源请查看AIOT开放平台的“资源授权”页面，以下为部分特殊资源的定义说明。

资源：**ac_state**

空调命令压缩格式（4 bytes）: （二进制）

|0 1 2 3|4 5 6 7|8 9 10 11|12 13 14 15|16 17 18 19 20 21 22 23|24 25 26 27 28 29 30 31|

| 位置        | 值                                        | 描述      |
| --------- | ---------------------------------------- | ------- |
| [0 ~3]    | 0: off; 1: on; 2: toggle; E: circle; F: invalid; else: reserve | 开关      |
| [4 ~ 7]   | 0: heat; 1: cool; 2: auto; 3: dry; 4: wind; E: circle; F: invalid; else: reserve | 模式      |
| [8 ~ 11]  | 0: low; 1: middle; 2: high; 3: auto; E: circle; F: invalid; else: reserve | 风速      |
| [12 ~ 13] | 0: horizontal; 1: vertical; 2: circle; 3: invalid; | 风向      |
| [14 ~ 15] | 0: swing; 1: fix; 2: circle; 3: invalid; | 扫风      |
| [16 ~ 23] | 0 ~ 240; 243: up; 244: down; FF: invalid | 温度      |
| [24]      | 默认为0                                     | 扩展位     |
| [25]      | 默认为0                                     | 是否为压缩码  |
| [26]      | 默认为0                                     | LED显示   |
| [27]      | 0: 开关命令; 1: 非开关命令                        | 是否为开关命令 |
| [28 ~ 31] | 00: 无状态; 01: 有状态; 02: 协议; 03: 推荐场景; 04: 半状态; 11: 忽略 | 空调类型    |

> 注意：
>
> - [16-23]温度数值0~240为十进制，如果设置为25度，那么二进制为00011001 。
> - "ac_state"值必须采用10进制数。
> - 在控制空调时，模式、风速和风向不要设置为“F（invaild）”，可能出现无法识别导致控制失败的情况。建议在设置ac_state值之前，可以先通过/open/resource/query接口查询ac_state当前值，然后根据需求修改对应值。

例如：设置为“开空调，模式为制冷，风速为低档，风向为水平，摆动扫风，温度为25度” 
对应的二进制数为：00010001000000000001100100000001，二进制数转换为十进制，则ac_state值为“285219073”。 



资源：**cfg_param**



| 参数             | 类型     | 描述                       | 可写可读 |
| -------------- | ------ | ------------------------ | ---- |
| WriteMask      | uint16 | 标志要写的成员是哪一个，需要写的成员的对应位为1 | NA   |
| bPosLimitState | bool_t | 电机是否已经设置了行程              | RW   |
| bPolarity      | bool_t | 电机旋转的极性                  | RW   |
| u8MotorStatus  | uint8  | 电机当前的状态                  | RO   |
| bManualEnabled | bool_t | 电机是否使能了手拉功能              | RW   |
| u8TransTime    | uint8  | 窗帘从起点允许到终点的使用时间，单位s      | RO   |











