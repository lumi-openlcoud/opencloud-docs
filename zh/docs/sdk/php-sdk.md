# PHP SDK开发文档

本文档旨在说明如何使用该SDK将消息推送到第三方服务器的开发。



## 前期准备

1、访问并登录[AIOT开放平台](https://opencloud.aqara.cn/)网站，创建新应用后，在“应用管理”-“应用概览”页面获取“AppId”和“AppKey”。

2、在“消息推送”页面，配置服务器信息（URL、Token、EncodingAESKey和消息加解密方式），并设置是否订阅消息。详细说明请参见“云端开发手册”的“[消息推送](http://docs.opencloud.aqara.cn/development/cloud-development/#_10)”章节。

3、下载并解压[PHP SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/aiot_sdk_message_php.zip)。

- SecHttpHandler.php为AIOT开放平台提供的SDK。
- demo.php为消息推送验证服务器的Demo示例。

4、下载并安装集成开发环境。



## 使用说明

### 安装

将SecHttpHandler.php添加到您的项目中，在代码中添加如下引用：

```
<?php
include 'SecHttpHandler.php';
?>
```




### 设置身份凭据

消息推送使用“AppId”和“AppKey”进行身份验证，因此需在代码中设置凭据。

> 注意：确保“AppId”和“AppKey”的代码不要泄漏，例如发布在Github的公共项目上。

```
$appId="<your-APP-ID>";      //第三方应用ID，APPID
$appKey="<your-APP-KEY>";    //第三方应用秘钥，AppKey
$token="<your-Token>";      //在配置服务器信息中自定义的Token
```



### 验证服务器

> 注意：由于当前只支持明文模式，因此仅介绍如何通过明文模式验证服务器。

在“消息推送”页面配置服务器信息后，AIOT服务器会发送POST请求到服务器地址（URL），请求将携带一个参数echostr（JSON格式），它是一个随机的字符串。第三方服务器收到该请求后，若原样返回echostr参数内容，则验证服务器成功，否则验证失败。

代码示例如下：

```
$json = file_get_contents("php://input");
$secHttpHandler = SecHttpHandler::createSecHttpHandler($appId, $appKey, $token);
$secRequest = $secHttpHandler->getSecRequest($json);
$jsonResponse = json_encode($secRequest->payload);
$secResponse = $secHttpHandler->getSecResponse(0, $jsonResponse, $secRequest);
echo $secResponse;
```