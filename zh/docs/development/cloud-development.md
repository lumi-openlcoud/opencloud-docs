# 云端开发手册


## 首页

### 概述

AIOT是提供设备管理、数据采集与远程控制等服务的物联网云平台。通过智能设备采集的数据存储在AIOT云端数据库中，比如：功率、温湿度、开关状态、门窗状态等。同时，AIOT云端可以通过下发控制命令来远程控制设备。那开发者如何获得采集数据呢？又如何远程控制自己的设备呢？

为了满足第三方开发者的需求，AIOT开放平台提供云端对接方式。通过Open API和消息推送服务，可以从云端采集数据和远程控制设备。目前提供如下接口：

- 位置管理
- 设备管理
- 场景管理
- 资源管理
- 联动管理

另外，AIOT开放平台提供了消息推送服务，支持把设备上报的实时数据推送到第三方服务器，满足了开发者对数据实时性的要求。

下面，本手册将介绍下如何基于云端对接方式开发第三方应用，包括应用的创建、配置和开发等。

### 开发流程

一个好的软件开发流程加上一个好的软件系统架构，会给软件开发带来事半功倍的效果。对于云对接方式，可按照以下流程进行开发。

![应用开发流程](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/application-development-process.png)

一个稳定和高效的应用需要考虑API调用性能、消息处理性能、Access-Token的高可用性等方面，建议参考如下架构图。

![应用参考架构图](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/application-reference-architecture.png)

如果开发者还不熟悉API的调用方法，推荐使用Web API测试工具[Postman](https://www.getpostman.com/)。

> 注意：使用Postman时请关闭SSL验证（SSL certifacate verification）。

## 应用创建与配置

### 创建应用

登录[AIOT开放平台](https://opencloud.aqara.cn/)，单击“新建应用”。根据提示输入“应用名称”、“行业类型”和“应用简介”，单击“完成”后，系统会自动分配**Appid**和**Appkey**。



### 申请资源权限

资源是指由设备产生的数据，包括设备的设置参数与实时状态。一个设备具有多个资源，不同的资源表示不同含义的数据。例如，开关状态（plug_status）是智能插座的一个资源，表示智能插座当前是否通电。通过访问该资源，开发者可以查询插座的当前状态和远程开关插座。

访问资源之前，开发者需要先按级别申请资源的权限。资源级别分为1级和2级，1级表示常用的资源，2级表示不常用的资源。获得某个类型设备的1级授权后，开发者可以在应用中访问该类型设备的所有常用资源。

默认情况下，应用会自动分配所有类型设备的1级资源权限。如果需要申请2级资源授权，请访问“应用管理”页面，然后切换到“资源授权”页。

- 单击每一行右侧的“申请资源”按钮，申请对应设备类型的资源。
- 选中左侧多个勾选框，然后单击右上角的“批量申请”按钮，可以批量申请多个设备类型的资源。

申请单提交后请耐心等待管理员审核，预计1个工作日的处理时间。



### 申请API权限

API是AIOT开放平台对外提供数据的接口，用于查询、控制设备和配置自动化、场景等信息。目前开放的API接口默认为已授权状态。



## 账号授权

只有获得AIOT账号的授权许可，第三方应用才能访问与控制该账号下的设备，同时AIOT开放平台才会把设备消息推送到第三方服务器。

> 说明：在应用商店或Apple Store搜索“Aqara Home”，下载安装后注册Aaqra账号。

目前，AIOT开放平台提供授权方式：OAuth 2.0授权。

### OAuth2.0

OAuth2.0 是一个开放标准，允许用户让第三方应用访问该用户在某一网站（或物联网平台）上存储的私密资源（如用户信息、照片、视频、设备数据等），而无需将用户名和密码提供给第三方应用。

AIOT开放平台采用OAuth 2.0标准的授权码（authorization_code）模式，适用于拥有server端的应用。OAuth 2.0 授权流程简单、安全，时序图如下所示，完成授权流程后获得访问令牌（AccessToken）。此后，开发者使用访问令牌来调用接口，获取用户信息或操作用户的设备。

![Oauth2.0授权码模式时序图](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/doc-cloud-development/auth-sequence-diagram.png)

OAuth 2.0 详细授权流程如下：

> 注意：本章节URL示例以“中国大陆地区（https://aiot-oauth2.aqara.cn/）”为例进行说明，若在其他地区，请参考[国家（地区码）](http://docs.opencloud.aqara.cn/development/region_code)文档更改域名。

#### 步骤1 请求授权码

首先，第三方应用需要通过浏览器将用户重定向到AIOT OAuth 2.0服务。使用Aqara账号登录成功后，AIOT开放平台会返回用户的授权码（code）。授权码的有效期为10分钟，请在10分钟内完成后续流程。

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
- **返回参数**

| 参数名           | 描述                         |
| ------------- | -------------------------- |
| access_token  | 访问令牌，第三方应用访问AIOT开放服务的凭证    |
| expires_in    | 访问令牌的剩余有效时间，单位为秒           |
| token_type    | 根据OAuth 2.0 标准，取值为`bearer` |
| openId        | 授权用户的唯一标识                  |
| refresh_token | 新的刷新令牌，旧的刷新令牌立即作废          |
| state         | 取值为任意字符串，认证服务器将原样返回该参数     |



## API调用

### API调用规范

1. 为了保证数据传输的安全，AIOT开放平台对外提供的API均采用HTTPS协议，中国大陆地区域名为：**https://aiot-open-3rd.aqara.cn**。若在其他地区，请参考[国家（地区码）](http://docs.opencloud.aqara.cn/development/region_code)文档更改域名。

2. OpenID 作为第三方应用对用户的唯一标识，是原AIOT账号加密后的结果。每个AIOT用户对每个第三方应用有一个唯一的OpenID。

3. 接口的请求body和返回结果都采用**JSON格式**。

4. 通过接口查询设备状态或控制设备时需要设置参数“资源别名”，不同资源的取值类型也不一样。所有资源的信息（别名、取值类型、含义等）请访问[AIOT开放平台](https://opencloud.aqara.cn)的“应用管理->资源授权”页面。

5. 所有功能的详细API定义请访问[AIOT开放平台](https://opencloud.aqara.cn)的“应用管理->API访问”页面。

### 调用示例

#### OAuth 2.0

例如，通过调用接口查询一个设备的详细信息，调用方法如下：

- 请求URL：https://aiot-open-3rd.aqara.cn/open/device/query

- 请求方式： HTTP POST （application/json）

- 请求header示例

| Key          | Value                             | 描述（可不填）          |
| ------------ | --------------------------------- | ---------------- |
| Appid        | 54a2301000000000000478911         | 应用的唯一标识          |
| Appkey       | oT7kp77v123456siiXISamsPpvaTaWeZ  | 应用的秘钥            |
| Openid       | Yb2bR2btJ1234EmU5Z3jzh4oQsttie    | 通过OAuth授权获得的用户ID |
| Access-Token | 12db5a10c49fd2112233dbc67a0d13ab5 | 通过OAuth授权获得的访问令牌 |
| Content-Type | application/json                  | 返回结果采用JSON格式     |

> 注意：在请求header时填写的Key和Value值，需注意字母大小写。

- 请求body示例

```
  {
  "openId": "Yb2bR2btJ1234EmU5Z3jzh4oQsttie",
  "did": "lumi.158d0001234654"
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

如果第三方应用需要接收设备消息，开发者需要按照如下步骤启用消息推送功能：

1. 填写服务器配置；
2. 验证服务器地址；
3. 调用订阅资源接口，订阅需要推送的资源；
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

> 注意：目前支持“明文模式”和“安全模式”，为了数据安全性，推荐使用“安全模式”。



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

在安全模式下，服务器验证的方法变得复杂，和微信公众号平台类似。 开发者保存服务器配置后后，AIOT服务器将发送POST请求到填写的服务器地址（URL)：

- Token: header签名token,
- EncodingAESKey：AES加密密钥(Base64处理),
- echostr：随机字符串。

body加密解密：
```
public static String encrypt(String src, byte[] key) throws Exception {
    SecretKeySpec skeySpec = new SecretKeySpec(key, "AES");
    AlgorithmParameterSpec params = new IvParameterSpec(IV);
    Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
    cipher.init(Cipher.ENCRYPT_MODE, skeySpec, params);
    byte[] content = src.getBytes("utf-8");
    byte[] ret = cipher.doFinal(content);
    return Base64.getEncoder().encodeToString(ret);
}

public static byte[] decrypt(String src, byte[] key) throws Exception {
    byte[] srcByte = Base64.getDecoder().decode(src);
    SecretKeySpec skeySpec = new SecretKeySpec(key, "AES");
    AlgorithmParameterSpec params = new IvParameterSpec(IV);
    Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
    cipher.init(Cipher.DECRYPT_MODE, skeySpec, params);
    byte[] ret = cipher.doFinal(srcByte);
    return ret;
}
```



若确认此次POST请求来自绿米服务器，请原样返回echostr参数内容，则验证服务器成功，否则验证失败。

其中，header签名流程如下：

1. 将Appid、Token、Time三个参数进行字典序排序，然后进行拼接；
  例如：appid=xxx&token=xxx&time=xxx
2. 拼接开发者配置的EncodingAESKey；
  例如：appid=xxx&token=xxx&time=xxx&AESKey
3. 最后对产生的字符做MD5（32位），生成的数即为Sign的值（小写），开发者通过对比Sig的一致性来判断该请求是否来源于AIOT服务器。



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
            "did": "lumi.xxxxxxxxxx",
            "attach": "xxxx"
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
| attach  | 附加信息-推送时带上                    |

**设备消息**

```
{
    "msgType": "device", 
    "data": {
        "openId": "xxxxxx", 
        "name": "空调伴侣", 
        "model": "lumi.acpartner.xxx", 
        "time": 1503560767, 
        "event": "DEV_INFO_CHANGED", 
        "did": "lumi.xxxxxxx", 
        "parentId": "", 
        "extra": "{"clientId":"xxxx"}"
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
| extra    | 附加消息                  |

| 事件类型               | 描述    |
| :----------------- | :---- |
| GW\_BIND           | 网关入网  |
| GW\_UN\_BIND       | 网关解绑  |
| GW\_ONLINE         | 网关在线  |
| GW\_OFFLINE        | 网关离线  |
| SUB\_DEV\_BIND     | 子设备入网 |
| SUB\_DEV\_UN\_BIND | 子设备解绑 |
| SUB\_DEV\_ONLINE   | 子设备在线 |
| SUB\_DEV\_OFFLINE  | 子设备离线 |

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











