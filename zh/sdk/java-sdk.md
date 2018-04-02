# Java SDK开发文档

本文档旨在说明如何使用该SDK将设备或资源消息推送到第三方服务器的开发。



## 前期准备

1、访问并登录[AIOT开放平台](https://opencloud.aqara.cn/)网站，创建新应用后，在“应用管理”-“应用概览”页面获取“AppId”和“AppKey”。

2、在“消息推送”页面，配置服务器信息（URL、Token、EncodingAESKey和消息加解密方式），并设置是否订阅消息。详细说明请参见“云端开发手册”的“[消息推送](http://docs.opencloud.aqara.cn/development/cloud-development/#_10)”章节。

3、下载并解压[Java SDK](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/sdk/aiot_sdk_message_java.zip)。

- iot.utils-1.0.0.jar：包含SDK相对应的Java源代码文件。
- SecHttpHandler.java：解压iot.utils-1.0.0.jar，在\iot.utils-1.0.0\com\lumi\largedata\iot\sechttp目录下，为AIOT开放平台提供的SDK接口类。
- SecRecvResourceServlet.javax：解压iot.utils-1.0.0.jar，在\iot.utils-1.0.0\com\lumi\largedata\iot\sechttp\demo目录下，包含一个如何使用该SDK的Demo。

4、下载并安装集成开发环境。



## 使用说明

### 安装SDK

消息推送SDK支持以下两种安装方式：

**1、使用Maven**

如果您使用了Maven管理依赖，您可以通过向`pom.xml`中添加以下代码来安装消息推送Java SDK：

```
<dependency>
    <groupId>com.lumi.largedata</groupId>
    <artifactId>iot</artifactId>
    <version>1.0.0</version>
</dependency>
```



**2、在集成开发环境（IDE）中导入JAR文件**

**<u>Eclipse</u>**

1. 将下载的`iot.utils-1.0.0.jar`文件复制到您的项目文件夹中。
2. 在Eclipse中打开您的项目，右键单击该项目，单击**Properties**。
3. 在弹出的对话框中，单击**Java Build Path > Libraries > Add JARs**，添加下载的JAR文件。
4. 单击**Apply and Close**。



**<u>IntelliJ</u>**

1. 将下载的`iot.utils-1.0.0.jar`文件复制到您的项目文件夹中。
2. 在IntelliJ中打开您的项目，在菜单栏中单击**File > Project Structure**。
3. 在弹出的对话框中，单击**Modules > Dependencies**。在出现的列表中单击**add > JARs or directories**，选择要添加的JAR文件。
4. 单击**Apply**，然后单击**OK**。



### 设置身份凭据

消息推送使用“AppId”和“AppKey”进行身份验证，因此需在代码中设置凭据。

> 注意：确保“AppId”和“AppKey”的代码不要泄漏，例如发布在Github的公共项目上。

```
private String MY_APP_ID = "<your-APP-ID>";    //第三方应用ID，APPID
private String MY_APP_KEY = "<your-APP-Key>";   //第三方应用秘钥，AppKey
private String MY_TOKEN = "<your-Token>";     //在配置服务器信息中自定义的Token
```



### 验证服务器

在“消息推送”中配置服务器信息后，AIOT服务器会发送一个POST请求到设置的服务器地址（URL），请求将携带一个参数echostr（JSON格式），它是一个随机的字符串。如果第三方服务器收到该请求，若原样返回echostr参数内容，则验证服务器成功，否则验证失败。若验证失败，可单击URL后的”验证服务器“再次验证。

提取消息体，代码示例如下：

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

判断消息体是否携带参数echostr，若携带echostr，则验证服务器。代码示例如下：

```
if (dataJson.containsKey("echostr")) {        
    SecHttpHandler secHttpHandler = SecHttpHandler.createHttpClient(MY_TOKEN, request.getHeader("Appid"), request.getHeader("Appkey"));   
    if (MY_APP_ID.equals(request.getHeader("Appid")) && MY_APP_KEY.equals(request.getHeader("Appkey"))) {      
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
            response.getWriter().write(JSONObject.toJSONString("Server verification failed"));
            }
            response.getWriter().flush();
}
```



### 判断消息类型

如果消息体中包含了msgType，判断是资源消息是设备消息。

- 资源消息（resource）：资源的变化消息，比如温度变化、功率变化等。
- 设备消息（device）：设备的事件消息，比如设备在线离线、绑定解绑等。

```
if (dataJson.containsKey("msgType")) {   
    String msgType = dataJson.getString("msgType");
    if (msgType.equals("resource")) {
        xxxxxxxxxxxxxxx   //解析消息体
            } else if (msgType.equals("device")) {
        xxxxxxxxxxxxxxx   //解析消息体
            }
        }
```



## Demo示例

解压iot.utils-1.0.0.jar，在\iot.utils-1.0.0\com\lumi\largedata\iot\sechttp\demo目录下，包含一个如何使用SDK的Demo：SecRecvResourceServlet.javax。

