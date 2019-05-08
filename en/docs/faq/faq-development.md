### **Cloud Development**

<u>**Q: When using the Postman to test API, why it always prompt "Could not get any response"?**</u>

**A: **1) Company network restricts access; 2) Postman enable SSL certifacate verification.

&nbsp;

<u>**Q: If the network is disconnected for more than 2 hours, do you need to re-run the process to get the accessToken?**</u>

**A:** If you set the timing (usually one and a half hours) to get a new accessToken with refreshToken, then you do not need to re-process the process to obtain. The AccessToken is valid for 2 hours and the RefreshToken is valid for one month.

&nbsp;

<u>**Q: When subscribing to resources (calling interface: / 3rd/v1.0/open/subscriber/resource), prompt: Device permission denied**</u>

**A:** Check whether the device under Appid's corresponding user account has been unbound.

&nbsp;

<u>**Q: The device is offline. When the API is called to query the resource value of the device, the result is still returned normally. What data is returned?**</u>

**A: **The data returned is the last state before device offline.

&nbsp;

**<u>Q: How many queries does the query device (call interface: / 3rd / v1.0 / open / device / query) support?</u>** 

**A: **The default is 50. Currently, pageSize has no restrictions, but the number is too large and it is easy to time out. It is recommended not to exceed 300.

&nbsp;

**<u>Q: Why the results returned through the interface always prompt “Error parameter”?</u>** 

**A: **1. The body of the request and the results returned through the interface using the **JSON format**. Please check whether the format is correct; 2. Please check whether the parameter is correct. 

&nbsp;

<u>**Q: Subscribed resources, why not receive push?**</u>

**A:** The Lumi server checks the availability of the server receiving resource push once a week. If it is not available, it will stop pushing. It needs to re-verify the server before it can re-push.

&nbsp;

### **Gateway LAN Communication**

<u>**Q: I can't receive multicast message?**</u>

**A: **Heartbeat and attributes are reported by multicast method. But many routers have different types of restrictions on multicast. You can check whether firewall rules in the background of the router filter multicast ip (224.0.0.50) or replace the router.

&nbsp;

<u>**Q: When user uses the KEY of the gateway to perform AES-CBC 128 encryption on token, I can't get the correct key?**</u>

**A: **For some development languages, such as Java, it needs to use filling method: "NoPadding”. For example, using Cipher class, cipher = Cipher.getInstance(algorithmStr), algorithmStr = "AES/CBC/ NoPadding"。

&nbsp;

