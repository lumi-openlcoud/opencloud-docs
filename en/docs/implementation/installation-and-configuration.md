# Installation and Configuration Guide

## Preface

**Safety Statement**

Aqara products include electrical devices which should be installed and commissioned by qualified specialists according to electrical code and the Installation and Configuration Guide. During the installation, operation and maintenance of relevant device, local safety code and relevant operating rules must be followed; otherwise, personal injury or device damage may be caused. The safety precautions referred to in this document are a supplement to local safety code only. 



**Introduction**

| **Chapter **                     | **Description **                         |
| -------------------------------- | ---------------------------------------- |
| Preparations before Installation | Make a list of hardware devices to be prepared before installation. |
| Installation and LogIn Aqara App | Introduce how to download and install Aqara App and register an account. |
| Device installation              | Introduce installation precautions, requirements and processes of Aqara devices. |
| Add device                       | How to add Hub and Sub-device in Aqara App. |
| Scene and automation setting     | How to set scenes and automation in Aqara App so as to achieve intelligent linkage among different devices. |



## Preparations before Installation

Before installation and configuration , please make sure you have known the parameters and functions of Aqara products (Refer to [Product Introduction](http://docs.opencloud.aqara.cn/guideline/product-discription/) for details), and prepare the following hardware devices: 

| No.  | Device name                              | Quantity |
| ---- | ---------------------------------------- | -------- |
| 1    | Hub devices                              | >=1      |
| 2    | Controller and/or sensor devices         | Several  |
| 3    | Android or iPhone (iOS 8.0 or higher) smart phone | 1        |
| 4    | Wireless router with network access (2.4GHz) | 1        |



## Installation and LogIn Aqara App

Aqara App is a smart home App. All Aqara products are controlled through Aqara App which can achieve functions such as scene linkage and remote control among ZigBee devices. 

Specific operation steps are as follows: 

**Step1** Search "Aqara" in mobile App store or Apple Store, or scan the following QR code to download the App and install it on your phone. 

![appäşçť´ç ](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/implementation/installation-and-configuration/install_app_.png) 

**Step2** Open Aqara App, Click "New User Registration" button, enter your phone number or email address to obtain a verification code, then Click "Next Step".

> **Note:** Aqara App supports login bound to WeChat and Facebook. If WeChat or Facebook is installed on your phone, corresponding icon will Appear. 

**Step3** Set the login password and Click "Next Step". 

**Step4** In the "Registration Success" window, enter the name of your "Home" and Click "Confirm" to complete your registration. 




## Device installation

Aqara products include hubs, controllers and sensors. Hub products are central hubs that connect and manage various intelligent sensors and controllers. Control products are devices that complete actions according to relevant control instructions, such as power on and rotation, and are mainly used for the execution of scene and automation. Sensor products achieve automatic detection and control mainly through sensing temperature, humidity, photosensitivity and displacement etc. and are mainly used for home automation triggering. 

All devices shall be installed or placed properly according to practical environment before use so as to achieve intelligent linkage among them. 




### Hub devices

Now only one hub device that can be used directly on the Aqara APP is **Air Conditioning Controller (advanced)**.

In addition to Hub function, Air Conditioning Controller can also fast implement the intellectualization reconstruction of traditional air conditioner. Therefore, their installation position can be classified into two categories: first one is "Hub Function Priority" and the second one is "Air Conditioning Control Priority". Users can choose the installation position according to their actual needs. 



**Hub Function Priority**

Install Hub devices (Air Conditioning Controller) in the central position of the home as far as possible and keep it 2~6m away from the wireless router to ensure wireless signal coverage and the stability of the devices. 

> **Note:** For homes over 70 square meters, increase the quantity of Hubs in different areas according to the practical situation. 

![ç˝ĺłä˝ç˝Ž](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_gateway.png)

**Air Conditioning Controller Priority**

Plug the Air Conditioning Controller in the outlet for air conditioner (16A), then plug the air conditioner power plug in the outlet of the Air Conditioning Controller, and keep it 2 to 6m away from the wireless router to ensure wireless signal coverage and the stability of the devices.

> **Note:** Air Conditioning Controller controls the air conditioner with infrared. Therefore, there should be no barrier between the Air Conditioning Controller and the infrared panel of the air conditioner. 

![ĺŽčŁçŠşč°äź´äžŁ](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/implementation/installation-and-configuration/install_condition.png)




### **Control devices **

Precautions before installation: 

- **Control devices, except Plug, shall be installed and commissioned by qualified specialists according to electrical code and the user manual.**
- **Please be sure to install the devices after the main switch is turned off. The main switch can be turned on after the installation. **




#### **Wall Outlet**

The specific installation method is as follows: 

1. Turn off the main power switch, connect the neutral wire and live wire in the wall box to Hole N and L of the input terminal and connect the earth wire to middle hole and tighten the screw. 
2. Raise the switch shell with a straight screwdriver. 
3. Fix the switch to the wall junction box with a screw. 
4. Put down the switch shell. 
5. Turn on the main switch, now the blue status indicator flashes once, representing normal power on. Press the indicator on the shell, red status indicator flashes slowly and continuously, indicating that the switch is in a normal state but without network access. Please refer to "Device Network Access" chapter for specific network access method. 

Please refer to [Wall Outlet Installation Guide](https://www.aqara.com/cn/videointroduce.html) for video installation guide of wall outlet. 



#### **Plug**

Please connect the Plug directly to outlets within the area where the Hub signal covers. 



#### **Wall Switch(No Neutral)**

Take Wall Switch (Double Rocker) for example, each key on the Wall Switch controls one-way lamps. The specific installation method is as follows: 

![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_ctrl_neutral1.png)                      ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_ctrl_neutral3.png)

1. Turn off the main power switch, connect the live wire (red color) in the wall box to Hole L of the input terminal and connect the light wire (green or yellow color) to Hole L1 and L2 (Lamps connected with L1 and L2 are controlled with left key and right key of the switch respectively) and tighten the screw. 
2. Raise the switch shell with a straight screwdriver. 
3. Fix the switch to the wall junction box with a screw. 
4. Put down the switch shell. 
5. Turn on the main switch, now the blue status indicator flashes once, representing normal power on. Press the indicator on the shell, red status indicator flashes slowly and continuously, indicating that the switch is in a normal state but without network access. Please refer to "Device Network Access" chapter for specific network access method. 

> **Note:**
>
> - If the lamp has an old style tube with glow starter, please replace it with electronic glow starter. At present, Wall Switch(ZigBee) supports lamps of major brands, but for lamps of minor brands or supplied by counterfeit manufacturers, due to internal circuit design and material of the lamp, abnormal phenomena such as flashing, shutdown and crash may be caused, so you should be cautious when you select Wall Switch. 
> - Wall Switch (No Neutral) has one-way and two-way types, the load on each way shall not exceed 800W. 



#### **Wall Switch(With Neutral)**

Take Wall Switch (Double Rocker) for example, each key on the Wall Switch controls one-way lamps. The specific installation method is as follows: 

![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_ctrl_neutral1.png)                     ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_ctrl_neutral2.png)

1. Turn off the main power switch, connect the neutral wire and live wire in the wall box to Hole N and L of the input terminal and connect the light wire to Hole L1 and L2 (Lamps connected with L1 and L2 are controlled with left key and right key of the switch respectively) and tighten the screw. 
2. Raise the switch shell with a straight screwdriver. 
3. Fix the switch to the wall junction box with a screw. 
4. Put down the switch shell. 
5. Turn on the main switch, now the blue status indicator flashes once, representing normal power on. Press the indicator on the shell, red status indicator flashes slowly and continuously, indicating that the switch is in a normal state but without network access. Please refer to "Device Network Access" chapter for specific network access method. 

> **Note:**
>
> - Generally, live wire is red while lamp load line is green or yellow. It is suggested to install the wires after they are tested by a professional electrician with professional tools such as an electroprobe. Wall Switch (With Neutral, Double Rocker) has 4 connection holes, i.e., N, L, L1 and L2; N is connected with the neutral wire, L is connected with the live wire, L1 and L2 are connected with the load lines of the lamp. 
> - The maximum load of Wall Switch (With Neutral) is 2,500W (all ways). 



#### **Curtain Controller**

It is suggested to install by a professional worker. Please refer to [Curtain Controller Installation Guide](https://www.aqara.com/cn/videointroduce.html) for video installation guide of Curtain Controller. 



#### **Door Lock**

It is suggested to install Door Lock by a professional door lock worker. Please refer to [Door Lock Installation Guide](https://www.aqara.com/cn/videointroduce.html) for video installation guide of Door Lock. 




#### **Thermostat**

Please refer to the user manual attached for detailed installation guide. Generally, Thermostat shall be installed at a position that is 1.5m above the ground, except: 

1. At the wall corner, by the door or window, on the door, or behind the door;
2. In a non-temperature control space, enclosed heating pipeline and flue; 
3. Next to a cold or hot air duct or a radiator;
4. Next to a place exposed to direct sunlight or to air flow or other heating unit (such as TV); 
5. Covered by a metal object, in order to avoid any impact on wireless communication. 

> **Note:** If an external Temperature and Humidity Sensor is adopted, position 1, 2 and 3 may not be taken into consideration. 



#### **Wireless Relay Controller(2 Channels)**

Please refer to the user manual attached for detailed installation guide. 



#### **Dimmer**

Please refer to the user manual attached for detailed installation guide. 



### **Sensor devices **

Precautions before installation: 

- Sensor devices shall be installed in the signal coverage area of hub devices; otherwise, normal connection is unavailable. 
- Keep the surface clean and dry. Do not install sensor devices on a metal surface; otherwise, wireless communication distance may be affected. 
- To ensure that the security and convenient adjustment of the sticker, do not compact it when it is first installed. Just attach the device gently to a designated position (make sure the device will not drop). After it is confirmed that the device is installed correctly by tests, compact and stick the device. 
- If the position of the sensor has to be adjusted, remove the device from left or right side; do not pull it vertically because it is difficult to remove the device and the viscosity of the sticker may be reduced by doing so.  
- Do not drop it when installing since the body of the sensor may be damaged. 



#### **Door and Window Sensor**

The installation method is as follows: 

1. Remove the protective film of the sticker.


2. Try to align with the installation label by the side of the body and the magnet when installing.  
3. Stick the sensors at the opening/closing area and ensure not to prevent the door/window from opening/closing normally (It is suggested to install the body on the fixed surface of the opening/closing area and install the magnet on the movable surface. The installation clearance shall be less than **22mm** when the door/window is closed). 

![é¨çŞäź ćĺ¨ĺŽčŁć­ĽéŞ¤](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_sensor_magnet.png)



#### **Motion Sensor**

There are three installation modes for Motion Sensor: 

- Place the sensor where you need it. 
- Remove the protective film (provided with round sticker), and stick it directly to the desired position.
- Stick the Motion Sensor to the stand, and stick the stand to where you need it.

> **Note:**
>
> 1. The recommended installation height is 1.2m to 2.1m. If the installation height is below 1.2 m, the detection area will decrease but the use will not be affected; if above 2.1 m, the detection area might have blind spots. 
> 2. Note that the lens should be aligned with the detection area when installing, and placed or pasted as close as possible to the edge of a table or cabinet.

​                                    ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_sensor_motion1.png)          ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_sensor_motion2.png)



#### **Temperature and Humidity Sensor**

There are two installation modes for Temperature and Humidity Sensor: 

- Place the sensor where you need it. 
- Remove the protective film (provided with round sticker), and stick it directly to the desired position.



#### **Water Leak Sensor**

There are two installation modes for Water Leak Sensor:

- Place it directly on the surface where you need to monitor water leak. 
- Use hex screwdriver to loosen the probes, and connect to cables for more smart scenes.

> **Note:** The antenna is right above the "water drop" icon. 



#### **Natural Gas Detector**

Precautions before installation: 

1. Natural Gas Detector can start the gas solenoid valve or the exhaust fan by direct linkage. Be sure to install it by a **specialist**. 

2. The Natural Gas Detector shall be installed in a room with combustible gas (such as natural gas and other combustible gas that contains methane), e.g., kitchen. 

3. The power cord of the Detector is 1.5m long. Make sure that there are 220V~ power sockets within a 1.5m radius from the installation location. 

4. It is for indoor use only. 

5. There should be no barrier between the detector and gas Appliances to avoid influence on the detection. 

6. It should not be installed at a position with large air flow, such as air let, ventilating fan, window and room, to avoid influence on the detection. 

##### Installation position and limitation  

- **Wall-mounted**: The horizontal distance between the Natural Gas Detector and the gas Appliance or gas valve is L (1m<L<4m); the distance between the Natural Gas Detector and the ceiling is D (D <0.3m). Do not install the Natural Gas Detector right above the gas Appliance. 

  ![ĺŁćĺŽčŁ](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_sensor_natgas1.png)

- **Ceiling-mounted**: The horizontal distance between the Natural Gas Detector and the gas Appliance or gas valve is L (1m<L<4m); Do not install the Natural Gas Detector right above the gas Appliance.

  ![ĺ¸éĄśĺŽčŁ](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_sensor_natgas2.png)

##### Installation Mode and Step

> **Note:** To ensure the installation security of Natural Gas Detector, it is recommended to install it with screw. Please use caution when you adopt to install it with sticker since any accident and loss caused therefrom shall be borne by yourself. 

- **Screw installation mode**

  1.Press on the position circled in the figure below with your thumb, then rotate counterclockwise to remove the base plate. 

  ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/implementation/installation-and-configuration/install_sensor_natgas3.png)

  2.Select an appropriate installation position and use a pencil to make the drilling position.

  ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/implementation/installation-and-configuration/install_sensor_natgas4.png)

  3.Use an impact drill (bit diameter: 6mm) to drill a hole at the marked position, and then insert the expansion rubber stopper into the hole. 

  4.Use apping screw to fix the base plate and press the body of the detector on the base plate, and then rotate clockwise to clip the detector into the base plate. 

  ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/implementation/installation-and-configuration/install_sensor_natgas5.png)

  5.Use the attached adaptor to provide power input for the detector. 

- **Sticking installation mode**

  1.Stick the double-sided 3M sticker in the box to the base plate on the back of the product (without removing the base plate). 

  2.Clean the surface of the installation position, remove the protective film and stick it to the installation position.

  3.Use the attached adaptor to provide power input for the detector.

##### Installation verification

- Press the button once. If the Hub makes voice prompts of "Normal Link Confirmed", it indicates that the detector is installed successfully.
- Press the button of the detector for over 3s. The detector starts to simulate the alarm (i.e., make a high decibel alarm sound). 



#### **Smoke Alarm**

Precautions before use: 

1. It is for indoor use on the ceiling only. 


2. It should not be installed in the bathroom or at a position that is easy to be stained with water or a position with water drop. 


3. It should not be installed at a position where vapor or other non-fire smoke is easy to be formed. 


4. The distance between the alarm and lighting equipment shall be greater than 50cm. 


5. The distance between the alarm and the ventilating fan, air conditioner and air vent shall be greater than 150cm. 


6. If the cabinet is close to the ceiling, the distance between the alarm and the cabinet shall be greater than 60cm. 

##### Installation Mode and Step

> **Note:** To ensure the installation security of Natural Gas Detector, it is recommended to install it with screw. Please use caution when you adopt to install it with sticker since any accident and loss caused therefrom shall be borne by yourself. 

- **Screw installation mode **

  1.Select an appropriate installation position and use a pencil to make the drilling position.

  ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/implementation/installation-and-configuration/install_sensor_smoke1.png)

  2.Use an impact drill (bit diameter: 6mm) to drill a hole at the marked position, and then insert the expansion rubber stopper into the hole.

  ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_sensor_smoke2.png)

  3.Press the body of the alarm on the base plate, and then rotate clockwise to clip the detector into the base plate.

  > **Note:** If no batteries are mounted on the alarm, the body of the alarm can't be clipped into the base plate. 

  ![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/install_sensor_smoke3.png)

- **Sticking installation mode**

  1.Stick the double-sided 3M sticker in the box to the base plate on the back of the product

  2.Clean the surface of the installation position, remove the protective film and stick it to the installation position.

  3.Press the body of the alarm on the base plate, and then rotate clockwise to clip the detector into the base plate.

  > **Note:** If no batteries are mounted on the alarm, the body of the alarm can't be clipped into the base plate.

##### Installation verification

- Press the button once. If the Hub makes voice prompts of "Normal Link Confirmed", it indicates that the detector is installed successfully.
- Press the button of the detector for over 3s. The detector starts to simulate the alarm (i.e., make a high decibel alarm sound). 



#### **Wireless Switch**

There are three installation modes for wireless switch: 

- Place the wireless switch where you need it. 
- Remove the protective film and stick it directly to the desired position.
- Drill a hole on the wall and fix the Wireless Mini Switch with screws (Only Wireless Remote Switch supports this installation mode). 




## Add device

In the add device stage, Aqara products can be classified into two categories: Hub device and Sub-device. Hub device is a device with hub function and is used to connect and manage multiple sensors and controllers. Sub-device includes sensors and controllers. All Sub-devices must be used together with hub devices. Therefore, before setting Sub-device on the App, you must set hub device to ensure the normal wireless communication between Wi-Fi and ZigBee. 



### **Hub device**

Add hub device to the App as follows: 

**Step1** Choose device 

Open Aqara App, Click "+" in the top right corner of the "Device List" interface, select corresponding hub device under the Hub menu, and set the device according to the prompt. 

**Step2** Connect Wi-Fi

Select the Wi-Fi to be connected, enter the password, click "Next Step", connect Wi-Fi according to the prompt on the phone, and then return to Aqara App. 

> **Note:** The system can't identify Wi-Fi that contains special characteristics. If a Wi-Fi contains special characteristics, please change the name and then reconnect. If the connection failed again, please reset the device or change the Wi-Fi and reconnect. If the Wi-Fi is unstable, it is suggested to use a 4G phone as the Wi-Fi hotspot. 

**Step3** Set device information 

Select device installation position according to the prompt and enter device name to complete the setting. 

> **Note:** Do not use the same name for several devices to avoid trouble caused by repeated device name. It is suggested to name the devices according to their locations, such as living room-Hub, bedroom-Air.

**Step4** Pair with air conditioner (This operation is for Air Conditioning Controller only.) 

After adding Air Conditioning Controller successfully, you should pair it with the air conditioner manually to enable the control function: 

1. Open the "Air Conditioning Controller", click "Pair With Air Conditioner Manually", select air conditioner brand, click "Confirm". 

2. Select corresponding air conditioner according to "Remote Control No." and click "Start Test". 
3. When the test is passed, the air conditioner is paired successfully.



### **Sub-device**

Add sub-device to the App as follows: 

**Step1** Choose sub-device 

Open Aqara App, Click "+" in the top right corner of the "Device List" interface, select corresponding hub device under the Hub menu, and set the device according to the prompt. 

**Step2** Set device information 

Select device installation position according to the prompt and enter device name to complete the setting. 

> **Note:** Do not use the same name for several devices to avoid trouble caused by repeated device name. It is suggested to name the devices according to their locations, such as living room-plug, bedroom-plug.

**Step3** Test the connection state 

After adding sub-device successfully, press the reset button of the sensor or controller once or short press the reset button with the reset needle. If the Hub makes voice prompts of "Normal Link Confirmed", it indicates that the Sub-device and the Hub can communicate effectively.



## Scene and automation setting

Aqara device supports two setting modes in the App: scene and automation. 

- **Scene**: The user can perform custom setting on actions that need to be executed in the scene. When people trigger the scene, set actions will be executed in proper order. 
- **Automation**: The user can perform customs setting on trigger conditions and actions to be executed. When trigger conditions are met, set actions will be executed.

The differences between scene and automation are as followsďź

| **Differences** | **Scene**                                | **Automation**                           |
| --------------- | ---------------------------------------- | ---------------------------------------- |
| Trigger mode    | Trigger manually                         | Trigger automatically when set conditions are met |
| Device          | Only Hub and controller devices can be controlled; sensor devices cannot be controlled. | All devices                              |
| Action executed | Automation cannot be an executed action of scene | Scene can be an executed action of automation |



### **Set a scene **

Set a scene as follows:

1.Open Aqara App, click "Device List" to go to "Scene".

2.Click "+" in the top right corner, select to use system default scene or add a custom scene. 

- Default scene (Get up, Get home, Leave home, Sleep): Select any default scene and then enter "Edit Scene" page. On the page, there are relevant default actions, you may delete actions you don't need and add actions you need. 
- Custom scene: Click "Add Custom Scene" at the bottom of the page, enter "Scene Name", click "+" behind the action to add the action. 

3.After adding the action, click "Try" in the top right corner for confirmation of the set effect. 

4.Click "Save" to complete the setting. The name of the scene saved is displayed on the page of "Scene" and the icon of the scene saved is displayed in the control item on the "Home Page". 

5.There are two ways to execute a scene: 

- On "Scene" page, click "Execute Once" button behind the scene to be executed. 
- In the control item on the "Home Page", click the icon of the scene to be executed. 

6.Delete a scene. On scene page, click the scene to be deleted and then click "detele" in the top right corner of the "Edit Scene" page. 

> Note: 
>
> 1. When adding actions, please pay attention to the execution order of the actions and execute them in proper order from top to bottom. 
>
>
> 2. It is suggested to set different icons and names for different scenes so that such scenes will not be confused due to the same icon they use. The icon can be changed as follows: On scene page, click the scene for which the icon should be changed, click "..."-"Change Icon" in the top right corner of "Edit Scene" page. 



### **Set an automation**

Set an automation as follows: 

1.Open Aqara App, click "Automation". 

2.Click "+" in the top right corner, use the automation recommended by the system, or create a custom automation: 

- Automation recommended by the system (Home Arm and Away Arm): Select any automation and enter "Edit Automation" page. On the page, there are default set conditions and actions to be executed, you may add or delete the settings according to your need.  
- Create a custom automation: Click "Create a Custom Automation" at the bottom of the page, enter "Automation Name", click "+", set activating conditions and actions to be executed. 

3.Click "Save" to complete the setting. 

4.On "Automation" page, automation can be activated by moving the slider behind the automation name. When the set automation conditions are set, corresponding action will be executed automatically. 

5.Delete an automation. On automation page, click the automation to be deleted and then click "Delete" in the top right corner of the "Edit Automation" page. 

### **Examples **

**Scene** (Take "Go home" scene for example)

Requirement: When you get home, close the indoor camera and turn on the lamp in the living room, air conditioner and the TV. The execution sequence is shown in the figure below. 

![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/example1_.png)

Open Aqara App, enter "Device List"-"Scene" page, click "+" in the top right corner to add a scene. System default scenes include Go home scene. You can use and set this scene directly, or add a new custom scene. Here explain this by taking "Add a Custom Scene" for example. 

1.Click "Add a Custom Scene" at the bottom of the page and enter the name of the scene as "Go home". 

2.After clicking "+" behind "Add Actions", set actions in proper order according to the flow chart: 

- Click "Camera Hub", and select "Turn off". 
- Click "Wall Switch" connected with the lamp in the living room, and select "Turn on". 
- Click "Air Conditioning Controller", and select "Turn on". 
- Click "Plug" connected with the TV, and select "Turn on". 

3.Click "Try" to verify whether actions for the scene can be executed correctly. 

4.Click "Save" to complete the setting. 


**Automation **(Take "Away Arm" for example) 

Requirement: During office hours on a working day (from 9:00 am to 6:00 pm), when the door or window is opened or someone is detected in the room, the Hub will sound the alarm and meanwhile send alert notifications to App on the mobile phone and alarm short message to the mobile phone. Make a list of "Activating Conditions" and "Actions to Be Executed" according to needs, as shown in the figure below. 

![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/en/implementation/installation-and-configuration/example20.png)

Open Aqara App, enter "Automation" page, click "+" in the top right corner to add an automation. There is an Away Arm automation by system default. You can use and set this automation directly, or add a new custom automation. Here explain this by taking "Add a Custom Automation" for example.  

1.Click "Add a Custom Automation" at the bottom of the page and enter the name of the automation as "Away Arm". 

2.Activate conditions and select "Meet any of the Conditions", Click "+" to add specific conditions. 

- Click "Door and Window Sensor", and select "The Moment When the Door or Window is Opened". 
- Click "Motion Sensor", and select "The Moment When Someone Passes By". 

3.After adding the specific conditions, click the icons behind "Door and Window Sensor" and "Motion Sensor", select "Period", set "Repeat setting" as "From Monday to Friday", set the "Starting Time" as "09:00", and set the "Ending Time" as "18:00". 

4.Click "+" behind "Execute in Proper Order" and add actions to be executed. 

- Click "Air Conditioning Controller", select "Alarm", set alarm ring (Police Car No. 1) and volume (60), Click "Confirm". 
- Click "Send the Information to Mobile Phone", select "Send Alarm Information + Short Message". 

5.Click "Save" to complete the setting. 