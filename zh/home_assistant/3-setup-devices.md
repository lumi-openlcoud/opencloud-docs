# 添加设备

只有网关设备需要在configuration.yaml文件中进行配置，子设备通过APP关联网关后，Home Assistant会自动识别。

> 注意：空调伴侣目前只能利用[插件](https://github.com/mac-zhou/homeassistant-mi-acpartner)在Home Assistant中实现空调控制功能，无法用作网关功能。因此，空调伴侣下添加的子设备也无法被识别。

## 配置网关设备

**步骤1：**在APP中添加网关设备。

**步骤2：**获取网关key。

1、打开APP，选择需要进行局域网通信的网关设备；

2、单击右上角的“...”，选择关于，默认情况下，此页面不显示“局域网协议”，需连续点击10次才可显示。

3、开启“局域网协议”，获取随机key，单击“确定”。

**步骤3：**在configuration.yaml文件中手动配置网关信息。

单个网关：

```
#单个网关，可不填mac地址
#discovery_retry为连接失败重试的次数
xiaomi_aqara:
  discovery_retry: 5
  gateways:
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
```

多个网关：

```
#多个网关情况下，必须填mac地址
xiaomi_aqara:
  gateways:
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
```

查询指定IP的网关：

```
#必须填mac地址
#interface为所使用的网络接口，默认值是全部
xiaomi_aqara:
  interface: '192.168.0.1'
  gateways:
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
```

**步骤4：**在Web页面的“配置”-“通用”-“配置检查”中，单击“检查配置”验证配置文件是否有效。

**步骤5：**在“配置”-“通用”-“服务管理”中重启服务即可看到添加的设备信息。



## 配置子设备

在APP中添加子设备，添加成功后，重启Home Assistant即可查看到关联的子设备。

当前支持的子设备列表如下：

- 墙壁插座
- 墙壁开关（单火、单&双键）
- 墙壁开关（零火、单&双键）
- 智能窗帘电机
- 智能门锁
- 人体传感器
- 温湿度传感器
- 门窗传感器
- 水浸传感器
- 无线开关
- 无线开关（贴墙式）


