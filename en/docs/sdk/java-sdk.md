# Java SDK Development Documentation

The purpose of this document is to explain how to use SDK for the development of push notification. 



## Preparation

1. Login in [AIOT Open Cloud Platform](https://opencloud.aqara.cn/). After create an application, you can get “AppId” and “AppKey” from “Application Management" - "Application Overview" page.

2. In “Push Notification” page, configure server information (URL, Token, EncodingAESKey and Message encryption mode), and set whether to subscribe message. For details, see the "[Push Notification](http://docs.opencloud.aqara.com/en/development/cloud-development/#_10)” chapter in "Cloud Development Manual".

3. Download and decompress [Java SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/aiot_sdk_message_java_v0.3.zip).

   1) iot.utils-1.0.0.jar: contains Java source code files of SDK.

- SecHttpHandler.java: The SDK interface classes provided by the AIOT open cloud platform include message encryption and decryption rules.

- SecRequest.java: The request packet object of secure HTTP request.

- SecResponse.java: HTTP response result object.

- SecurityUtils.java: Security encryption tools.

  2) SecRecvResourceServlet.javax: decompress  iot.utils-1.0.0.jar. In \iot.utils-1.0.0\com\lumi\largedata\iot\sechttp\demo folder, it is a demo of verify server in push notification.

4. Download and install the integrated development environment.



## Steps

### Install SDK

This SDK supports two installation methods:

**1. Using Maven**

If you use Maven to manage dependencies, you can install the Java SDK by adding the following code to `pom.xml` :

```
<dependency>
    <groupId>com.lumi.largedata</groupId>
    <artifactId>iot</artifactId>
    <version>1.0.0</version>
</dependency>
```



**2. Import JAR file in IDE**

**<u>Eclipse</u>**

1. Copy `iot.utils-1.0.0.jar` file to your project folder.
2. Open your project in Eclipse, right-click the project and click **Properties**.
3. Click **Java Build Path > Libraries > Add JARs** and select the JAR file to add.
4. Click **Apply and Close**. 



**<u>IntelliJ</u>**

1. Copy `iot.utils-1.0.0.jar` file to your project folder.
2. Open your project in IntelliJ, click **File > Project Structure** from the menu.
3. Click **Modules > Dependencies**. In the list that appears, click **add > JARs or directories** and select the JAR file to add.
4. Click **Apply**, then click **OK**.





### Set up identity credentials

Message push uses "AppId" and "AppKey" for authentication, so you need to set credentials in the code.

> Note: Make sure that the code for "AppId" and "AppKey" is not leaked, for example, published on public projects of Github.

```
private String MY_APP_ID = "<your-APP-ID>";    //Third-party application ID, APPID
private String MY_APP_KEY = "<your-APP-Key>";   //Third-party application key, AppKey
private String MY_TOKEN = "<your-Token>";     //Custom Tokens in "Server Configuration"
```



### Verify server

> Note: Presently, only the "plain text mode" is supported. So only introduce how to verify the server through plain text mode.

After the developer saves the server configuration, the AIOT server sends a POST request to the server address (URL) as configured by the developer. The request will carry an "echostr" parameter(JSON format) that consists of a random string. If the third-party server receives the request, please return the "echostr" parameter without any changes to ensure authentication to the server is successful; otherwise, the authentication fails.

Extract message body and code example is as follows:

```
ServletInputStream inputStream = request.getInputStream();
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
byte[] bytes = new byte[256];
int len = 0;
while ((len = inputStream.read(bytes)) != -1) {
    outputStream.write(bytes, 0, len);
    }
String payload = new String(outputStream.toByteArray(), "utf-8");
JSONObject dataJson = JSON.parseObject(payload);    
```

Determine whether the message body carries the parameter echostr, and return the content of the echostr parameter to verify the server. Code example is as follows:

```
if (dataJson.containsKey("echostr")) {        
    if (MY_APP_ID.equals(request.getHeader("Appid")) && MY_APP_KEY.equals(request.getHeader("Appkey"))) {      
        SecHttpHandler secHttpHandler = SecHttpHandler.createHttpClient(MY_TOKEN, request.getHeader("Appid"), request.getHeader("Appkey"));   
        try {
            SecRequest secRequest = secHttpHandler.getSecRequest(payload);
            Map<String, Object> result = secRequest.getPayload();
            String echoStr = result.get("echostr").toString();    
            String secResponse = secHttpHandler.getSecResponse(0, echoStr, secRequest);  
            String secResponseDecode = URLDecoder.decode(secResponse, "UTF-8");
            response.getWriter().write(secResponseDecode);   
            } catch (Exception e) {
                    //TODO
                }
        } else {               
            response.getWriter().write(JSONObject.toJSONString("invalid appid or appkey"));
            }
            response.getWriter().flush();
}
```



### Judgment message type

After you enable the message subscription function, you can use `msgType` to determine whether it is a resource message or a device message.

- Resource message(resource): resource change information, such as temperature changes, power changes, etc .

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

- Device messages(device): Device event messages, such as device on-line/off-line, linking/unlinking, etc.

```
{
    "msgType": "device", 
    "data": {
        "openId": "GoeFrrL7mN9SsGRi8WJn4x4YnQpXTS", 
        "name": "air conditioning controller", 
        "model": "lumi.acpartner.aq1", 
        "time": 1503560767, 
        "event": "DEV_INFO_CHANGED", 
        "did": "lumi.158d00010b4090", 
        "parentId": ""
    }
}
```

Code example of judging the message type is as follows:

    if (dataJson.containsKey("msgType")) {   
        String msgType = dataJson.getString("msgType");
        if (msgType.equals("resource")) {
            JSONArray data = dataJson.getJSONArray("data");
            JSONObject dedata = data.getJSONObject(0);
            Map<String, Object> msdata = new HashMap<String, Object>();
            msdata.put("time", dedata.get("time"));
            xxxxxxxxxxxxxxx   //resolve message body
        } else if (msgType.equals("device")) {
            JSONObject data= dataJson.getJSONObject("data");
            Map<String, Object> devdata = new HashMap<String, Object>();
            devdata.put("openId", data.get("openId"));
            xxxxxxxxxxxxxxx   //resolve message body
            }
        }

