# PHP SDK Development Documentation

The purpose of this document is to explain how to use SDK for the development of push notification. 



## Preparation

1. Login in [AIOT Open Cloud Platform](https://opencloud.aqara.cn/). After create an application, you can get “AppId” and “AppKey” from “Application Management" - "Application Overview" page.

2. In “Push Notification” page, configure server information (URL, Token, EncodingAESKey and Message encryption mode), and set whether to subscribe message. For details, see the "Push Notification" chapter in "Cloud Development Manual".

3. Download and decompress [PHP SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/aiot_sdk_message_php.zip).

   1) SecHttpHandler.php is the SDK that provided by the AIOT open cloud platform.

   2) demo.php is a demo of verify server in push notification.

4. Download and install the integrated development environment.



## Steps

### Install SDK

Add SecHttpHandler.php to your project. Please add the following reference in the code:

```
<?php
include 'SecHttpHandler.php';
?>
```



### Set up identity credentials

Message push uses "AppId" and "AppKey" for authentication, so you need to set credentials in the code.

> Note: Make sure that the code for "AppId" and "AppKey" is not leaked, for example, published on public projects of Github.

```
$appId="<your-APP-ID>";      //Third-party application ID, APPID
$appKey="<your-APP-KEY>";    //Third-party application key, AppKey
$token="<your-Token>";      //Custom Tokens in "Server Configuration"
```



### Verify server

> Note: Presently, only the "plain text mode" is supported. So only introduce how to verify the server through plain text mode.

After the developer saves the server configuration, the AIOT server sends a POST request to the server address (URL) as configured by the developer. The request will carry an "echostr" parameter(JSON format) that consists of a random string. If the third-party server receives the request, please return the "echostr" parameter without any changes to ensure authentication to the server is successful; otherwise, the authentication fails.

Code example is as follows:

```
$json = file_get_contents("php://input");
$secHttpHandler = SecHttpHandler::createSecHttpHandler($appId, $appKey, $token);
$secRequest = $secHttpHandler->getSecRequest($json);
$jsonResponse = json_encode($secRequest->payload);
$secResponse = $secHttpHandler->getSecResponse(0, $jsonResponse, $secRequest);
echo $secResponse;
```