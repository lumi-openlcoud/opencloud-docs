###  **Air Conditioning Controller**

<u>**Q: Does Air Conditioning Controller have to work with a hub?**</u>

**A: **No, Air Conditioning Controller inherits all the functions of the hub (except night light and Illumination sensing function) , it can work independently to achieve the intelligent transformation of traditional air conditioning,  also can be used as a hub for other ZigBee sub-devices.

&nbsp;

<u>**Q: How do Air Conditioning Controller and sensor devices work with Aqara App?**</u>

**A: **1)  First, sensors send signals to Air Conditioning Controller by ZigBee. After receive these signals, Air Conditioning Controller will handle these signals and send them to cloud by Wi-Fi. Then cloud server send these information to Aqara App. 2) After the instruction initiated by Aqara APP reaches the Air Conditioning Controller via Cloud and Wi-Fi, Air Conditioning Controller will transfer instructions to corresponding infrared code, then send.

&nbsp;

<u>**Q: When Air Conditioning Controller is used as a hub, how many sub-devices does it support？**</u>

**A: **It supports up to 32 sub-devices.

&nbsp;

<u>**Q: What is the Air Conditioning Controller's power reporting mechanism?**</u>

**A: **When value of load power changed over 5W, Air Conditioning Controller will report.

&nbsp;

<u>**Q: What are the requirements of Air Conditioning Controller installation?**</u>

**A: **1) Since the Air Conditioning Controller mainly judges the status of the air conditioner by judging the power of the air conditioner, so the recommended installation method of Air Conditioning Controller is plug it in the air conditioning plug, then plug the power plug of air conditioning to Air Conditioning Controller；2) Air Conditioning Controller relies on launching infrared light to control Air Conditioning, so Air Conditioning Controller and air conditioning must be in the same room, and there is no obstructer between Air Conditioning Controller and infrared panel.

&nbsp;

<u>**Q: When Air Conditioning Controller is used as a hub, what are the requirements of installation?**</u>

**A: **When Air Conditioning Controller is used as a hub, you need to consider about use with sensors, it is better to choose the central area where each sensor is installed as the installation position of hub, and keep a distance of about 2-6 meters with the wireless router. 

&nbsp;

<u>**Q: Why Air Conditioning Controller doesn't plug into air conditioning outlet?**</u>

**A: **For safety reasons, air conditioner outlets are generally 16A specifications. If outlet is 10A specifications, please buy a converter.

&nbsp;

<u>**Q: Without my air conditioner brand or model, and can't match successfully, what should I do?**</u>

**A: **1)  Please don't match immediately after open/close air conditioner. After the air conditioner is turned on or off for a period of time (after 5 minutes), try to match again. 2)  Our infrared code already covers the vast majority of air conditioning brands and types. Maybe your air conditioner is not yet covered, you can follow the APP guidelines to upload type and picture of remote control, we will develop soon. 

&nbsp;

<u>**Q: How to reset Air Conditioning Controller?**</u>

**A: **Long press the switch button for 5s, when red light keeps flashing, it means reset successful. 

&nbsp;

<u>**Q: Why the bounded air conditioner and sub-devices appear after reset?**</u>

**A: **The bounded air conditioner and sub-devices are related to Aqara account. If Aqara account isn't change, it will not delete these information after reset.

&nbsp;

### **Smart Plug**

<u>**Q: What is the Smart Plug's power reporting mechanism?**</u>

**A: **Reporting mechanism: the value of power changed over 5% and 3W, or heartbeat report (heartbeat cycle is 5~8 minutes).

&nbsp;

<u>**Q: Does Smart Plug (ZigBee) support 16A outlet?**</u>

**A: **Smart Plug (ZigBee) is 10A specifications, it is not support 16A outlet.

&nbsp;

<u>**Q: Does Smart Plug (ZigBee) support household appliances, such as air conditioner, water heater, humidifier, water dispenser?**</u>

**A: **The maximum load power of Smart Plug is 2200W, the maximum voltage and electric current are AC 250V 10A. As long as the power, voltage and electric current of the appliances are within the maximum load, it can be used. Note: some air conditioners and water heater only support 16A outlet, they can't use this plug.

&nbsp;

<u>**Q: Does Smart Plug (ZigBee) support Power off memory function?**</u>

**A: **Based on security considerations, after power off, when power on again, it turns off plug in deafult. However, there is a "power off memory" setting item in Aqara app. After enable the "power off memory" function, it will remember the status before power off. When power on again, it will recover the status before power off.

&nbsp;

<u>**Q: Does Smart Plug (ZigBee) support electricity statistics?**</u>

**A: **Support. Smart Plug (ZigBee) supports real-time power statistics and precision measurement of the actual power used by the appliance. Daily and monthly electricity statistics can be viewed in APP.

&nbsp;

### **Wall Outlet**

<u>**Q: What is the Wall Outlet's power reporting mechanism?**</u>

**A: **Reporting mechanism: the value of power changed over 5% and 3W, or heartbeat report (heartbeat cycle is 5~8 minutes).

&nbsp;

<u>**Q: What if Wall Outlet's neutral line and fire line are connected backwards?**</u>

**A: **Red indicator light will be on, please change neutral line and fire line.

&nbsp;

### **Wall Switch**

<u>**Q: What's the difference between Wall Switch(No Neutral, Single/Double Rocker) and Wall Switch(With Neutral, Single/Double Rocker) ?**</u> 

**A: **1) The basic functions are the same, but Wall Switch(With Neutral, Single/Double Rocker)  has electricity statistics function, and has ZigBee signal relay function. 2) The former only need fire line, but the latter need neutral line and fire line.  

&nbsp;

<u>**Q: What is the maximum commiunication distance between Wall Switch and hub?**</u> 

**A: **If there is a wall between Wall Switch and hub, the commiunication distance can reach 7~10m. If there are many walls between them, it is better to put Wall Switch as close as possible to hub.

&nbsp;

<u>**Q: Will there be a short circuit when the connection of neutral line and fire line is reversed?**</u> 

**A: **If the connection of neutral line and fire line is reversed, it can use normally. But when turn off Wall Switch, the fire line of light isn't turned off, it will have an electric shock. If the light wire is connected to hole L or hole N by mistake, it doesn't have a short circuit, but the wall switch will no response. It is suggested to install by a professional electrician. 

&nbsp;

<u>**Q: How much power does the Wall Switch(With Neutral, Single/Double Rocker)  need to be equipped with to operate normally?**</u> 

**A: **Since there is no minimum load requirement due to the neutral line and the live line, the maximum load of the total number of channels is not to exceed 2500W.

&nbsp;

<u>**Q: How much power does the Wall Switch(No Neutral, Single/Double Rocker)  need to be equipped with to operate normally?**</u>

**A: **Wall Switch (No Neutral, Single/Double Rocker) can support 3W energy saving lamp or 5W LED lamp or 16W fluorescent lamp. Special Note: If it is an old lamp with a starter, please replace the electronic starter. At present, Wall Switch has better support for mainstream brands of lamps, but for niche brands or cottage manufacturers, due to the circuit design and the use of materials inside the lamps, it may cause abnormal phenomena such as lamp flicker, switch crash, etc. Please choose carefully.

&nbsp;&nbsp;

<u>**Q: Does Wall Switch support dual-control function?**</u> 

**A: **First, by using the Wall Switch and the wireless switch, it is very easy to implement the dual-control function. The wireless switch can be freely placed at most locations in the home, and the Wall Switch is controlled in conjunction; secondly, if the original switch in the home is dual in the dual control mode, when only one of the switches is replaced, it is necessary to maintain another ordinary switch in the normally open state or in the short-closed state. Otherwise, after the ordinary switch is powered off, the ZigBee communication module of the Wall Switch cannot be powered, and the intelligent device cannot be intelligent control. Since the aqara Wall Switch was designed at the beginning, it hopes to allow users to implement dual control without slotted wiring. Therefore, users who have already used dual control circuits for slotted wiring can choose to use a wireless switch to achieve dual control.

&nbsp;

<u>**Q: What's the difference between Wall Switch and Wireless Mini Switch?**</u>

**A: **Wall Switch is connected to the wire, the most critical role is to be able to remotely intelligently control the power supply and disconnection of the connected circuit, if the ordinary light is transformed into a smart light, Wall Switch is required; wireless switch is a controller, it can control Wall Switch and other equipment through the APP, but can not directly control the lights.

&nbsp;

### **Curtain Controller**

<u>**Q: What kind of track does Curtain Controller support?**</u>

**A: **Straight rail (Full wall), Straight rail (not Full wall), L type, Trapezoidal, Arc.

&nbsp;

<u>**Q: What's the maximum weight of a single track?**</u>

**A: **50kg.

&nbsp;

<u>**Q: If there is no Wi-Fi, can Curtain work normally?**</u>

**A: **Curtain supports three control method: local button control, sensor linkage control and remote control by APP. After Wi-Fi is turned off, you can control the device by local button control and sensor linkage control.

&nbsp;

<u>**Q: After curtain and track are installed, what can I do if curtain can't be fully opened or closed, or half-closed? **</u>

**A: **1. Check whether the guide belt is installed; 2.  Check if something is blocking the curtain; 3. If installed properly, open APP, click "Setting"-"Installation guide"-"clear route", and then perform a complete opening and closing operation.

> Note: Don't do "clear route" frequently. After "clear route", please be sure to perform a complete opening and closing operation. Otherwise, the problem that cannot open or close the curtain normally may appear again.

&nbsp;

### **Temperature and Humidity Sensor**

<u>**Q: What is the Temperature and Humidity Sensor's reporting mechanism?**</u>

**A: **1) Temperature: the value changed over ±0.5℃ or report with humidity. 2) Humidity: the value changed over ±6% or report with temperature. 3) heartbeat report, heartbeat cycle is 60 minutes.

&nbsp;

<u>**Q: What is the maximum commiunication distance between sensors and hub?**</u>

**A: **The indoor commiunication distance is about 10 meters (including the distance from the wall), and the outdoor commiunication distance is about 65 meters.

&nbsp;

<u>**Q: How do I verify that the current installation location is appropriate?**</u>

**A: **After Temperature and Humidity Sensor connect successful, press button before install, if the hub voice prompt "connection is ok", it means current position is suitable. If you can't hear anything, please bring it close, then try again.

&nbsp;

<u>**Q: How to identify the same sensors that are bound on same gateway?**</u>

**A: **The device ID of same sensors that are bound on same gateway are different. You can rename these devices to distinguish on APP. For example, 

&nbsp;

### **Water Leak Sensor**

<u>**Q: How does Water Leak Sensor work?**</u>

**A: **Using the conductivity of water, when the two probes come into contact with water at the same time, a current loop is formed. At this time, the sensor reports the flooding/leakage status to the gateway, thereby triggering an alarm or associating with other smart devices to perform related actions.

&nbsp;

<u>**Q: Where is the reset button of Water Leak Sensor?**</u>

**A: **The Water Leak Sensor's button design is in the middle of the case. Hold the upper case and bottom case with your fingers and press the “water drop” icon to trigger the button.

&nbsp;

<u>**Q: Where is Water Leak Sensor's antenna?**</u>

**A: **The Water Leak Sensor's antenna uses an on-board antenna and the antenna is positioned directly above the "water drop" icon.

&nbsp;

<u>**Q: When does the water level reach the alarm?**</u>

**A: **When the detected water level reaches a height of 0.5mm, an alarm can be triggered.

&nbsp;
