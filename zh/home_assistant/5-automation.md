# 自动化

自动化即当事件被触发，且满足指定条件时，自动执行预先设定的动作。它由触发器（Trigger）、条件（Condition）和动作（Action）组成，可通过以下两种方式来实现：

- 在configuration.yaml文件中配置自动化信息。
- 在Home Assistant的Web页面，“配置”-“自动化”，单击页面有下角的橙色“+”即可创建自动化。



## 示例

详细了解各模块前，可先查看两种方法的区别。示例：双击无线开关，网关发出门铃声。

- 通过configuration.yaml文件配置自动化。

```
automation:
- alias: play a doolbell
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.switch_158d0001xxxxxx
      click_type: double
  action:
    service: xiaomi_aqara.play_ringtone
    data:
      gw_mac: 640980XXXXXX
      ringtone_id: 10
      ringtone_vol: 8
```

- 在Home Assistant的Web页面配置自动化。

![](http://cdn.cnbj2.fds.api.mi-img.com/cdn/aiot/doc-images/zh/home-assistant/automation-0.png)



## 触发器（Trigger）

触发器是自动化中不可缺少的部分，包括Event，State，Time，Numeric_state等不同类型，由“platform”来标识。

- **Event**

  当事件执行时触发，可以只设置事件名称或指定事件数据来触发。

  ```
  - automation:
      trigger:
        platform: event
        event_type: MY_CUSTOM_EVENT
        event_data:     #optional
          mood: happy
  ```

  > 说明：系统自带事件列表可参考xxxxxxxxxxx。


- **State**

  当指定实体的状态发生变化时触发。若只告知了entity_id，那么该实体的任何状态或属性发生变化都会触发。

  如下示例，若只有entity_id，没有click_type，那么不管单击双击都会触发该自动化。

  ```
  - alias: play a doolbell
    trigger:
      platform: event
      event_type: click
      event_data:
        entity_id: binary_sensor.switch_xxxxxxxx
        click_type: double
  ```


- **Time**

  时间有很多种触发方法。你可以用`at`来设置每天某一个时间点时触发，也可以用`/`来设置当前时间能被该数字整除时触发等。

  ```
  - automation:    #匹配每小时过5分钟时触发，即8:05，9:05等都会触发
      trigger:
        platform: time
        minutes: 5
        seconds: 00    
        //注意：若没有设置seconds，仅匹配minutes，那么会在5分时触发60次

  - automation 2:  #每天下午15:32时触发
      trigger:
        platform: time
        at: '15:32:00'

  - automation 3:   #当前时间能被5整除时触发，相当于每隔五分钟触发一次
      trigger:
        platform: time
        minutes: '/5'
        seconds: 00
  ```

- **Numeric_state**

  当实体状态的某个数值满足设定的门限阈值时触发。

  ```
  - automation:   #当温度超过30度时触发
      trigger:
        platform: numeric_state
        entity_id: sensor.temperature
        value_template: '{{ state.attributes.temperature }}'
        above: 30
  ```

- **Home Assistant**

  当Home Assistant开启或关闭时触发。

  ```
  - automation:    
      trigger:
        platform: homeassistant
        event: start
  ```

- **Sun**

  当太阳升起或降落时触发。可以是太阳升起或降落的前一段时间或者后一段时间，也可以根据水平线距离来判断，如离水平线还有4米的时候触发，或者已越过水平线4米的时候触发。

  ```
  - automation:     #太阳降落前45分钟触发
      trigger:
        platform: sun
        event: sunset
        offset: '-00:45:00'
        
  - automation 2:     #当天完全黑的时候开灯，即太阳降落到水平线以下4米时
      alias: "Lighting on when dark outside"
      trigger:
        platform: numeric_state
        entity_id: sun.sun
        value_template: "{{ state.attributes.elevation }}"
        below: -4.0
      action:
        service: light.turn_on
        entity_id: light.gateway_light_xxxxxxxxxxxx
  ```


- **Template**

  当实体状态发生变化引起模板输出值为“true”时触发。

  ```
  - automation:
      trigger:
        platform: template
        value_template: "{% if is_state('sensor.motion_sensor_xxxxxxxxxxx', 'motion') %}true{% endif %}"
  ```

- **多个触发源**

  当自动化规则中有多个触发源时，任何一个触发满足时，则触发自动化执行动作。

  ```
  - automation:
      trigger:
        - platform: time
          minutes: 5
          seconds: 00
        - platform: event
          event_type: click
          event_data:
            entity_id: binary_sensor.switch_xxxxxxxxxxx
            click_type: double
  ```

  ​

## 条件（Condition）

条件是自动化中非必须的一部分，它可以用在一个脚本或自动化中来阻止触发执行进一步的动作。条件与触发器看起来很像，但其实非常不同。触发器会看系统中发生了哪些事件，比方说开关正在被打开；而条件只关注系统的当前状态，比方说开关当前状态是开还是关。条件与触发器一样有多种类型，用“condition”来标识。

- **多个条件**

  用“AND”和“OR”来对多个条件进行组合，可以单独使用，也可混合使用。

  ```
  condition:    
    condition: and
    conditions:
      - condition: state
        entity_id: 'sensor.montion_sensor_xxxxxxxx'
        state: 'montion'
      - condition: numeric_state
        entity_id: 'sensor.temperature'
        below: '20'

  condition 2:   
    condition: and
    conditions:
      - condition: state
        entity_id: 'sensor.montion_sensor_xxxxxxxx'
        state: 'montion'
      - condition: or
        conditions:
          - condition: state
            entity_id: sensor.temperature
            above: '40'
          - condition: numeric_state
            entity_id: 'sensor.temperature'
            below: '20'
  ```


- **State**

  判断实体是否是某个指定的状态。

  ```
  condition:
    condition: state
    entity_id: sensor.montion_sensor_xxxxxxxx
    state: montion
  ```


- **Time**

  时间条件可以是在某个指定时间前、某个指定时间后，或者判断是否是某个星期的某一时刻。

  ```
  condition:   #每周一、周三的15点到凌晨2点
    condition: time
    after: '15:00:00'
    before: '02:00:00'
    weekday:
      - mon
      - wed
  ```

- **Numeric_state**

  判断实体的数字类型状态是否符合条件（大于某值或小于某值）。判断的对象也可以是模板值（value_template）：对状态值进行调整计算后再进行判断，或者按照实体的数字类型属性而非状态来判断条件是否满足。

  ```
  condition:    #判断实体sensor.temperature的状态值加上2之后是否在17与25之间
    condition: numeric_state
    entity_id: sensor.temperature
    above: 17
    below: 25
    value_template: {{ float(state.state) + 2 }}
  ```


- **Sun**

  判断触发后太阳是否已经升级或降落。`before`和`after`只能用来设置为`sunset`和`sunrise`。还可以为太阳升起或降落设置偏移值，例如`before_offset`和`after_offset`。

  ```
  condition:   #太阳下山前一小时
    condition: sun
    after: sunset
    after_offset: "-1:00:00"
  ```


- **Template**

  判断模板输出值是否为true。

  ```
  condition:
    condition: template
    value_template: '{{ states.device_tracker.iphone.attributes.battery > 50 }}'
  ```



## 动作（Action）

动作是自动化中不可缺少的一部分。当“触发器”被触发，且“条件”满足的情况下，则执行动作。动作就像脚本一样，可以调用一个服务，也可以触发一个事件。调用服务时，你可以设置指定的entity_id，还可以设置一些可选的服务参数，如示例中的brightness。条件也可以是动作的一部分，你可以在一个动作中调用多个服务和条件，它会按照顺序执行。如果其中有一个条件不被满足，动作将停止，在这个条件之后的动作将不被执行。

```
- alias: play a doolbell
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.switch_xxxxxxxx
      click_type: double
  action:
    service: light.turn_on
    entity_id: light.gateway_light_xxxxx
    data:
      brightness: 100
```


