---
layout: page
title: "Service Calls"
description: "Instructions how to call services in Home Assistant."
date: 2016-03-12 12:00 -0800
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /getting-started/scripts-service-calls/
---

许多组件都允许通过触发特定的事件调用服务，最常见的是自动化场景触发。 但是脚本和 亚马逊语音服务（Amazon Echo）同样也可以调用服务。

本页所展示的调用服务使用的配置格式在所有组件中通用。

本页将使用自动化组件演示如何调用服务，可使用的调用方式和可适用的组件有很多并不限于自动化部分，请具体问题具体分析。

<p class='note'>
可以使用主页面左侧栏中的开发者工具<img src='/images/screenshots/developer-tool-services-icon.png' class='no-shadow' height='38' /> 发现可调用的服务。
</p>

### {% linkable_title The basics %}

以下配置演示如何在群组 `group.living_room` 中调用 `homeassistant.turn_on` 服务。该项设定将开启使群组`group.living_room`中的所有设备。 你也可以省略 `entity_id` 项，这样这项设定将自动打开所有可供开启的设备。

```yaml
service: homeassistant.turn_on
entity_id: group.living_room
```

### {% linkable_title Passing data to the service call %}

你也可以在配置中声明相关设备的其他变量。例如，你可以设置开启电灯至某一特定亮度和颜色：

```yaml
service: light.turn_on
entity_id: group.living_room
data:
  brightness: 120
  rgb_color: [255, 0, 0]
```

### {% linkable_title Use templates to decide which service to call %}

你可以运用模板 [templating] 动态选择所调用的服务。例如，你可以在开灯状态下调用特定的服务：
```yaml
service_template: >
  {% raw %}{% if states.sensor.temperature.state | float > 15 %}
    switch.turn_on
  {% else %}
    switch.turn_off
  {% endif %}{% endraw %}
entity_id: switch.ac
```

### {% linkable_title Using the Services Developer Tool %}

你可以使用开发者工具中的服务面板测试所要调用的服务。

比如，你想要测试开启或关闭群组，有关群组的知识，详见[群组]文档。

开启群组，请在面板中填入如下值:
Domain: `homeassistant`
Service: `turn_on`
Service Data: `{ "entity_id": "group.kitchen" }`


### {% linkable_title Use templates to determine the attributes %}

传输至所调用服务的信息同样可以使用模板。

```yaml
service: thermostat.set_temperature
data_template:
  entity_id: >
    {% raw %}{% if is_state('device_tracker.paulus', 'home') %}
      thermostat.upstairs
    {% else %}
      thermostat.downstairs
    {% endif %}{% endraw %}
  temperature: {% raw %}{{ 22 - distance(states.device_tracker.paulus) }}{% endraw %}
```

[templating]: /topics/templating/


