# 开发者工具


## 服务（Services）

Web页面开发者工具中的状态（Services）可查询当前实体的所有功能，例如xiaomi_aqara.play_ringtone即网关铃声功能；light.turn_on即开灯功能。

- 服务：可查看当前实体支持的所有服务。
- 服务数据：设置服务参数，JSON格式。

选择服务后，在页面下方会显示服务的参数信息和举例，可根据实际情况设置。

![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/home-assistant/services.png)



## 状态（States）

Web页面开发者工具中的状态（States）显示了当前所有实体可用的状态，一个实体可以是任何东西，如一盏灯、一个开关等。

一个状态包含如下部分：

| 名称         | 描述                      | 例如            |
| ---------- | ----------------------- | ------------- |
| Entity ID  | 实体的唯一标识ID               | light.turn_on |
| State      | 设备当前的状态                 | on            |
| Attributes | 与设备或当前状态相关的额外数据（JSON格式） | brightness    |

在States页面也可重写已存在的状态，如选择设备，输入State和Attributes后，单击“SET STATE”即可。在此处设定的设备状态仅在Home Assistant生效，并非对真实设备发生操作。例如，修改了灯的开关状态，并不会引起灯实际的开关，但如果其他程序从系统中读取数据，将会读取修改后的数据。

![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/home-assistant/state-0.png)



## 事件（Events）

事件是Home Assistant的核心机制。它的使用是非常灵活的。事件类型只要是字符串，没有其他限制。每个事件能包含附加的数据，只要设置为JSON格式，它可以是数字、字符串、字典和列表。

Home Assistant预定义了一些事件，列表如下：

- 事件`HOMEASSISTANT_START`

  当配置的所有组件被初始化时触发。

- 事件`HOMEASSISTANT_STOP`

  当Home Assistant关闭时触发。当关闭时，任何打开的连接都将被关闭，释放资源。

- 事件`STATE_CHANGED`

  当实体的状态发生改变时触发，包含`old_state`和`new_state`。

  | 参数        | 说明                           |
  | --------- | ---------------------------- |
  | entity_id | 状态发生变化的实体ID。                 |
  | old_state | 在实体状态发生变化前的状态。如果是新的实体，则忽略此项。 |
  | new_state | 实体的新状态。如果实体已被移除，则忽略此项。       |

- 事件`TIME_CHANGED`

  被计时器每秒触发，包含当前时间。

  | 参数   | 说明       |
  | ---- | -------- |
  | now  | 当前的UTC时间 |

- 事件`SERVICE_REGISTERED`

  当一个新的服务在Home Assistant中注册时触发。

  | 参数      | 说明                 |
  | ------- | ------------------ |
  | domain  | 服务的域名。例如：light。    |
  | service | 被调用的服务。例如：turn_on。 |

- 事件`CALL_SERVICE`

  当服务被调用时触发。

  | 参数              | 说明                                |
  | --------------- | --------------------------------- |
  | domain          | 服务的域名。例如：light。                   |
  | service         | 被调用的服务。例如：turn_on。                |
  | service_data    | 服务调用的参数。例如： {'brightness': 120 }。 |
  | service_call_id | 服务调用的唯一调用id。例如：23123-4。           |

- 事件`SERVICE_EXECUTED`

  当调用的服务被执行时触发。

  | 参数              | 说明                      |
  | --------------- | ----------------------- |
  | service_call_id | 服务调用的唯一调用id。例如：23123-4。 |

- 事件`PLATFORM_DISCOVERED`

  当一个新平台被discovery组件发现时触发。

  | 参数         | 说明                                       |
  | ---------- | ---------------------------------------- |
  | service    | 被发现的平台。例如：zwave。                         |
  | discovered | 以字典结构包含发现的信息。例如： { "host":"192.168.1.10", "port": 8889}。 |

- 事件`COMPONENT_LOADED`

  当一个新的组件被加载和初始化时触发。

  | 参数        | 说明                  |
  | --------- | ------------------- |
  | component | 被初始化的组件域名。例如：light。 |


