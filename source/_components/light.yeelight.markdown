---
layout: page
title: "Yeelight Wifi Bulb"
description: "Instructions how to setup Yeelight Wifi devices within Home Assistant."
date: 2016-10-29
sidebar: true
comments: false
sharing: true
footer: true
logo: yeelight.png
ha_category: Light
ha_release: 0.32
ha_iot_class: "Local Polling"
---

 `yeelight` 平台允许你在 HA 中接入 Yeelight 的 Wifi 灯具。

### {% linkable_title Example configuration %}

添加下列配置至 `configuration.yaml` 文件：

```yaml
light:
  - platform: yeelight
    devices:
      192.168.1.25:
        name: Living Room
        transition: 1000
        use_music_mode: True #(defaults to False)
        save_on_change: False #(defaults to True)
      192.168.1.13:
        name: Front Door
```

变量说明：

- **ip** (*必须*): IP
- **name** (*可选*): 昵称
- **transition** (*O可选*, 默认 350): 光效过渡时间（毫秒）
- **use_music_mode** (*可选*, 默认 False): 开启音乐随动模式，默认关闭
- **save_on_change** (*可选*, 默认 True): 保存当前状态为下次启动默认状态

#### {% linkable_title Music mode  %}
每个灯泡每分钟的网络请求数最大为 60，这个限制可以通过开启音乐随动模式突破。在音乐随动模式中，灯具会响应任何时间传输回的控制信号，也就是说通信协议始终处于开启状态。但是该项设置并不适合部分场景，请按需配置。

### {% linkable_title Initial setup %}
<p class='note'>
使用前请确保在 Yeelight app 中与设备配对，升级至最新固件并开启“极客模式”，同时确定设备的 ip。
</p>

<p class='note warning'>
该组件经过下列设备测试通过，如果你发现支持其他设备，请反馈给论坛或者群。
</p>

- **YLDP01YL**: LED 灯泡 (白光版)
- **YLDP02YL**: LED 灯泡 (彩光版)
- **YLDP03YL**: LED 灯泡 (彩光版) - E26
- **YLDD02YL&YLDD01YL**: 灯带 (彩光版)





