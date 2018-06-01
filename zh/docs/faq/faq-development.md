### **云端开发**

<u>**Q：使用Postman测试API时出现："Could not get any response"？**</u>

**A：**可能原因：1）公司网络对访问做了限制；2）Postman启用了SSL验证（SSL certifacate verification）。

&nbsp;

<u>**Q：设备已离线，调用API查询设备的资源值时，仍正常返回结果，返回的是什么数据？**</u>

**A：**返回的数值为设备离线前最后的状态。

&nbsp;

**<u>Q：调用查询资源历史数据的API（/open/res/query/history/option），不设置行号，默认是查询多少条数据？</u>** 

**A：**默认是50条。count可以设置大于50条，但要注意若count设置太大，容易出现超时。

&nbsp;

**<u>Q：为什么接口返回结果一直提示“请求参数错误”？</u>** 

**A：**1、接口的请求body和返回结果都采用JSON格式，请检查格式是否正确；2、请检查参数名称是否正确。 

&nbsp;

### **网关局域网开发**

<u>**Q：Aqara APP没有开启“局域网协议”功能的入口？**</u>

**A：**打开Aqara APP，选择需要进行局域网通信的网关设备。默认情况下，此页面不显示“局域网协议”，需连续点击"设备类型"10次才可显示。 

> 注意：当前仅“**升级版空调伴侣**”支持局域网通信协议功能。

&nbsp;

<u>**Q：Aqara APP无法打开“局域网协议”功能，开启后依旧显示灰色？**</u>

**A：**可能是设备固件版本过低，请先更新设备固件到最新版本。

&nbsp;

<u>**Q：无法收到组播消息？**</u>

**A：**设备心跳和状态上报都是组播方式发送，而很多路由器对组播都有不同形式的限制，可检查路由器后台的防火墙规则是否有过滤组播ip（224.0.0.50），或者更换路由器。

&nbsp;

<u>**Q：网关KEY对token进行AES-CBC 128加密时，无法获取到正确的key？**</u>

**A：**对于某些开发语言（如：Java）需使用“NoPadding”的填充方式。例如：使用Cipher类，cipher = Cipher.getInstance(algorithmStr)；其中algorithmStr = "AES/CBC/ NoPadding"。

&nbsp;

