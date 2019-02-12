# 场景和自动化接口文档

## 示例

### 场景

例如：回家开空调。

```
{
	"name": "DEMO-A",
	"content": [
		{
			"action": "open_acpartner",
			"delayTime": "0",
			"did": "lumi.158d0001a61234"
}]
}

```



### 自动化

例如：当有人经过时，开启夜灯。

```
{
	"name" : "DEMO-B",
	"conditions": [
		{
			"did": "lumi.158d00010f1234",
			"trigger": "motion"
		}
		],
		"actions": [
			{
				"did": "lumi.158d0001b11234",
				"action": "open_corridor_light"
			}
			]
}
```



若Trigger和Action需要配置详细参数信息，则需设置“paramId”和“value”。

例如：当温度上升到22度时，开启夜灯。

```
{
	"name" : "DEMO-C",
	"conditions": [
		{
			"did": "lumi.158d00010f1234",
			"trigger": "temp_more_than_instant",
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
				"did": "lumi.158d0001b11234",
				"action": "open_corridor_light"
			}
			]
}
```



配置单个参数信息：

当有人经过的时候，开启夜灯，且设置夜灯颜色为红色。

```
{
	"name" : "DEMO-D",
	"conditionRelation": "AND",
	"conditions": [
		{
			"trigger": "motion",
            "did": "lumi.158d0001241234"
		}
		],
		"actions": [
			{
				"did": "lumi.158d0001b11234",
				"action": "set_corridor_light_argb",
				"params": [
                            {
                            	"paramId":"PD.lightARGB",
                            	"value":"520028160"
                            }
                            ]
			}
			]
}
```



配置多个参数信息：

当有人经过时，播放指定铃声。

```
{
	"name" : "DEMO-E",
	"conditionRelation": "AND",
	"conditions": [
		{
			"trigger": "motion",
            "did": "lumi.158d0001241234"
		}
		],
		"actions": [
			{
				"did": "lumi.158d0001b11234",
				"action": "play_music",
				"params": [
                            {
                            	"paramId":"PD.musicIndex",
                            	"value":"1"},
                            	{
                            	"paramId":"PD.vol",
                            	"value":"15"
                            }
                            ]
			}
			]
}
```



## Trigger和Action列表

### 网关

**Trigger列表**

| name | Trigger   |
| ---- | --------- |
| 亮度较暗 | low_light |
| 报警   | on_alarm  |



**Action列表**

| name   | Action                  |
| ------ | ----------------------- |
| 开夜灯    | open_corridor_light     |
| 关夜灯    | close_corridor_light    |
| 开/关夜灯  | toggle_corridor_light   |
| 设置夜灯颜色 | set_corridor_light_argb |
| 播放指定铃声 | play_music              |

部分Action需要配置具体参数信息：

- **set_corridor_light_argb**

  | paramId      | value                                   |
  | ------------ | --------------------------------------- |
  | PD.lightARGB | 夜灯ARGB，格式：亮度+RGB，取值范围：0~0x64FFFFFF，转十进制 |

- **play_music**

  | paramId       | value             |
  | ------------- | ----------------- |
  | PD.musicIndex | 播放音乐的编号，0~8，10~13 |
  | PD.vol        | 音量，取值范围0~100      |

  ​

### 空调伴侣（升级版）

**Trigger列表**

| name | Trigger   |
| ---- | --------- |
| 亮度较暗 | low_light |
| 报警   | on_alarm  |



**Action列表**

| name     | Action           |
| -------- | ---------------- |
| 开空调      | open_acpartner   |
| 关空调      | close_acpartner  |
| 开/关空调    | toggle_acpartner |
| 开空调至指定状态 | ctrl_acpartner   |
| 播放指定铃声   | play_music       |

部分Action需要配置具体参数信息：

- **ctrl_acpartner**

  | paramId   | value                                    |
  | --------- | ---------------------------------------- |
  | PD.status | 根据值决定是开关、风速、模式等等。可参考[资源定义](http://docs.opencloud.aqara.cn/development/cloud-development/#_15)。 |

- **play_music**

  | paramId       | value                   |
  | ------------- | ----------------------- |
  | PD.musicIndex | 播放音乐的编号，0~8，10~13，20~33 |
  | PD.vol        | 音量，取值范围0~100            |

  ​

### 墙壁插座（ZigBee版）

**Trigger列表**

| name | Trigger          |
| ---- | ---------------- |
| 打开瞬间 | trigger_turn_on  |
| 关闭瞬间 | trigger_turn_off |
| 开着   | trigger_on       |
| 关着   | trigger_off      |



**Action列表**

| name | Action |
| ---- | ------ |
| 开    | open   |
| 关    | close  |
| 开/关  | toggle |



### 智能插座（ZigBee版）

**Trigger列表**

| name | Trigger          |
| ---- | ---------------- |
| 打开瞬间 | trigger_turn_on  |
| 关闭瞬间 | trigger_turn_off |
| 开着   | trigger_on       |
| 关着   | trigger_off      |



**Action列表**

| name | Action |
| ---- | ------ |
| 开    | open   |
| 关    | close  |
| 开/关  | toggle |



### 墙壁开关（单键版）

**Trigger列表**

| name | Trigger          |
| ---- | ---------------- |
| 打开瞬间 | trigger_turn_on  |
| 关闭瞬间 | trigger_turn_off |
| 开着   | trigger_on       |
| 关着   | trigger_off      |



**Action列表**

| name | Action |
| ---- | ------ |
| 开    | open   |
| 关    | close  |
| 开/关  | toggle |



### 墙壁开关（双键版）

**Trigger列表**

| name   | Trigger                |
| ------ | ---------------------- |
| 左路打开瞬间 | trigger_left_turn_on   |
| 左路关闭瞬间 | trigger_left_turn_off  |
| 左路开着   | trigger_left_on        |
| 左路关着   | trigger_left_off       |
| 右路打开瞬间 | trigger_right_turn_on  |
| 右路关闭瞬间 | trigger_right_turn_off |
| 右路开着   | trigger_right_on       |
| 右路关着   | trigger_right_off      |



**Action列表**

| name  | Action       |
| ----- | ------------ |
| 左键开   | left_open    |
| 左键关   | left_close   |
| 左键开/关 | left_toggle  |
| 右键开   | right_open   |
| 右键关   | right_close  |
| 右键开/关 | right_toggle |



### 智能窗帘

**Action列表**

| name      | Action      |
| --------- | ----------- |
| 打开窗帘      | open        |
| 关闭窗帘      | close       |
| 停止窗帘      | stop        |
| 窗帘打开到指定位置 | open_degree |

部分Action需要配置具体参数信息：

- **open_degree**

  | paramId     | value             |
  | ----------- | ----------------- |
  | PD.position | 窗帘打开百分比，取值范围0~100 |

  ​

### 智能门锁

**Trigger列表**

| name   | Trigger                 |
| ------ | ----------------------- |
| 反锁     | reverse_locked          |
| 任意指纹开锁 | unlock_any_fing         |
| 某人指纹开锁 | unlock_someone_fing     |
| 任意卡片开锁 | unlock_any_card         |
| 某人卡片开锁 | unlock_someone_card     |
| 任意密码开锁 | unlock_any_password     |
| 某人密码开锁 | unlock_someone_password |
| 有人撬锁   | pick_lock               |
| 多次错误开锁 | verified_wrong          |

部分Trigger需要配置具体参数信息：

- **unlock_someone_fing**

  | paramId    | value   |
  | ---------- | ------- |
  | PD.lockUID | 门锁指纹UID |

- **unlock_someone_card**

  | paramId    | value   |
  | ---------- | ------- |
  | PD.lockUID | 门锁卡片UID |

- **unlock_someone_password**

  | paramId    | value   |
  | ---------- | ------- |
  | PD.lockUID | 门锁密码UID |


- **verified_wrong**

  | paramId       | value          |
  | ------------- | -------------- |
  | PD.errorCount | 错误次数，取值范围3~10。 |

  ​

**Action列表**

| name | Action        |
| ---- | ------------- |
| 设置音量 | adjust_volume |

部分Action需要配置具体参数信息：

- **adjust_volume**

  | paramId    | value                       |
  | ---------- | --------------------------- |
  | PD.lockVol | "高音":1，"中音":2，"低音":3，"静音":4 |

  ​

### 空调温控器

**Trigger列表**

| name   | Trigger            |
| ------ | ------------------ |
| 打开瞬间   | trigger_turn_on    |
| 关闭瞬间   | trigger_turn_off   |
| 开着     | trigger_on         |
| 关着     | trigger_off        |
| 处于指定模式 | trigger_in_x_model |

部分Trigger需要配置具体参数信息：

- **trigger_in_x_model**

  | paramId    | value         |
  | ---------- | ------------- |
  | PD.acmodel | "制热":0,"制冷":1 |

**Action列表**

| name      | Action                    |
| --------- | ------------------------- |
| 开空调       | hvac_on                   |
| 关空调       | hvac_off                  |
| 风速设定为自动模式 | set_wind_speed_auto_model |
| 开空调至指定状态  | hvac_ctrl                 |

部分Action需要配置具体参数信息：

- **hvac_ctrl**

  | paramId   | value                                    |
  | --------- | ---------------------------------------- |
  | PD.status | 根据值决定是开关、风速、模式等等。可参考[资源定义](http://docs.opencloud.aqara.cn/development/cloud-development/#_15)。 |



### 双路控制器

**Trigger列表**

| name    | Trigger              |
| ------- | -------------------- |
| 第1路打开瞬间 | trigger_ch1_turn_on  |
| 第1路关闭瞬间 | trigger_ch1_turn_off |
| 第2路打开瞬间 | trigger_ch2_turn_on  |
| 第2路关闭瞬间 | trigger_ch2_turn_off |
| 第1路打开中  | trigger_ch1_on       |
| 第1路关闭中  | trigger_ch1_off      |
| 第2路打开中  | trigger_ch2_on       |
| 第2路关闭中  | trigger_ch2_off      |



**Action列表**

| name   | Action          |
| ------ | --------------- |
| 第1路开   | channel1_on     |
| 第1路关   | channel1_off    |
| 第1路开/关 | channel1_toggle |
| 第2路开   | channel2_on     |
| 第2路关   | channel2_off    |
| 第2路开/关 | channel2_toggle |



### 调光控制器

**Trigger列表**

| name    | Trigger              |
| ------- | -------------------- |
| 第1路打开瞬间 | trigger_ch1_turn_on  |
| 第1路关闭瞬间 | trigger_ch1_turn_off |
| 第2路打开瞬间 | trigger_ch2_turn_on  |
| 第2路关闭瞬间 | trigger_ch2_turn_off |
| 第3路打开瞬间 | trigger_ch3_turn_on  |
| 第3路关闭瞬间 | trigger_ch3_turn_off |
| 第1路开着   | trigger_ch1_on       |
| 第1路关着   | trigger_ch1_off      |
| 第2路开着   | trigger_ch2_on       |
| 第2路关着   | trigger_ch2_off      |
| 第3路开着   | trigger_ch3_on       |
| 第3路关着   | trigger_ch3_off      |



**Action列表**

| name     | Action       |
| -------- | ------------ |
| 打开第1路    | turn_on_ch1  |
| 关闭第1路    | turn_off_ch1 |
| 打开/关闭第1路 | toggle_ch1   |
| 打开第2路    | turn_on_ch2  |
| 关闭第2路    | turn_off_ch2 |
| 打开/关闭第2路 | toggle_ch2   |
| 打开第3路    | turn_on_ch3  |
| 关闭第3路    | turn_off_ch3 |
| 打开/关闭第3路 | toggle_ch3   |



### 门窗传感器

**Trigger列表**

| name     | Trigger     |
| -------- | ----------- |
| 门窗打开瞬间   | open        |
| 门窗关闭瞬间   | close       |
| 门窗开着     | opening     |
| 门窗关着     | closing     |
| 门窗打开一段时间 | open_until  |
| 门窗关闭一段时间 | close_until |

部分Trigger需要配置具体参数信息：

- **open_until**

  | paramId     | value             |
  | ----------- | ----------------- |
  | PD.duration | 时长，取值1~1439，单位分钟。 |


- **close_until**

  | paramId     | value             |
  | ----------- | ----------------- |
  | PD.duration | 时长，取值1~1439，单位分钟。 |



### 人体传感器

**Trigger列表**

| name          | Trigger                    |
| ------------- | -------------------------- |
| 有人移动瞬间        | motion                     |
| 无人移动一段时间      | no_motion_xx               |
| 有人移动且光照度高于指定值 | motion_illumination_higher |
| 有人移动且光照度低于指定值 | motion_illumination_below  |

部分Trigger需要配置具体参数信息：

- **no_motion_xx**

  | paramId     | value             |
  | ----------- | ----------------- |
  | PD.duration | 时长，取值1~1439，单位分钟。 |


- **motion_illumination_higher**

  | paramId         | value               |
  | --------------- | ------------------- |
  | PD.illumination | 光照度，取值1~1000，单位 Lx。 |


- **motion_illumination_below**

  | paramId         | value               |
  | --------------- | ------------------- |
  | PD.illumination | 光照度，取值1~1000，单位 Lx。 |



### 温湿度气压传感器

**Trigger列表**

| name     | Trigger                |
| -------- | ---------------------- |
| 温度上升到    | temp_more_than_instant |
| 温度下降到    | temp_less_than_instant |
| 处于指定温度以上 | temp_more_than         |
| 处于指定温度以下 | temp_less_than         |
| 湿度上升到    | humi_more_than_instant |
| 湿度下降到    | humi_less_than_instant |
| 处于指定湿度以上 | humi_more_than         |
| 处于指定湿度以下 | humi_less_than         |

部分Trigger需要配置具体参数信息：

- **temp_more_than_instant**

  | paramId | value               |
  | ------- | ------------------- |
  | PD.temp | 温度，取值范围-40~125，单位℃。 |


- **temp_less_than_instant**

  | paramId | value               |
  | ------- | ------------------- |
  | PD.temp | 温度，取值范围-40~125，单位℃。 |


- **temp_more_than**

  | paramId | value               |
  | ------- | ------------------- |
  | PD.temp | 温度，取值范围-40~125，单位℃。 |


- **temp_less_than**

  | paramId | value               |
  | ------- | ------------------- |
  | PD.temp | 温度，取值范围-40~125，单位℃。 |


- **humi_more_than_instant**

  | paramId | value          |
  | ------- | -------------- |
  | PD.humi | 湿度，取值范围0~100%。 |


- **humi_less_than_instant**

  | paramId | value          |
  | ------- | -------------- |
  | PD.humi | 湿度，取值范围0~100%。 |


- **humi_more_than**

  | paramId | value          |
  | ------- | -------------- |
  | PD.humi | 湿度，取值范围0~100%。 |


- **humi_less_than**

  | paramId | value          |
  | ------- | -------------- |
  | PD.humi | 湿度，取值范围0~100%。 |



### 水浸传感器

**Trigger列表**

| name | Trigger |
| ---- | ------- |
| 浸水报警 | leak    |
| 浸水解除 | unleak  |



### 天然气报警器

**Trigger列表**

| name     | Trigger |
| -------- | ------- |
| 气体浓度超标报警 | alarm   |



### 烟雾报警器

**Trigger列表**

| name     | Trigger |
| -------- | ------- |
| 烟雾浓度超标报警 | alarm   |



### 无线开关

**Trigger列表**

| name | Trigger      |
| ---- | ------------ |
| 单击   | click        |
| 双击   | double_click |



### 无线开关（升级版）

**Trigger列表**

| name | Trigger          |
| ---- | ---------------- |
| 单击   | click            |
| 双击   | double_click     |
| 长按   | long_click_press |
| 摇一摇  | shake_air        |



### 86无线开关（单键）

设备类型：lumi.sensor_86sw1.aq1和lumi.sensor_86sw1.v1

**Trigger列表**

| name | Trigger |
| ---- | ------- |
| 单击   | click   |



设备类型：lumi.remote.b186acn01

**Trigger列表**

| name | Trigger   |
| ---- | --------- |
| 单击   | click_ch0 |



### 86无线开关（双键）

设备类型：lumi.sensor_86sw2.aq1和lumi.sensor_86sw2.v1

**Trigger列表**

| name   | Trigger          |
| ------ | ---------------- |
| 左键单击   | left_click       |
| 右键单击   | right_click      |
| 左右同时单击 | left_right_click |



设备类型：lumi.remote.b286acn01

**Trigger列表**

| name   | Trigger    |
| ------ | ---------- |
| 左键单击   | click_ch0  |
| 右键单击   | click_ch1  |
| 左右同时单击 | both_click |



### 魔方传感器

**Trigger列表**

| name      | Trigger   |
| --------- | --------- |
| 翻转90°     | flip90    |
| 翻转180°    | flip180   |
| 摇一摇       | shake_air |
| 平面旋转      | rotate    |
| 敲击两下      | tap_twice |
| 轻推        | move      |
| 静止1分钟后被触动 | alert     |



### 动静帖

**Trigger列表**

| name  | Trigger |
| ----- | ------- |
| 感应到跌落 | drop    |
| 感应到倾斜 | tilt    |
| 感应到震动 | vibrate |

