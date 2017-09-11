---
layout: page
title: "Conditions"
description: "Documentation about all available conditions."
date: 2016-04-24 08:30 +0100
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /getting-started/scripts-conditions/
---

条件可以被添加至脚本或自动化设置中以阻止指令继续执行。条件会监视当下的系统状态，例如可以设定条件以反馈开关处于开启或关闭状态。
### {% linkable_title AND condition %}

以下展示如何用一个条件语句同时判断多个条件内容，当所有子条件满足时，该条件触发。

```yaml
condition:
  condition: and
  conditions:
    - condition: state
      entity_id: 'device_tracker.paulus'
      state: 'home'
    - condition: numeric_state
      entity_id: 'sensor.temperature'
      below: '20'
```

### {% linkable_title OR condition %}

以下展示如何用一个条件语句同时判断多个条件内容，当任一子条件满足时，该条件触发。

```yaml
condition:
  condition: or
  conditions:
    - condition: state
      entity_id: 'device_tracker.paulus'
      state: 'home'
    - condition: numeric_state
      entity_id: 'sensor.temperature'
      below: '20'
```

### {% linkable_title MIXED  AND and OR conditions %}

以下展示如何用一个条件语句同时判断多个条件内容，该设置重点展示如何在 AND 条件中嵌套 OR 判断子条件。
（译者注：下列设置表示当 paulus 在家并且温度低于20度，或者paulus 在家并且下雨时，条件触发。）

```yaml
condition:
  condition: and
  conditions:
    - condition: state
      entity_id: 'device_tracker.paulus'
      state: 'home'
    - condition: or
      conditions:
        - condition: state
          entity_id: sensor.weather_precip
          state: 'rain'
        - condition: numeric_state
          entity_id: 'sensor.temperature'
          below: '20'
```

### {% linkable_title Numeric state condition %}

数字条件（Numeric state condition）基于数值是否满足一定门槛而触发。 

如果以上`below` 和以下 `above` 同时出现在语句中，则 2 个条件都需要满足。

你可以使用值模板 `value_template` 将数据转换为数字类型，以实现条件触发：

```yaml
condition:
  condition: numeric_state
  entity_id: sensor.temperature
  above: 17
  below: 25
  # 如果你的传感器的值需要转换
  value_template: {% raw %}{{ float(state.state) + 2 }}{% endraw %}
```

（译者注：计算机中的数据类型有很多，最常见的有数字和字符串。其中数字还分浮点型（float）数字、整数型数字（int）等，上述配置中 HA 只可以在数字型数据间比较大小，因此需要将字符串（文本）数据转换为数字。）

### {% linkable_title State condition %}

当组件某个值符合设定时触发条件：

```yaml
condition:
  condition: state
  entity_id: device_tracker.paulus
  state: not_home
  # optional: trigger only if state was this for last X time.
  for:
    hours: 1
    minutes: 10
    seconds: 5
```

### {% linkable_title Sun condition %}

太阳组件 `sun` 可以判定太阳日出日落的状态，可以利用其触发特定场景。`before` 和 `after` 属性仅限 `sunset` 和 `sunrise`使用。 同样，另外还有专属的抵消值属性 (`before_offset`, `after_offset`)。类似的还有[sun trigger][sun_trigger]。

[sun_trigger]: /getting-started/automation-trigger/#sun-trigger

```yaml
condition:
  condition: sun
  after: sunset
  # 可选抵消值
  after_offset: "-1:00:00"
```

### {% linkable_title Template condition %}

模板条件将在值 [given template][template] 为真时触发。前提是该条件的输出值为布尔值或者被设定为真值 `true`.

```yaml
condition:
  condition: template
  value_template: '{% raw %}{{ states.device_tracker.iphone.attributes.battery > 50 }}{% endraw %}'
```
（译者注：上述设置表示当 iPhone 的电量大于50时，条件触发。）

在自动化配置中，模板式条件有权限监测 `trigger` 变量，详见 [此处][automation-templating].

[template]: /topics/templating/
[automation-templating]: /getting-started/automation-templating/

### {% linkable_title Time condition %}

时间条件可设定在某个特定时间点触发：

```yaml
condition:
  condition: time
  # 至少需要设置下列其中一项属性
  after: '15:00:00'
  before: '02:00:00'
  weekday:
    - mon
    - wed
    - fri
```

`weekday` 的有效值有 `mon`, `tue`, `wed`, `thu`, `fri`, `sat`, `sun`。
时间节点可以跨越零点，比如可以从下午3点到隔日2点，系统会自动识别。
### {% linkable_title Zone condition %}

地点条件通过判定组件是否在特定地点而触发。开启地点自动化配置前，你必须已设定好支持 GPS 定位的设备追踪平台 (device tracker）。目前仅有[OwnTracks 平台](/components/device_tracker.owntracks/) 和 [iCloud 平台](/components/device_tracker.icloud/) 支持这项功能。

```yaml
condition:
  condition: zone
  entity_id: device_tracker.paulus
  zone: zone.home
```

### {% linkable_title Examples %}
以下设定表示在14点至23点太阳落山时分，客厅的灯处于关闭状态，并且电灯关闭脚本没有运行时，条件触发。
```yaml
    condition:
      - condition: numeric_state
        entity_id: sun.sun
        value_template: '{{ state.attributes.elevation }}'
        below: 1
      - condition: state
        entity_id: light.living_room
        state: 'off'
      - condition: time
        before: '23:00:00'
        after: '14:00:00'
      - condition: state
        entity_id: script.light_turned_off_5min
        state: 'off'
```


