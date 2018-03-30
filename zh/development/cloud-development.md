# 云端开发手册


## 首页

### 概述

AIOT是一个物联网云平台，提供用户管理、设备管理、数据采集与远程控制等服务。通过智能设备采集的数据存储在AIOT云端数据库中，比如：功率、温湿度、开关状态、门窗状态等。同时，AIOT云端可以通过下发控制命令来远程控制设备。那开发者如何获得采集数据呢？又如何远程控制自己的设备呢？

为了满足这些第三方开发者的需求，AIOT开放平台提供了云端对接方式。通过Open API和消息推送服务，开发者可以从云端采集数据和远程控制设备，从而开发丰富的物联网应用。

AIOT开放平台提供了非常完善的接口，包括：

- 用户管理
- 位置管理
- 设备管理
- 场景管理
- 资源管理
- 远程控制

另外，AIOT开放平台提供了消息推送服务，支持把设备上报的实时数据推送到第三方服务器，满足了开发者对数据实时性的要求。

下面，本手册将介绍下如何基于云端对接方式开发第三方应用，包括应用的创建、配置和开发等。

### 写在前面

一个好的软件开发流程加上一个好的软件系统架构，会给软件开发带来事半功倍的效果。因此，为了帮助开发者开发得更加顺畅，在前面交代些重要的事。如果你是一个经验丰富的开发者，可以跳过这一小节。

在应用正式开发前，开发者需要做些准备工作，可参考如下流程。

![应用开发流程](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/application-development-process.png)

一个稳定和高效的应用需要考虑API调用性能、消息处理性能、Access-Token的高可用性等方面，建议参考如下架构图。

![应用参考架构图](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/application-reference-architecture.png)

如果开发者还不熟悉API的调用方法，推荐使用Web API测试工具[Postman](https://www.getpostman.com/)。Postman是一款优秀的Web API和HTTP请求调试工具，功能强大，界面清晰，操作方便快捷。Postman能够帮助开发者测试AIOT Open API，从而快速掌握API的调用方法。

> 注意：使用Postman时请关闭SSL验证（SSL certifacate verification）。

## 应用创建与配置

### 创建应用

登录[AIOT开放平台](https://opencloud.aqara.cn/)，单击“新建应用”。创建应用时，开发者需要设置应用的相关参数，包括：

- 应用名称
- 行业类型
- 简介

创建应用成功后平台自动分配AppID和AppKey，其中AppID是应用的唯一标识，而AppKey是应用的秘钥。AppID和AppKey非常重要，在应用开发过程中会被频繁使用，比如：在调用AIOT开放平台Open API时，开发者需要在请求Header中加上AppID和AppKey作为校验参数。

> 注意：请开发者务必妥善保管AppKey，以防泄露，同时建议定期重置AppKey。

### 申请资源权限

资源是一种抽象的说法，在这里指由设备产生的数据，包括设备的设置参数与实时状态。一个设备具有多个资源，不同的资源表示不同含义的数据。例如，开关状态（plug_status）是智能插座的一个资源，表示智能插座当前是否通电。通过访问该资源，开发者可以查询插座的当前状态和远程开关插座。

访问资源之前，开发者需要先按级别申请资源的权限。资源级别分为1级和2级，1级表示常用的资源，2级表示不常用的资源。获得某个类型设备的1级授权后，开发者可以在应用中访问该类型设备的所有常用资源。默认情况下，应用会自动分配所有类型设备的1级资源权限。

如果开发者需要申请2级资源授权，请访问“应用管理”页面，然后切换到“资源授权”页，如下图所示。

- 单击每一行右侧的“申请资源”按钮，申请对应设备类型的资源。
- 选中左侧多个勾选框，然后单击右上角的“批量申请”按钮，可以批量申请多个设备类型的资源。

申请单提交后需等待1到3个工作日，审核结果会以短信或邮件的方式通知到开发者。

![资源授权页](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/apply-resource.png)

### 申请API权限

API是AIOT开放平台对外提供数据的接口，也是开发者查询与控制设备的主要方式。在开发应用前，开发者请根据自己的需求来申请API的权限。默认情况下，应用会自动分配常用API的权限。

如果开发者需要申请API，请访问“应用管理”页面，然后切换到“API访问”页，如下图所示。

- 单击每一行右侧的“申请”按钮，申请对应API的访问权限。
- 单击右上角的“批量申请”按钮，然后按功能批量申请多个API的访问权限。

申请单提交后需等待1到3个工作日，审核结果会以短信或邮件的方式通知到开发者。

![API访问页](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/apply-api.png)

## 账号授权

在设备入网后，设备绑定到唯一的AIOT账号，也就是Aqara APP的登录账号。只有获得AIOT账号的授权许可，第三方应用才能访问与控制该账号下的设备，同时AIOT平台才会把设备消息推送到第三方服务器。

> 说明：在应用商店或Apple Store搜索“Aqara”，下载安装后注册Aaqra APP账号。

目前，AIOT开放平台只提供一种授权方式：OAuth 2.0，后续会提供更多的授权方式。

### OAuth2.0

OAuth2.0 是一个开放标准，允许用户让第三方应用访问该用户在某一网站（或物联网平台）上存储的私密资源（如用户信息、照片、视频、设备数据等），而无需将用户名和密码提供给第三方应用。如果您想对OAuth2.0开放标准进行扩展阅读，请参考 [理解OAuth2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html) | [OAuth标准（英文）](https://oauth.net/2/)。

AIOT开放平台采用OAuth 2.0标准的授权码（authorization_code）模式，适用于拥有server端的应用。OAuth 2.0 授权流程简单、安全，时序图如下所示，完成授权流程后获得访问令牌（AccessToken）。此后，开发者使用访问令牌来调用接口，获取用户信息或操作用户的设备。

![Oauth2.0授权码模式时序图](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/auth-sequence-diagram.png)

OAuth 2.0 详细授权流程如下：

#### 步骤1 请求授权码

首先，第三方应用需要通过浏览器将用户重定向到AIOT OAuth 2.0服务。使用Aqara APP账号登录成功后，AIOT开放平台会返回用户的授权码（code）。授权码的有效期为10分钟，请在10分钟内完成后续流程。

- **URL：**  https://aiot-oauth2.aqara.cn/authorize?client_id=xxx&response_type=code&redirect_uri=xxxx&state=xxx&theme=x
- **请求方式：** HTTP GET
- **请求参数** 

| 参数名           | 是否必须 | 描述                            |
| ------------- | ---- | ----------------------------- |
| client_id     | 是    | 第三方应用ID，AppID                 |
| response_type | 是    | 返回类型，按照OAuth 2.0 标准，取值为`code` |
| redirect_uri  | 是    | 第三方应用注册的重定向URI                |
| state         | 否    | 取值为任意字符串，认证服务器将原样返回该参数        |
| theme         | 否    | 页面主题，目前支持0、1、2三套主题，默认为主题0     |

- **返回说明**
```
GET  HTTP/1.1 302 Found
Location: https://redirect_uri?code=xxx&state=xxx
```

#### 步骤2 获取访问令牌
获得授权码后，第三方应用访问以下URL，用授权码换取访问令牌（AccessToken）。

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

- **返回示例（状态码200）**
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
- **返回参数**

| 参数名           | 描述                         |
| ------------- | -------------------------- |
| access_token  | 访问令牌，第三方应用访问AIOT开放服务的凭证    |
| expires_in    | 访问令牌的剩余有效时间，单位为秒           |
| token_type    | 根据OAuth 2.0 标准，取值为`bearer` |
| openId        | 授权用户的唯一标识                  |
| refresh_token | 刷新令牌，用于刷新访问令牌，有效期为30天      |
| state         | 取值为任意字符串，认证服务器将原样返回该参数     |

#### 步骤3 刷新访问令牌

由于访问令牌的有效期只有2个小时，所以开发者需要在访问令牌过期前使用刷新令牌进行刷新，建议的刷新周期为1个半小时。

- **URL：** https://aiot-oauth2.aqara.cn/refresh_token
- **请求方式：** HTTP POST (application/x-www-form-urlencoded)
- **请求参数**

| 参数名           | 是否必须 | 描述                                |
| ------------- | ---- | --------------------------------- |
| client_id     | 是    | 第三方应用ID，AppID                     |
| client_secret | 是    | 第三方应用秘钥，AppKey                    |
| grant_type    | 是    | 根据OAuth 2.0 标准，取值为`refresh_token` |
| refresh_token | 是    | 上一步请求获得的刷新令牌                      |

- **返回示例（状态码200）**
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
- **返回参数**

| 参数名           | 描述                         |
| ------------- | -------------------------- |
| access_token  | 访问令牌，第三方应用访问AIOT开放服务的凭证    |
| expires_in    | 访问令牌的剩余有效时间，单位为秒           |
| token_type    | 根据OAuth 2.0 标准，取值为`bearer` |
| openId        | 授权用户的唯一标识                  |
| refresh_token | 新的刷新令牌，旧的刷新令牌立即作废          |
| state         | 取值为任意字符串，认证服务器将原样返回该参数     |

> 注意：如果刷新访问令牌时出现非正常返回的情况，请重试！

## API调用

### API调用规范

1. 为了保证数据传输的安全，AIOT开放平台对外提供的API均采用HTTPS协议，统一域名为：**https://rpc.opencloud.aqara.cn**。

2. OpenID 作为第三方应用对用户的唯一标识，是原AIOT账号加密后的结果。每个AIOT用户对每个第三方应用有一个唯一的OpenID。

3. 接口的请求body和返回结果都采用**JSON格式**，如果开发者采用其他格式，会提示“请求参数错误”。

4. 通过接口查询设备状态或控制设备时需要设置参数“资源别名”，不同资源的取值类型也不一样。所有资源的信息（别名、取值类型、含义等）请访问[AIOT开放平台](https://opencloud.aqara.cn)的“应用管理->资源授权”页面。

5. 所有功能的详细API定义请访问[AIOT开放平台](https://opencloud.aqara.cn)的“应用管理->API访问”页面。

### 调用示例

例如，通过调用接口查询一个设备的详细信息，调用方法如下：

- 请求URL：https://rpc.opencloud.aqara.cn/open/device/query

- 请求方式： HTTP POST （application/json）

- 请求header示例

| Key          | Value                            | 描述（可不填）          |
| ------------ | -------------------------------- | ---------------- |
| Appid        | 54a230103556040223478911         | 应用的唯一标识          |
| Appkey       | oT7kp77vzwxBn7siiXISamsPpvaTaWeZ | 应用的秘钥            |
| Openid       | Yb2bR2btJC6L8EmU5Z3jzh4oQsttie   | 通过OAuth授权获得的用户ID |
| Access-Token | 12db5a10c49fd289963dbc67a0d13ab5 | 通过OAuth授权获得的访问令牌 |
| Content-Type | application/json                 | 返回结果采用JSON格式     |

> 注意：在请求header时填写的Key和Value值，需注意字母大小写。

- 请求body示例

```
  {
  "openId": "Yb2bR2btJC6L8EmU5Z3jzh4oQsttie",
  "did": "lumi.158d00013fd654"
  }
```
> 说明：参数did表示设备ID，是设备的唯一标识。开发者可以通过调用接口，根据OpenID获得用户下所有设备的did。

- 返回结果示例
```
  {
    "result": {
        "bindDate": "2017-11-13",
        "chipVersion": "",
        "bindTime": "22:35:18",
        "name": "卧室红外光照传感器",
        "model": "lumi.sensor_motion.es2",
        "isOnline": 1,
        "firmwareVersion": "1",
        "did": "lumi.158d00013fd654",
        "parentId": "lumi.158d00010d65a9"
    },
    "code": 0,
    "isBytesData": 0,
    "requestId": "oHjXRtRdnm"
  }
```

## 消息推送

如果第三方应用需要接收设备消息，开发者需要按照如下步骤启用消息推送功能：

1. 填写服务器配置；
2. 验证服务器地址；
3. 按类型订阅消息；
4. 根据消息格式实现业务逻辑。

特别注意，第三方应用在正常接收消息后必须按照规定的格式返回JSON报文，格式如下：

```
{
	"code": 0|ErrorCode,
	"result": "自定义内容"
}
```

### 服务器配置

访问“应用管理->消息推送”页面，单击右上角的“编辑”按钮，填写配置信息：

1. **URL**：服务器地址，第三方服务器用来接收消息的接口URL；
2. **Token**：由开发者任意填写，用于生成签名；
3. **EncodingAESKey：**随机生成或由开发者手动填写，将用来对消息体进行加解密；
4. **消息加解密方式**：分为明文模式、兼容模式和安全模式，更改消息加解密方式后会立即生效，请开发者谨慎修改。

详细说明下消息加解密方式的区别：

- 明文模式：不对消息体进行加密，安全系数低；
- 兼容模式：消息体同时包含明文和密文，方便开发者调试和维护；
- 安全模式：消息体为纯密文，需要开发者加密和解密，安全系数高。

> 注意：目前仅支持“明文模式”，请开发者使用明文模式。

![消息服务器配置](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/message-subscribe.png)

### 验证服务器

**明文模式**

在明文模式下，服务器验证的方法很简单。开发者保存服务器配置后，AIOT服务器会发送一个POST请求到开发者设置的服务器地址（URL），请求将携带一个参数echostr（JSON格式），它是一个随机的字符串。如果第三方服务器收到该请求，请原样返回echostr参数内容，则验证服务器成功，否则验证失败。

发送报文格式：

```
{
	"echostr": "jdlfialjf8i"
}
```

返回报文格式如下：

```
{
	"code": 0,
	"result": "jdlfialjf8i"
}
```

**安全模式**

在安全模式下，服务器验证的方法变得复杂，和微信公众号平台类似。 开发者保存服务器配置后后，AIOT服务器将发送GET请求到填写的服务器地址（URL)，请求携带如下参数：

* **signature**：加密签名，signature结合了开发者填写的Token参数和请求中的timestamp参数、nonce参数；
* **timestamp**：时间戳；
* **nonce**：随机数；
* **echostr**：随机字符串。

若确认此次GET请求来自绿米服务器，请原样返回echostr参数内容，则验证服务器成功，否则验证失败。其中，加密/校验流程如下：

1. 将Token、timestamp、nonce三个参数进行字典序排序；
2. 将三个参数字符串拼接成一个字符串进行sha1加密；
3. 开发者获得加密后的字符串可与signature对比，标识该请求来源于AIOT服务器。

### 消息格式

目前，AIOT开放平台支持以下两类消息：
- 资源消息：资源的变化消息，比如温度变化、功率变化等；
- 设备消息：设备的事件消息，比如设备在线离线、绑定解绑等。

**资源消息**

```
{
    "msgType": "resource", 
    "data": [
        {
            "time": "1503556533", 
            "attr": "load_power", 
            "value": "3.93", 
            "did": "lumi.158d00011c1cee"
        }
    ]
}
```

| 参数      | 说明                            |
| ------- | ----------------------------- |
| msgType | 消息类型，取值为`resource`            |
| time    | 时间戳，单位：秒                      |
| attr    | 资源别名，资源别名的含义请参考“应用管理->资源授权”页面 |
| value   | 资源的最新值                        |
| did     | 设备ID                          |

**设备消息**

```
{
    "msgType": "device", 
    "data": {
        "openId": "GoeFrrL7mN9SsGRi8WJn4x4YnQpXTS", 
        "name": "空调伴侣", 
        "model": "lumi.acpartner.aq1", 
        "time": 1503560767, 
        "event": "DEV_INFO_CHANGED", 
        "did": "lumi.158d00010b4090", 
        "parentId": ""
    }
}
```

| 参数       | 说明                    |
| -------- | --------------------- |
| msgType  | 消息类型，取值为`device`      |
| openId   | 用户ID                  |
| name     | 设备名称                  |
| model    | 设备类型                  |
| time     | 时间戳，单位：秒              |
| event    | 事件类型，详细参数说明见下表        |
| did      | 设备ID                  |
| parentId | 父设备（网关）ID，如果是网关，该字段为空 |

| 事件类型               | 描述     |
| :----------------- | :----- |
| GW\_BIND           | 网关入网   |
| GW\_UN\_BIND       | 网关解绑   |
| GW\_ONLINE         | 网关在线   |
| GW\_OFFLINE        | 网关离线   |
| SUB\_DEV\_BIND     | 子设备入网  |
| SUB\_DEV\_UN\_BIND | 子设备解绑  |
| SUB\_DEV\_ONLINE   | 子设备在线  |
| SUB\_DEV\_OFFLINE  | 子设备离线  |
| DEV\_INFO\_CHANGED | 设备信息改变 |

## 返回码说明

第三方应用每次调用接口时，可能获得正确或错误的返回码，开发者可以根据返回码信息调试接口以及排查错误。

| 类别            | 返回码  | 表示                                       | 说明                          |
| ------------- | ---- | ---------------------------------------- | --------------------------- |
| SUCCESS       | 0    | SUCCESS                                  | 成功                          |
| ERROR_PACKAGE | 100  | ERROR_TIMEOUT                            | Timeout, 超时                 |
| ERROR_PACKAGE | 101  | ERROR_PACKAGE_ILLEGAL                    | 数据包非法                       |
| ERROR_PACKAGE | 102  | ERROR_PACKAGE_DAMAGE                     | 数据包损坏                       |
| ERROR_REQUEST | 301  | ERROR_REQUEST_PATH                       | 请求路径错误                      |
| ERROR_REQUEST | 302  | ERROR_REQUEST_PARAMS                     | 请求参数错误                      |
| ERROR_USER    | 401  | ERROR_USER_NO_REG                        | 用户未注册                       |
| ERROR_USER    | 402  | ERROR_USER_NO_LOGIN                      | 用户未登录                       |
| ERROR_USER    | 403  | ERROR_USER_PERMISSION_DENIED             | 拒绝用户访问，没有权限                 |
| ERROR_USER    | 411  | ERROR_PASSWORD_NOT_CORRECT               | 密码错误                        |
| ERROR_USER    | 412  | ERROR_TOKEN_FAILED                       | Token失效                     |
| ERROR_SERVER  | 500  | ERROR_INTERNAL_SERVER                    | Server Error 服务器出错，服务器处理中出错 |
| ERROR_DEVICE  | 601  | ERROR_DEVICE_NO_REG                      | 设备未注册                       |
| ERROR_DEVICE  | 602  | ERROR_DEVICE_OFFLINE                     | 设备离线                        |
| ERROR_DEVICE  | 603  | ERROR_DEVICE_PERMISSION_DENIED           | 拒绝设备访问，没有权限                 |
| ERROR_DEVICE  | 604  | ERROR_DEVICE_BIND                        | 设备绑定错误，未绑定，或绑定的用户名有误        |
| ERROR_THIRD   | 801  | ERROR_APP3RD_APPID_OR_APPKEY_ILLEGAL     | AppID或AppKey有误              |
| ERROR_THIRD   | 802  | ERROR_THIRD_APP_ID_HAS_NO_PERMISSION     | AppID无权限访问该API              |
| ERROR_THIRD   | 805  | ERROR_APP3RD_OAUTH2_ACCESSTOKEN_ILLEGAL  | AccessToken有误               |
| ERROR_THIRD   | 806  | ERROR_APP3RD_OAUTH2_ACCESSTOKEN_EXPIRED  | AccessToken过期               |
| ERROR_THIRD   | 807  | ERROR_APP3RD_OAUTH2_REFRESHTOKEN_ILLEGAL | RefreshToken有误              |
| ERROR_THIRD   | 808  | ERROR_APP3RD_OAUTH2_REFRESHTOKEN_EXPIRED | RefreshToken过期              |
| ERROR_OTA     | 901  | ERROR_OTA_FIRMWARE_NOT_EXIST             | 不存在该Firmware                |
