---
layout: page
title: "Script Syntax"
description: "Documention for the Home Assistant Script Syntax."
date: 2016-04-24 08:30 +0100
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /getting-started/scripts/
---

Scripts（脚本）是 Home Assistant 中可执行的一系列动作的统称。脚本可作为独立的实体接入 HA，详见 [Script component]，也可以嵌入自动化配置[automations] 和 亚马逊语音助手 [Alexa/Amazon Echo]中。

脚本的基础语法是有一系列的动作值组成的，如果脚本中只含有一个动作，则清单语法可被忽略。例：

```yaml
# 脚本作为组件的配置示例
script:
  example_script:
    sequence:
      # 此处使用了脚本语法
      - service: light.turn_on
        entity_id: light.ceiling
      - service: notify.notify
        data:
          message: 'Turned on the ceiling light!'
```

### {% linkable_title Call a Service %}

HA 中最重要的一项行动就是调用服务，可以通过多种方式实现。 具体调用方法，详见服务调用页 ([service calls page])，例如：

```yaml
#打开卧室的灯
alias: Bedroom lights on
service: light.turn_on
data:
  entity_id: group.bedroom
  brightness: 100
```

### {% linkable_title Test a Condition %}

在执行脚本时，你可以通过添加执行条件以停止继续执行。如果条件回传值不为正`true`（译者注：即为 `false` 或 `unknown`），则脚本将中止执行。具体条件设置详见此页（[conditions page]）。

```yaml
condition: state
entity_id: device_tracker.paulus
state: 'home'
```

### {% linkable_title Delay %}

延迟适用于想要临时暂停脚本执行并于稍后恢复执行的场合。对于延迟设置，有多种语法表达方式：

```yaml
# 延迟 1 小时
delay: 01:00
```

```yaml
# 延迟 1 小时 30 分钟
delay: 00:01:30
```

```yaml
# 延迟 1 分钟
delay:
  # 语法支持毫秒、秒、分钟、小时、日
  minutes: 1
```

```yaml
# 延迟输入框设定的时间input_slider.minute_delay 
# 时间表达的有效格式为 HH:MM 或 HH:MM:SS
delay: {% raw %}'00:{{ states.input_slider.minute_delay.state | int }}:00'{% endraw %}
```
### {% linkable_title Wait %}

我们使用`wait_template` 模板以支持延迟至某项时间完成，通过条件为真`true` 判定，详见[Template-Trigger](/getting-started/automation-trigger/#template-trigger)。在设置中，你可以另行添加延时命令`delay`实现时间完成时停滞一段时间后再执行脚本。


```yaml
# 等待至媒体播放器停止播放后执行
wait_template: {% raw %}"{{ states.media_player.floor.states == 'stop' }}"{% endraw %}
```

```yaml
# 等待至值小于 10 或 一分钟之后再执行
wait_template: {% raw %}"{{ states.climate.kitchen.attributes.valve < 10 }}"{% endraw %}
timeout: 00:01:00
```

### {% linkable_title Fire an Event %}

这项指令允许你触发 HA 中的事件。事件具有多种作用，它可以触发自动化场景或在组件间传递其他组件信息。例如下面的设置，就是开启事件记录的入口。

```yaml
event: LOGBOOK_ENTRY
event_data:
  name: Paulus
  message: is waking up
  entity_id: device_tracker.paulus
  domain: light
```

[Script component]: /components/script/
[automations]: /getting-started/automation-action/
[Alexa/Amazon Echo]: /components/alexa/
[service calls page]: /getting-started/scripts-service-calls/
[conditions page]: /getting-started/scripts-conditions/


