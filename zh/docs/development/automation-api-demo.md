# 场景和自动化接口示例

> 注意：场景和自动化接口的详细参数说明，请参考Trigger和Action列表。

## 场景

### 示例

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



## 自动化

### 示例

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

