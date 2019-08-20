# 场景和自动化配置说明

Aqara设备支持两种配置方式：自动化和场景。

- 自动化：用户可自定义设置触发条件（Trigger）和执行动作（Actions），当满足触发条件时，自动执行设定的动作。
- 场景：用户可自定义设置场景中需要执行的动作（Actions），当人为触发该场景时，按顺序执行设定的动作。

## 接口调用说明

### 查询设备支持的触发器和动作

调用以下接口确认设备支持的trigger和action信息，详细调用参数请参考“控制台”-“API参考”：

OAuth授权接入v1.0接口：

查询指定对象类型有哪些Trigger：/open/ifttt/trigger/query

查询指定的对象类型下有哪些Action：/open/ifttt/action/query

设备接入v2.0接口：

查询指定对象类型下Trigger(IF)：/open/ifttt/trigger/definition/query

查询指定对象类型下Action(Then)：/open/ifttt/action/definition/query

### 创建自动化和场景

根据设备的trigger和action定义信息，调用以下接口创建自动化和场景，详细调用参数请参考“控制台”-“API参考”：

OAuth授权接入v1.0接口：

创建自动化：/open/ifttt/add

创建场景：/open/scene/add

设备接入v2.0接口：

创建自动化：/open/ifttt/create

创建场景：/open/ifttt/scene/create

## 示例

以下示例以“v1.0接口”参数为例说明。

### 场景

例如：设置夜灯颜色。

```
{
	"name": "测试",
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

### 自动化

例如：当有人经过时，开启夜灯。

```
{
	"name" : "这是一条联动",
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

若Trigger和Action需要配置详细参数信息，则需设置“paramId”和“value”。

例如：当温度上升到22度时，开启夜灯。

```
{
	"name" : "这是一条联动",
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

配置单个参数信息：

当有人经过的时候，开启夜灯，且设置夜灯颜色为红色。

```
{
	"name" : "这是一条联动",
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

配置多个参数信息：

当单击无线开关时，播放指定铃声。

```
{
	"name" : "这是一条联动",
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
