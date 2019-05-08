### **云端开发**

<u>**Q：使用Postman测试API时出现："Could not get any response"？**</u>

**A：**可能原因：1）公司网络对访问做了限制；2）Postman启用了SSL验证（SSL certifacate verification）。

&nbsp;

<u>**Q：断网超过2小时，需要重新操作流程获取accessToken吗？**</u>

**A：**如果设置了定时（一般一个半小时）用refreshToken获取新的accessToken，那么是不需要再重新操作流程获取的。AccessToken 有效期2小时，refreshToken有效期一个月。

&nbsp;

<u>**Q：订阅资源（调用接口：/3rd/v1.0/open/subscriber/resource）时，提示：Device permission denied**</u>

**A：**检查Appid对应的用户账号下设备是否已解绑。

&nbsp;

<u>**Q：设备已离线，调用API查询设备的资源值时，仍正常返回结果，返回的是什么数据？**</u>

**A：**返回的数值为设备离线前最后的状态。

&nbsp;

**<u>Q：查询设备（调用接口：/3rd/v1.0/open/device/query）最大支持查询多少条数据?</u>** 

**A：**默认显示50条，目前pageSize 没有做限制，但数量太大容易超时，建议最大不要超过300。

&nbsp;

**<u>Q：为什么接口返回结果一直提示“请求参数错误”？</u>** 

**A：**1、接口的请求body和返回结果都采用JSON格式，请检查格式是否正确；2、请检查参数名称是否正确。 

&nbsp;

<u>**Q：订阅的资源，为什么收不到推送了？**</u>

**A：** 绿米服务器每周会校验一次接收资源推送的服务器是否可用，如果不可用即会停止推送，需要重新验证服务器后才会重新推送。

&nbsp;

### **网关局域网开发**

<u>**Q：无法收到组播消息？**</u>

**A：**设备心跳和状态上报都是以组播方式发送，而很多路由器对组播都有不同形式的限制，可检查路由器后台的防火墙规则是否有过滤组播ip（224.0.0.50），或者更换路由器。

&nbsp;

<u>**Q：网关KEY对token进行AES-CBC 128加密时，无法获取到正确的key？**</u>

**A：**对于某些开发语言（如：Java）需使用“NoPadding”的填充方式。例如：使用Cipher类，cipher = Cipher.getInstance(algorithmStr)；其中algorithmStr = "AES/CBC/ NoPadding"。

&nbsp;

