---
layout: page
title: "实体的个性化"
description: "简单的实体前端个性化"
date: 2016-04-20 06:00
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /getting-started/customizing-devices/
---

默认情况下，接入HomeAssistant的所有设备都是可见状态，并且根据其所属类型的不同，都会被分配一个默认图标。通过修改下面一些参数，你可以自定义这些设备在前端网页的显示效果。具体是通过覆盖特定实体的某些属性来实现的。

<p class='note'>
请记住，一定要把 `customize`, `customize_domain` 和 `customize_glob` 放在 `homeassistant:` 字段下，否则会配置失败。
</p>

```yaml
homeassistant:
  name: Home
  unit_system: metric
  # 等等其他

  customize:
    # 为每一个你想自定义的实体，添加一个自定义设置
    sensor.living_room_motion:
      hidden: true
    thermostat.family_room:
      entity_picture: https://example.com/images/nest.jpg
      friendly_name: Nest
    switch.wemo_switch_1:
      friendly_name: Toaster
      entity_picture: /local/toaster.jpg
    switch.wemo_switch_2:
      friendly_name: Kitchen kettle
      icon: mdi:kettle
    switch.rfxtrx_switch:
      assumed_state: false
  # 为一类实体添加自定义设置
  customize_domain:
    light:
      icon: mdi:home
  # 为满足某些条件的实体添加自定义设置，支持通配符
  customize_glob:
    "light.kitchen_*":
      icon: mdi:description

```

### {% linkable_title 可能的值 %}

| 属性 | 描述 |
| --------- | ----------- |
| `friendly_name` | 实体名称（可使用汉字，汉化一般在此处）
| `hidden`    | 设置为 `true` 表示隐藏实体，`false` 表示显示实体
| `entity_picture` | 要设置为实体图标的图像URL
| `icon` | [MDI](http://MaterialDesignIcons.com)网站上所有支持的图标，格式为：mdi:图标名称
| `assumed_state` | 非状态反馈（假定状态）开关会默认显示为 ` 开` 和 `关` 两个按钮，如果设置为 `false` 则会显示为默认滑块开关
| `device_class` | 设置设备的类别，更改设备类别后前端界面显示会有相应变化（见下文）

### {% linkable_title 设备类别 %}

目前，仅以下两个平台支持设备类别属性：

* [Binary Sensor](/components/binary_sensor/)（二进制传感器，如仅有 `开` 和 `关` 两个状态）
* [Cover](/components/cover/)（电动门/窗）

### {% linkable_title 重新载入个性化设置 %}

HomeAssistant提供一个服务用来重新载入核心配置，服务的名称为 `homeassistant/reload_core_config`。这就允许你在不重启HomeAssistant的情况下，使个性化设置生效。点击<img src='/images/screenshots/developer-tool-services-icon.png' alt='service developer tool icon' class="no-shadow" height="38" /> 服务开发者工具，选择 `homeassistant/reload_core_config` 然后点击 "Call Service" 即可调用此服务。

<p class='note warning'>
新的个性化设置信息将在该实体下一次状态更新时起作用。
</p>
