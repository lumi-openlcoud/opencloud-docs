# Scene and Automation API Documation

## Examples

### Scene

Example: Turn on light and set the light color to red.

```
{
	"name": "scene-test",
	"positionId":"real1.xxxxxxxx",	
	"content": [
		{
			"action": "AD.lumi.gateway.set_corridor_light_argb",
			"delayTime": "0",
			"did": "lumi.xxxxxxxxxx",
			"model": "lumi.gateway.aq1",
			"params": [{
				"paramId":"PD.lightARGB",
				"value":"520028160"
			}]
}]
}
```



### Automation

Example: Turn on light when someone passes by.

```
{
	"name" : "test",
	"positionId":"real1.xxxxxxxxx",	
	"conditionRelation": "0",
	"conditions": [
		{
			"trigger": "TD.lumi.sensor_motion.motion",
            "did": "lumi.xxxxxx",
            "model":"lumi.sensor_motion.aq1",
			"params": []
		}
		],
		"actions": [
			{
				"did": "lumi.xxxxx",
				"action": "AD.lumi.gateway.open_corridor_light",
				"model": "lumi.gateway.aq1",
		     	"params": []
			}
			]
}
```



If Trigger and Action needs more detail infomation, you need to set “paramId” and “value”.

Example: Turn on light when temperature is up to 22℃.

```
{
	"name" : "test",
	"positionId":"real1.xxxxxxxxx",	
	"conditionRelation": "0",
	"conditions": [
		{
			"trigger": "TD.lumi.weather.temp_more_than_instant",
            "did": "lumi.xxxxxx",
            "model":"lumi.weather.v1",
			"params": [
                      {
                            "paramId":"PD.temp",
                            "value":"22"
                      }
			]
		}
		],
		"actions": [
			{
				"did": "lumi.xxxxx",
				"action": "AD.lumi.gateway.open_corridor_light",
				"model": "lumi.gateway.aq1",
		     	"params": []
			}
			]
}
```



Configuring single parameter information:

Example: Turn on light and set the light color to red, when someone passes by.

```
{
	"name" : "test",
	"positionId":"real1.xxxxxxxxx",	
	"conditionRelation": "0",
	"conditions": [
		{
			"trigger": "TD.lumi.sensor_motion.motion",
            "did": "lumi.xxxxxx",
            "model":"lumi.sensor_motion.aq1",
			"params": []
		}
		],
		"actions": [
			{
				"did": "lumi.xxxxx",
				"action": "AD.lumi.gateway.set_corridor_light_argb",
				"model": "lumi.gateway.aq1",
		     	"params": [{
		     		"paramId":"PD.lightARGB",
		     		"value":"520028160"
		     	}]
			}
			]
}
```



Configuring multiple parameter information:

Play specified music when click switch.

```
{
	"name" : "test",
	"positionId":"real1.xxxxxxxxx",	
	"conditionRelation": "0",
	"conditions": [
		{
			"trigger": "TD.lumi.sensor_switch.click",
            "did": "lumi.xxxxxx",
            "model":"lumi.sensor_switch.aq3",
			"params": []
		}
		],
		"actions": [
			{
				"did": "lumi.xxxxx",
				"action": "AD.lumi.gateway.play_music",
				"model": "lumi.gateway.aq1",
		     	"params": [{
		     		"paramId":"PD.musicIndex",
		     		"value":"11"
		     	},
		     	{
		     		"paramId":"PD.vol",
		     		"value":"10"
		     	}]
			}
			]
}
```



## List of Trigger and Action

### Hub

**Trigger**

| name         | Trigger   |
| ------------ | --------- |
| Light is dim | low_light |
| Alarm        | on_alarm  |



**Action**

| name                    | Action                  |
| ----------------------- | ----------------------- |
| Turn on night light     | open_corridor_light     |
| Turn off night light    | close_corridor_light    |
| Turn on/off night light | toggle_corridor_light   |
| Set night light color   | set_corridor_light_argb |
| Play assigned ringtone  | play_music              |

Some Actions need to configure specific parameter information:

- **set_corridor_light_argb**

  | paramId      | value                                    |
  | ------------ | ---------------------------------------- |
  | PD.lightARGB | Night lightARGB. Format: lightness+RGB, ranges: ~0x64FFFFFF, decimal |

- **play_music**

  | paramId       | value                     |
  | ------------- | ------------------------- |
  | PD.musicIndex | Index of music: 0~8，10~13 |
  | PD.vol        | Volume, ranges: 0~100     |

  ​

### Air Conditioning Controller (Advanced)

**Trigger**

| name         | Trigger   |
| ------------ | --------- |
| Light is dim | low_light |
| Alarm        | on_alarm  |



**Action**

| name                      | Action           |
| ------------------------- | ---------------- |
| Turn on AC                | open_acpartner   |
| Turn off AC               | close_acpartner  |
| Turn on/off AC            | toggle_acpartner |
| Set AC to assigned status | ctrl_acpartner   |
| Play assigned ringtone    | play_music       |

Some Actions need to configure specific parameter information:

- **ctrl_acpartner**

  | paramId   | value                                    |
  | --------- | ---------------------------------------- |
  | PD.status | Base on the value to decide the switch, wind speed, mode, and so on. For detail, see [Resource definition](http://docs.opencloud.aqara.cn/en/development/cloud-development/#resource-definition). |

- **play_music**

  | paramId       | value                           |
  | ------------- | ------------------------------- |
  | PD.musicIndex | Index of music: 0~8，10~13，20~33 |
  | PD.vol        | Volume, ranges: 0~100           |

  ​

### Wall Outlet (Zigbee Version)

**Trigger**

| name              | Trigger          |
| ----------------- | ---------------- |
| When it turns on  | trigger_turn_on  |
| When it turns off | trigger_turn_off |
| On                | trigger_on       |
| Off               | trigger_off      |



**Action**

| name   | Action |
| ------ | ------ |
| On     | open   |
| Off    | close  |
| On/Off | toggle |



### Smart Plug (Zigbee Version)

**Trigger**

| name              | Trigger          |
| ----------------- | ---------------- |
| When it turns on  | trigger_turn_on  |
| When it turns off | trigger_turn_off |
| On                | trigger_on       |
| Off               | trigger_off      |



**Action**

| name   | Action |
| ------ | ------ |
| On     | open   |
| Off    | close  |
| On/Off | toggle |



### Wall Switch (Single Rocker)

**Trigger**

| name              | Trigger          |
| ----------------- | ---------------- |
| When it turns on  | trigger_turn_on  |
| When it turns off | trigger_turn_off |
| On                | trigger_on       |
| Off               | trigger_off      |



**Action**

| name   | Action |
| ------ | ------ |
| On     | open   |
| Off    | close  |
| On/Off | toggle |



### Wall Switch (Double Rocker)

**Trigger**

| name                        | Trigger                |
| --------------------------- | ---------------------- |
| When left button turns on   | trigger_left_turn_on   |
| When left button turns off  | trigger_left_turn_off  |
| Left button is on           | trigger_left_on        |
| Left button is off          | trigger_left_off       |
| When right button turns on  | trigger_right_turn_on  |
| When right button turns off | trigger_right_turn_off |
| Right button is on          | trigger_right_on       |
| Right button is off         | trigger_right_off      |



**Action**

| name                | Action       |
| ------------------- | ------------ |
| Left button on      | left_open    |
| Left button off     | left_close   |
| Left button on/off  | left_toggle  |
| Right button on     | right_open   |
| Right button off    | right_close  |
| Right button on/off | right_toggle |



### Curtain

**Action**

| name                      | Action      |
| ------------------------- | ----------- |
| Open curtain              | open        |
| Close curtain             | close       |
| Pause curtain             | stop        |
| Open to assigned position | open_degree |

Some Actions need to configure specific parameter information:

- **open_degree**

  | paramId     | value                                    |
  | ----------- | ---------------------------------------- |
  | PD.position | Opening percentage of curtain ,ranges: 0~100 |

  ​

### Lock

**Trigger**

| name                       | Trigger                 |
| -------------------------- | ----------------------- |
| Reverse locked             | reverse_locked          |
| Unlock by any fing         | unlock_any_fing         |
| Unlock by someone fing     | unlock_someone_fing     |
| Unlock by any card         | unlock_any_card         |
| Unlock by someone card     | unlock_someone_card     |
| Unlock by any password     | unlock_any_password     |
| Unlock by someone password | unlock_someone_password |
| Lock picking               | pick_lock               |
| Multiple unlock error      | verified_wrong          |

Some Triggers need to configure specific parameter information:

- **unlock_someone_fing**

  | paramId    | value           |
  | ---------- | --------------- |
  | PD.lockUID | Fingerprint UID |

- **unlock_someone_card**

  | paramId    | value    |
  | ---------- | -------- |
  | PD.lockUID | Card UID |

- **unlock_someone_password**

  | paramId    | value        |
  | ---------- | ------------ |
  | PD.lockUID | Password UID |


- **verified_wrong**

  | paramId       | value                      |
  | ------------- | -------------------------- |
  | PD.errorCount | Error number, ranges: 3~10 |

  ​

**Action**

| name          | Action        |
| ------------- | ------------- |
| Adjust volume | adjust_volume |

Some Actions need to configure specific parameter information:

- **adjust_volume**

  | paramId    | value                                    |
  | ---------- | ---------------------------------------- |
  | PD.lockVol | "High pitch":1，"Middle pitch":2，"Low pitch":3，"Mute":4 |

  ​

### Thermostat

**Trigger**

| name              | Trigger            |
| ----------------- | ------------------ |
| When it turns on  | trigger_turn_on    |
| When it turns off | trigger_turn_off   |
| On                | trigger_on         |
| Off               | trigger_off        |
| In assigned mode  | trigger_in_x_model |

Some Triggers need to configure specific parameter information:

- **trigger_in_x_model**

  | paramId    | value            |
  | ---------- | ---------------- |
  | PD.acmodel | "Hot":0,"Cold":1 |

**Action**

| name                          | Action                    |
| ----------------------------- | ------------------------- |
| Turn on AC                    | hvac_on                   |
| Turn off AC                   | hvac_off                  |
| Set fan speed to auto mode    | set_wind_speed_auto_model |
| Turn on AC to assigned status | hvac_ctrl                 |

Some Actions need to configure specific parameter information:

- **hvac_ctrl**

  | paramId   | value                                    |
  | --------- | ---------------------------------------- |
  | PD.status | Base on the value to decide the switch, wind speed, mode, and so on. For detail, see [Resource definition](http://docs.opencloud.aqara.cn/en/development/cloud-development/#resource-definition). |



### Wireless Relay Controller (2 Channels)

**Trigger**

| name                     | Trigger              |
| ------------------------ | -------------------- |
| When turn on channel #1  | trigger_ch1_turn_on  |
| When turn off channel #1 | trigger_ch1_turn_off |
| When turn on channel #2  | trigger_ch2_turn_on  |
| When turn off channel #2 | trigger_ch2_turn_off |
| Channel #1 is on         | trigger_ch1_on       |
| Channel #1 is off        | trigger_ch1_off      |
| Channel #2 is on         | trigger_ch2_on       |
| Channel #2 is off        | trigger_ch2_off      |



**Action**

| name                   | Action          |
| ---------------------- | --------------- |
| Turn on channel #1     | channel1_on     |
| Turn off channel #1    | channel1_off    |
| Turn on/off channel #1 | channel1_toggle |
| Turn on channel #2     | channel2_on     |
| Turn off channel #2    | channel2_off    |
| Turn on/off channel #2 | channel2_toggle |



### Dimmer

**Trigger**

| name                | Trigger              |
| ------------------- | -------------------- |
| Channel #1 Turn on  | trigger_ch1_turn_on  |
| Channel #1 Turn off | trigger_ch1_turn_off |
| Channel #2 Turn on  | trigger_ch2_turn_on  |
| Channel #2 Turn off | trigger_ch2_turn_off |
| Channel #3 Turn on  | trigger_ch3_turn_on  |
| Channel #3 Turn off | trigger_ch3_turn_off |
| Channel #1 on       | trigger_ch1_on       |
| Channel #1 off      | trigger_ch1_off      |
| Channel #2 on       | trigger_ch2_on       |
| Channel #2 off      | trigger_ch2_off      |
| Channel #3 on       | trigger_ch3_on       |
| Channel #3 off      | trigger_ch3_off      |



**Action**

| name                | Action       |
| ------------------- | ------------ |
| Turn on channel #1  | turn_on_ch1  |
| Turn off channel #1 | turn_off_ch1 |
| Toggle channel #1   | toggle_ch1   |
| Turn on channel #2  | turn_on_ch2  |
| Turn off channel #2 | turn_off_ch2 |
| Toggle channel #2   | toggle_ch2   |
| Turn on channel #3  | turn_on_ch3  |
| Turn off channel #3 | turn_off_ch3 |
| Toggle channel #3   | toggle_ch3   |



### Door and Window Sensor

**Trigger**

| name                    | Trigger     |
| ----------------------- | ----------- |
| When door/window opens  | open        |
| When door/window closes | close       |
| Open                    | opening     |
| Close                   | closing     |
| Open in ...             | open_until  |
| Close in ...            | close_until |

Some Triggers need to configure specific parameter information:

- **open_until**

  | paramId     | value                                   |
  | ----------- | --------------------------------------- |
  | PD.duration | Duration, ranges: 1~1439, unit: minute. |


- **close_until**

  | paramId     | value                                   |
  | ----------- | --------------------------------------- |
  | PD.duration | Duration, ranges: 1~1439, unit: minute. |



### Motion Sensor

**Trigger**

| name                                     | Trigger                    |
| ---------------------------------------- | -------------------------- |
| Motion detected                          | motion                     |
| No motion detected in ...                | no_motion_xx               |
| Motion detected and illuminance above set value | motion_illumination_higher |
| Motion detected and illuminance below set value | motion_illumination_below  |

Some Triggers need to configure specific parameter information:

- **no_motion_xx**

  | paramId     | value                                   |
  | ----------- | --------------------------------------- |
  | PD.duration | Duration, ranges: 1~1439, unit: minute. |


- **motion_illumination_higher**

  | paramId         | value                                   |
  | --------------- | --------------------------------------- |
  | PD.illumination | illumination, ranges: 1~1000, unit: Lx. |


- **motion_illumination_below**

  | paramId         | value                                   |
  | --------------- | --------------------------------------- |
  | PD.illumination | illumination, ranges: 1~1000, unit: Lx. |



### Temperature and Humidity Sensor

**Trigger**

| name                  | Trigger                |
| --------------------- | ---------------------- |
| Temprature climbs to  | temp_more_than_instant |
| Temprature drops to   | temp_less_than_instant |
| Above set temprature  | temp_more_than         |
| Below set temperature | temp_less_than         |
| Humidity climbs to    | humi_more_than_instant |
| Humidity drops to     | humi_less_than_instant |
| Above set humidity    | humi_more_than         |
| Below set humidity    | humi_less_than         |

Some Triggers need to configure specific parameter information:

- **temp_more_than_instant**

  | paramId | value                                |
  | ------- | ------------------------------------ |
  | PD.temp | Temprature, ranges: -40~125, unit: ℃ |


- **temp_less_than_instant**

  | paramId | value                                |
  | ------- | ------------------------------------ |
  | PD.temp | Temprature, ranges: -40~125, unit: ℃ |


- **temp_more_than**

  | paramId | value                                |
  | ------- | ------------------------------------ |
  | PD.temp | Temprature, ranges: -40~125, unit: ℃ |


- **temp_less_than**

  | paramId | value                                |
  | ------- | ------------------------------------ |
  | PD.temp | Temprature, ranges: -40~125, unit: ℃ |


- **humi_more_than_instant**

  | paramId | value                    |
  | ------- | ------------------------ |
  | PD.humi | Humidity, ranges: 0~100% |


- **humi_less_than_instant**

  | paramId | value                    |
  | ------- | ------------------------ |
  | PD.humi | Humidity, ranges: 0~100% |


- **humi_more_than**

  | paramId | value                    |
  | ------- | ------------------------ |
  | PD.humi | Humidity, ranges: 0~100% |


- **humi_less_than**

  | paramId | value                    |
  | ------- | ------------------------ |
  | PD.humi | Humidity, ranges: 0~100% |



### Water Leak Sensor

**Trigger**

| name                   | Trigger |
| ---------------------- | ------- |
| Alarm when water leaks | leak    |
| Disable alarm          | unleak  |



### Natural Gas Detector

**Trigger**

| name                                     | Trigger |
| ---------------------------------------- | ------- |
| Alarm when gas density exceeds standard level | alarm   |



### Smoke Alarm

**Trigger**

| name                                     | Trigger |
| ---------------------------------------- | ------- |
| Alarm when smoke density exceeds standard level | alarm   |



### Wireless Mini Switch

**Trigger**

| name         | Trigger      |
| ------------ | ------------ |
| Single press | click        |
| Double press | double_click |



### Wireless Mini Switch(Advanced)

**Trigger**

| name         | Trigger          |
| ------------ | ---------------- |
| Single press | click            |
| Double press | double_click     |
| Long press   | long_click_press |
| Shake        | shake_air        |



### Wireless Remote Switch (Single Rocker)

Device model: lumi.sensor_86sw1.aq1和lumi.sensor_86sw1.v1

**Trigger**

| name         | Trigger |
| ------------ | ------- |
| Single press | click   |



Device model: lumi.remote.b186acn01

**Trigger**

| name         | Trigger   |
| ------------ | --------- |
| Single press | click_ch0 |



### Wireless Remote Switch (Double Rocker)

Device model: lumi.sensor_86sw2.aq1和lumi.sensor_86sw2.v1

**Trigger**

| name                         | Trigger          |
| ---------------------------- | ---------------- |
| Single press the left button | left_click       |
| Single press right button    | right_click      |
| Single press both buttons    | left_right_click |



Device model: lumi.sensor_86sw2.aq1和lumi.sensor_86sw2.v1

**Trigger**

| name                         | Trigger    |
| ---------------------------- | ---------- |
| Single press the left button | click_ch0  |
| Single press right button    | click_ch1  |
| Single press both buttons    | both_click |



### Cube

**Trigger**

| name                           | Trigger   |
| ------------------------------ | --------- |
| Flip 90°                       | flip90    |
| Flip 180°                      | flip180   |
| Shake                          | shake_air |
| Rotate                         | rotate    |
| Tap twice                      | tap_twice |
| Push                           | move      |
| Motion detected after a minute | alert     |

### Vibration Sensor

**Trigger**

| name               | Trigger |
| ------------------ | ------- |
| Drop detected      | drop    |
| Tilt detected      | tilt    |
| Vibration detected | vibrate |

