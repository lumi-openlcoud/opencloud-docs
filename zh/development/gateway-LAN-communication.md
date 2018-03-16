# 网关局域网通信协议

## 概述

绿米智能网关支持局域网通讯功能，通过开放局域网通信API，允许开发者通过局域网通信对网关下各ZigBee子设备（传感器、控制器等）进行管理。

相比HTTP通讯，局域网通讯的速度更快，控制延迟更低。但是，局域网对接方式的开发成本更高，需要一台支持开发的第三方网关，并且开发者有嵌入式开发经验。

目前主要开放的API功能有：

- 发现与查询设备
- 设备状态上报
- 对设备进行读写操作
- 设备心跳上报




## 修订记录

介绍网关局域网通信协议各版本的主要变更内容。

| 更新时间       | 文档版本   | 更新日志                                     |
| ---------- | ------ | ---------------------------------------- |
| 2017-10-09 | V2.0.1 | 修改：水浸传感器的属性上报                            |
| 2017-09-20 | V2.0.0 | 修改：基本的JSON格式变更；部分设备的model值和属性名称变更；对于属性的取值类型，模拟量统一取值为数值型。 |



## 加密机制

局域网通信采用key加密方式，需在APP上获取随机生成的网关KEY，该KEY使用AES-CBC 128加密，为16个字节长度的字符串。开启局域网通信协议并拥有该网关的KEY后，才能与该网关进行局域网通信。 

> 注意：AES-CBC 128初始向量定义为：unsigned char const AES_KEY_IV[16] = {0x17, 0x99, 0x6d, 0x09, 0x3d, 0x28, 0xdd, 0xb3, 0xba, 0x69, 0x5a, 0x2e, 0x6f, 0x58, 0x56, 0x2e}。



获取网关KEY的具体操作如下：

1、打开Aqara APP，选择需要进行局域网通信的网关设备；

> 注意：当前仅“**升级版空调伴侣**”支持局域网通信协议功能。

2、默认情况下，此页面不显示“局域网协议”，需连续点击"设备类型"10次才可显示。

3、开启“局域网协议”，获取随机KEY，单击“确定”。

![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/development/gateway-LAN-communication/app-1.png)





## 设备发现与查询

### 发现网关设备

设备发现采用不加密方式，使用**组播**（**IP：224.0.0.50，peer_port：4321**），在局域网中发现网关设备。

以组播方式发送“whois”命令：

```
{
   "cmd":"whois"
}
```

所有网关收到“whois”命令都要应答且回复自己的IP信息，以**单播**的形式回复：

```
{
   "cmd":"iam",
   "ip":"192.168.0.42",   //网关IP地址
   "protocal":"UDP",
   "port":"9898",
   "model":"gateway.aq1",  //网关设备类型
   ......
}
```

 



### 查询子设备列表

命令以**单播**方式发送给网关的UDP 9898端口，用来获取网关中有哪些子设备。

以单播方式向网关发送“discovery”命令：

```
{
   "cmd":"discovery"
}
```

网关以单播方式回复，返回子设备的设备id和model值：

```
{
   "cmd":"discovery_rsp",
   "sid":"158d323123c9d9",     //sid为网关did
   "token":"TahkC7dalbIhXG22",    //网关生成的随机字符串
   "dev_list":[{"sid":"158d0000f1a750","model":"plug"},  
               {"sid":"158d00010fd645","model":"sensor_switch.aq2"}]  //sid为子设备did
}
```

> 注意：“token”为网关生成的随机字符串，每10s刷新一次，在未收到设备心跳上报的token前，用户可用此token来生成写设备时的“key”。
>





## 设备状态上报

当设备状态发生变化时，使用“report”命令以**组播**方式发送给（**IP：224.0.0.50，Port：9898**）上报属性状态，如门窗传感器的打开或关闭信息。利用上报的属性状态，用户可以实现智能联动操作，如关闭窗户即开启空调。

例如：

门窗传感器上报窗户的开关状态，格式如下：

```
{
   "cmd":"report",
   "model":"sensor_magnet.aq2",
   "sid":"158d0000123456",
   "params":[{"window_status":"open"}] 
}
```





## 设备心跳上报

**网关心跳**

网关心跳以**组播**方式发送给（**IP：224.0.0.50，Port：9898**）。网关每10秒钟发送一次心跳报文，用来告诉PC网关正常工作。若间隔65s以上未收到心跳包即表示网关处于离线状态。网关设备心跳格式如下：

```
{
    "cmd":"heartbeat",
    "model":"gateway.v3",
    "sid":"f0b429b3c9d965",
    "token":"1234567890abcdef",   //网关生成的随机字符串
    "params":[{"ip":"172.22.4.130"}]  //网关IP地址
 }
```

> 注意：“token”为网关生成的随机字符串，每10s刷新一次，可用此token来生成写设备时的“key”。
>





**子设备心跳**

子设备心跳以**组播**方式发送给（**IP：224.0.0.50，Port：9898**）。子设备通过心跳告诉PC：子设备正常工作（心跳上报频率：一般睡眠设备是每小时一次，插电设备是每10分钟一次）。子设备心跳格式如下：

```
{
   "cmd":"heartbeat",
   "model":"sensor_magnet.aq2",
   "sid":"158d000065a271",
   "params":[{"window_status":"open"}]
}
```

子设备心跳中可能包含子设备的属性状态，如格式中的`"window_status":"open"`。在设置心跳的时候，需看此属性状态的具体使用场景。

例如：开窗关空调场景，可以使用上面的心跳（有可能正常的report报文丢失，心跳报文可以补救）。但关窗开空调场景，就不能使用上面的心跳。因为有可能人离开的时候把空调关了，但心跳报文又让空调打开，很浪费电。

因此，针对心跳报文的使用，用户可根据使用需要自行决定是否用心跳做触发。





## 设备的读写操作

**读设备**

使用“read”命令以**单播**方式发送给网关的UDP 9898端口。用户可以用主动读取各设备的属性状态，网关返回该设备的全部属性信息。

例如：

读取墙壁开关的状态：

```
{
   "cmd":"read",
   "sid":"158d0000123456"   //墙壁开关did
}
```

网关以**单播**方式回复，格式如下：

```
{
   "cmd":"read_rsp",
   "model":"ctrl_neutral2",
   "sid":"158d0000123456",
   "params":[{"channel_0":"on"},{"channel_1":"off"}]  
}
```





**写设备**

使用“write”命令以**单播**方式发送给网关的UDP 9898端口。当用户需要控制各设备时，可使用“write”命令。

例如：

将墙壁开关（单火单键）的状态改为关闭：

```
{
    "cmd":"write",
    "model":"ctrl_neutral1",
    "sid":"158d0000123456",
    "key":"3EB43E37C20AFF4C5872CC0D04D81314",
    "params":[{"channel_0":"off"}]
 }
```



网关以**单播**方式回复格式：

```
{
   "cmd":"write_rsp",
   "model":"ctrl_neutral1",
   "sid":"158d0000123456",
   "params":[{"channel_0":"on"}]  
}
```

该“write_rsp”只代表网关收到了write命令，params里的属性状态为当前的设备最新状态，不是write之后的最终设备状态。最终的设备状态靠report报文进行上报。



> 注意：“key”为32个字节长度的字符串。当网关启用了加密模式时，会对该key进行解密并校验，以验证写命令的合法性。该“key”的生成规则是：用户收到心跳“heartbeat”里的16个字节的“token”字符串之后，使用网关的KEY（在APP里获取的随机KEY）对该字符串进行AES-CBC 128加密，生成16个字节的密文后，再转换为32个字节的ASCII码字符串。
>
> 例如：米家智能家庭APP中16个字符长度的随机KEY为“0987654321qwerty“，”token”为”1234567890abcdef”，加密后的密文是：0x3E,0xB4,0x3E,0x37,0xC2,0x0A,0xFF,0x4C,0x58,0x72,0xCC,0x0D,0x04,0xD8,0x13,0x14。那么，”key”为：”3EB43E37C20AFF4C5872CC0D04D81314”。





## 设备上报和控制报文格式

JSON报文的基本格式：

```
{
   "cmd":"write",     //命令类型，支持write/read/write_rsp/read_rsp/report/heartbeat
   "model":"ctrl_neutral2",   //设备类型
   "sid":"158d0000123456",    //设备的did
   "params":[{"channel_0":"on"},{"channel_1":"off"}]  //params里可以包含同一个设备的多个属性 
}
```





## 设备属性列表

介绍Aqara产品的设备类型、属性和使用示例。



### 空调伴侣

（设备类型model：acpartner.v3）

| 属性              | 说明                                       |
| --------------- | ---------------------------------------- |
| illumination    | 空调伴侣的光照度，取值范围一般为0~1300；支持report/read。    |
| proto_version   | 采用的通信协议版本号，如”2.0.1”。                     |
| mid             | 表示music id，即音乐铃声的id。支持write。取值有：0~8，10~13，20~29（上述为系统自带铃声），10000（表示停止播放铃声），>10001(表示用户自定义的铃声)。 |
| join_permission | 取值“yes”/”no”，表示是否允许添加子设备。                |
| remove_device   | 取值为子设备的did（did的16进制形式的字符串），用于删除某个子设备。    |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"acpartner.v3",
   "sid":"640980123456",
   "params":["illumination:500,"proto_version":"2.0.0"}]
   //光照度为500，通信协议版本为2.0.0。
}
```

 

控制：

播放mid为10005的自定义铃声：

```
{
   "cmd":"write",
   "model":"acpartner.v3",
   "sid":"640980123456",
   "params":[{"mid":10005}]
}
```

  

停止播放铃声：

```
{
   "cmd":"write",
   "model":"acpartner.v3",
   "sid":"640980123456",
   "params":[{"mid":10000}]
}
```

  

允许添加子设备：

```
{
   "cmd":"write",
   "model":"acpartner.v3",
   "sid":"640980123456",
   "params":[{"join_permission":"yes"}]
}
```

> 注意：添加子设备须在30s内进行操作：长按子设备重置键3~5秒直到蓝色指示灯连续闪烁后松开，网关提示设备添加成功，即入网成功。不同子设备长按重置键，指示灯可能不一样，请根据实际情况操作。



删除空调伴侣下的某个子设备：

```
{
   "cmd":"write",
   "model":"acpartner.v3",
   "sid":"640980123456",
   "params":[{"remove_device":"158d0000f12345"}]
}
```





### 智能插座

（设备类型model：plug）

| 属性              | 说明                            |
| --------------- | ----------------------------- |
| channel_0       | on/off（开/关）                   |
| load_power      | 负载功率，单位是瓦（W）                  |
| energy_consumed | 从产品开始被使用以来累计的负载消耗电量，单位是瓦时（Wh） |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"plug",
   "sid":"158d0000123123",
   "params":[{"channel_0":"on"}] //智能插座状态为“开”
}
```

 

控制：

```
{
   "cmd":"write",
   "model":"plug",
   "sid":"158d0000123123",
   "params":[{"channel_0":"off"}]  //将智能插座状态改为“关”
}
```



心跳上报（~10分钟每次）：

```
{
   "cmd":"heartbeat",
   "model":"plug",
   "sid":"158d0000123123",
   "params":[{"load_power":9.57},{”energy_consumed":57}] 
   //负载功率为9.57W，负载消耗电量为57Wh。
}
```





### 墙壁插座

（设备类型model：ctrl_86plug）

| 属性              | 说明                            |
| --------------- | ----------------------------- |
| channel_0       | on/off/unknown（开/关/未知）        |
| load_power      | 负载功率，单位是瓦（W）                  |
| energy_consumed | 从产品开始被使用以来累计的负载消耗电量，单位是瓦时（Wh） |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"ctrl_86plug",
   "sid":"158d0001123123",
   "params":[{"channel_0":"on"}]  //墙壁插座状态为“开”
}
```

 

控制：

```
{
   "cmd":"write",
   "model":"ctrl_86plug",
   "sid":"158d0001123123",
   "params":[{“channel_0”:”off”}]  //将墙壁插座状态改为“关”
}
```



心跳上报（~10分钟每次）：

```
{
   "cmd":"heartbeat",
   "model":"ctrl_86plug",
   "sid":"158d0001123123",
   "params":[{"load_power":9.57},{"energy_consumed":57}]
   //负载功率为9.57W，负载消耗电量为57Wh。
}
```





### 墙壁开关（零火单键）

（设备类型model：ctrl_ln1）

| 属性        | 说明                     |
| --------- | ---------------------- |
| channel_0 | on/off/unknown（开/关/未知） |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"ctrl_ln1",
   "sid":"158d0001112313",
   "params":[{"channel_0":"on"}]  //墙壁开关状态为“开”
}
```

 

控制：

```
{
   "cmd":"write",
   "model":"ctrl_ln1",
   "sid":"158d0001112313",
   "params":[{"channel_0":"off"}]   //将墙壁开关状态改为“关”
}
```





### 墙壁开关（零火双键）

（设备类型model：ctrl_ln2）

| 属性        | 说明                     |
| --------- | ---------------------- |
| channel_0 | on/off/unknown（开/关/未知） |
| channel_1 | on/off/unknown（开/关/未知） |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"ctrl_ln2",
   "sid":"158d0001112312",
   "params":[{"channel_1":"on"}]  //墙壁开关2状态为“开”
}
```

 

控制：

```
{
   "cmd":"write",
   "model":"ctrl_ln2",
   "sid":"158d0001112312",
   "params":[{"channel_1":"off"}]  //将墙壁开关2状态改为“关”
}
```





### 墙壁开关（单火单键）

（设备类型model：ctrl_neutral1）

| 属性        | 说明          |
| --------- | ----------- |
| channel_0 | on/off（开/关） |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"ctrl_neutral1",
   "sid":"158d0001112311",
   "params":[{"channel_0":"on"}]  //墙壁开关状态为“开”
}
```

 

控制：

```
{
   "cmd":"write",
   "model":"ctrl_neutral1",
   "sid":"158d0001112311",
   "params":[{" channel_0":"off"}]  //将墙壁开关状态改为“关”
}
```





### 墙壁开关（单火双键）

（设备类型model：ctrl_neutral2）

| 属性        | 说明          |
| --------- | ----------- |
| channel_0 | on/off（开/关） |
| channel_1 | on/off（开/关） |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"ctrl_neutral2",
   "sid":"158d0001112316",
   "params":[{"channel_0":"on"}]  //墙壁开关1状态为“开”
}
{
   "cmd":"report",
   "model":"ctrl_neutral2",
   "sid":"158d0001112316",
   "params":[{"channel_1":"on"}]  //墙壁开关2状态为“开”
}
```

 

控制： 

```
{
   "cmd":"write",
   "model":"ctrl_neutral2",
   "sid":"158d0001112316",
   "params":[{"channel_0":"on"}]  //将墙壁开关1改为“开”
}
{
   "cmd":"write",
   "model":"ctrl_neutral2",
   "sid":"158d0001112316",
   "params":[{"channel_1":"off"}]  //将墙壁开关2改为“关”
}
```





### 窗帘电机

（设备类型model：curtain）

| 属性             | 说明                                       |
| -------------- | ---------------------------------------- |
| curtain_status | open/close/stop/auto （开窗帘/关窗帘/停止工作/自动工作）。支持“write”（write之后设备会上报curtain_level），不支持“report”。 |
| curtain_level  | 取值：0-100表示打开窗帘的百分比；-1或255表示位置未知。支持“write”和“report”。 |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"curtain",
   "sid":"158d0001012345",
   "params":[{"curtain_level":50}]  //窗帘打开50%
}
```

 

控制：

```
{
   "cmd":"write",
   "model":"curtain",
   "sid":"158d0001012345",
   "params":[{"curtain_status":"open"}]  //开窗帘
}
{
   "cmd":"write",
   "model":"curtain",
   "sid":"158d0001012345",
   "params":[{"curtain_level":25}]  //窗帘打开25%
}
```

上报窗帘打开状态：

```
{
   "cmd":"report",
   "model":"curtain",
   "sid":"158d0001012345",
   "params":[{"curtain_level":25}]  //上报窗帘已打开25%
}
```





### 门窗传感器

（设备类型model：sensor_magnet.aq2）

门窗传感器感知窗户或门的打开/关闭状态，每动作一次发送一次report。

| 属性              | 说明                                       |
| --------------- | ---------------------------------------- |
| window_status   | open/close/unknown（开/关/未知）               |
| battery_voltage | 纽扣式电池电压值，单位mv，范围0~3300mv。一般情况下，小于2800mv时表示低电量。 |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"sensor_magnet.aq2",
   "sid":"158d0001123456",
   "params":[{"window_status":"open"}]  //窗户被打开
}
```

 

心跳上报（~60分钟每次）：

```
{
   "cmd":"report",
   "model":"sensor_magnet.aq2",
   "sid":"158d0001123456",
   "params":[{"battery_voltage":3000}]
}
```





### 人体传感器

（设备类型model：sensor_motion.aq2）

人体传感器探测到有人移动时会立即report信息，同时上报光照度值”lux”和“illumination”。在一直有人移动的情况下，为了省电，人体传感器最快一分钟发送一次report。人体传感器在每个心跳时，也会上报当前的光照度值”lux”。其他情况下，人体传感器不上报光照度值。

| 属性              | 说明                                       |
| --------------- | ---------------------------------------- |
| battery_voltage | 纽扣式电池电压值，单位mv，范围0~3300mv。一般情况下，小于2800mv时表示低电量。 |
| motion_status   | 取值：motion表示探测到有人；unknown表示未知             |
| lux             | 光照度值，取值范围 0 ~ 1200。在检测到有人移动时采集光照度并上报；或者在传感器心跳时上报。 |
| illumination    | 光照度值，取值范围 0 ~ 1200，只在检测到有人移动时采集光照度并上报。   |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"sensor_motion.aq2",
   "sid":"158d0000f12222",
   "params":[{"motion_status":"motion"}]  //检测到有人移动
}
```

 同时上报光照度值：

```
{
   "cmd":"report",
   "model":"sensor_motion.aq2",
   "sid":"158d0000f12222",
   "params":[{"lux":100},{"illumination":100}]  
}
```

 

心跳上报（~60分钟每次）：

```
{
   "cmd":"report",
   "model":"sensor_motion.aq2",
   "sid":"158d0000f12222",
   "params":[{"battery_voltage":3000},{"lux":50}]
}
```





### 温湿度传感器

（设备类型model：weather）

温湿度传感器检测到温度变化达到0.5度或者湿度变化达到6%时，发送一次report上报，温度或湿度上报时，同时会上报气压值。温湿度传感器在每次心跳时，也会上报当前温度、湿度和气压值。

| 属性              | 说明                                       |
| --------------- | ---------------------------------------- |
| temperature     | 温度，数值型，默认的invalid值为10000。                |
| humidity        | 湿度，数值型，默认的invalid值为0。                    |
| pressure        | 大气气压值，数值型，单位帕Pa，取值范围30000~110000。默认的invalid值为0。 |
| battery_voltage | 纽扣式电池电压值，单位mv，范围0~3300mv。一般情况下，小于2800mv时表示低电量。 |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"weather",
   "sid":"158d0000f12346",
   "params":[{"temperature":2333}]  //温度是23.33度
}
```

 

心跳上报（~60分钟每次）：

```
{
   "cmd":"heartbeat",
   "model":"weather",
   "sid":"112316",
   "params":[{"battery_voltage":3000}, {"temperature":2333},{"humidity":6678},{"pressure":99900}]
   //温度为23.33度，湿度为66.78%，大气气压为99.9KPa。
}
```





### 水浸传感器

（设备类型model：sensor_wleak.aq1）

| 属性              | 说明                                       |
| --------------- | ---------------------------------------- |
| wleak_status    | 取值：normal表示没有报警或者已解除报警；leak表示发生浸水报警。     |
| battery_voltage | 纽扣式电池电压值，单位mv，范围0~3300mv。一般情况下，小于2800mv时表示低电量。 |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"sensor_wleak.aq1",
   "sid":"158d0001234567",
   "params":[{"wleak_status":"leak"}]  //检测到发生浸水报警
}
```

心跳上报（~60分钟每次）：

```
{
   "cmd":"heartbeat",
   "model":"sensor_wleak.aq1",
   "sid":"158d0001234567",
   "params":[{"battery_voltage":3000}]
}
```





### 无线开关

（设备类型model：sensor_switch.aq3）

无线开关每按一次按键上报一个报文，400ms内按两次上报的报文是双击。

| 属性              | 说明                                       |
| --------------- | ---------------------------------------- |
| button_0        | click/double_click/shake（单击/双击/摇一摇）      |
| battery_voltage | 纽扣式电池电压值，单位mv，范围0~3300mv。一般情况下，小于2800mv时表示低电量。 |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"sensor_switch.aq3",
   "sid":"158d0000123456",
   "params":[{"button_0":"click"}]  //单击无线开关
}
```

心跳上报（~60分钟每次）：

```
{
   "cmd":"heartbeat",
   "model":"sensor_switch.aq3",
   "sid":"158d0000123456",
   "params":[{"battery_voltage":3000}]
}
```





### 86无线开关单键

（设备类型model：sensor_86sw1.aq1）

| 属性              | 说明                                       |
| --------------- | ---------------------------------------- |
| button_0        | click/double_click（单击/双击）                |
| battery_voltage | 纽扣式电池电压值，单位mv，范围0~3300mv。一般情况下，小于2800mv时表示低电量。 |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"sensor_86sw1.aq1",
   "sid":"158d0000123456",
   "params":[{"button_0":"double_click"}]
}
```





### 86无线开关双键

（设备类型model：sensor_86sw2.aq1）

| 属性           | 说明                             |
| ------------ | ------------------------------ |
| button_0     | click（左键单击）；double_click（左键双击） |
| button_1     | click（右键单击）；double_click（右键双击） |
| dual_channel | both_click（左右键同时按）             |

例如：

属性上报：

```
{
   "cmd":"report",
   "model":"sensor_86sw2.aq1",
   "sid":"158d0000123456",
   "params":[{"button_1":"double_click"}]
}
```

