### **Cloud Development**

<u>**Q: When using the Postman to test API, why it always prompt "Could not get any response"?**</u>

**A: **1) Company network restricts access; 2) Postman enable SSL certifacate verification.

&nbsp;

<u>**Q: The device is offline. When the API is called to query the resource value of the device, the result is still returned normally. What data is returned?**</u>

**A: **The data returned is the last state before device offline.

&nbsp;

**<u>Q: When the API is called to query the history data of resources(/open/res/query/history/option) and no "count" parameter, how many data will be returned by default?</u>** 

**A: **The default is 50. But the value of "count" can be set more than 50. If the value is too large, it is easy to timeout.

&nbsp;

**<u>Q: Why the results returned through the interface always prompt “Error parameter”?</u>** 

**A: **1. The body of the request and the results returned through the interface using the **JSON format**. Please check whether the format is correct; 2. Please check whether the parameter is correct. 

&nbsp;

### **Gateway LAN Communication**

<u>**Q: How to enable "LAN Communication" function on Aqara APP?**</u>

**A: **Open Aqara APP, select the gateway device that needs LAN communication. This page does not show "LAN Communication" by default. You need tap on "device type" 10 times and it will appear.

> **Note:** Only **Air Conditioning Controller(advanced)** supports LAN communication on Aqara APP.

&nbsp;

<u>**Q: I can't enable "LAN Communication" function on Aqara APP. The button still shows grey after enable it?**</u>

**A: **Please upgrade the device firmware to the lastest version.

&nbsp;

<u>**Q: I can't receive multicast message?**</u>

**A: **Heartbeat and attributes are reported by multicast method. But many routers have different types of restrictions on multicast. You can check whether firewall rules in the background of the router filter multicast ip (224.0.0.50) or replace the router.

&nbsp;

<u>**Q: When user uses the KEY of the gateway to perform AES-CBC 128 encryption on token, I can't get the correct key?**</u>

**A: **For some development languages, such as Java, it needs to use filling method: "NoPadding”. For example, using Cipher class, cipher = Cipher.getInstance(algorithmStr), algorithmStr = "AES/CBC/ NoPadding"。

&nbsp;

