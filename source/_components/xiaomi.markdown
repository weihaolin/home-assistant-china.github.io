---
layout: page
title: "Xiaomi Gateway"
description: "Instructions how to integrate your Xiaomi Gateway within Home Assistant."
date: 2017-07-21 16:34
sidebar: true
comments: false
sharing: true
footer: true
logo: xiaomi.png
ha_category: Hub
ha_release: "0.50"
ha_iot_class: "Local Polling"
---

小米平台 `xiaomi_gw` 允许你在 HA 中接入小米的 Zigbee 智能家居设备。支持的设备包括：

- 温度湿度传感器（新旧版）
- 人体传感器 （新旧版）
- 门窗传感器 （新旧版）
- 无线开关 （新旧版）
- 智能插座 Zigbee 版本 （反馈耗电、电力负载、通电、开关状态）
- 墙壁开关 (反馈耗电、电力负载、 通电状态）
- 绿米（Aqara）全系列墙壁开关，包括单、双键；单、零火；
- 绿米 （Aqara）无线开关单、双键
- 魔方控制器
- 天然气传感器 (反馈警报和浓度信息）
- 烟雾报警器 (反馈警报和浓度信息)
- 多功能网关 (支持网关灯、亮度传感器及铃声播放)
- 绿米智能窗帘器
- 各设备电量


不支持：

- 网关广播
- 网关案件
- 水浸传感器
- 绿米及米家空调伴侣
- 墙壁开关独立使用状态
- 烟雾及天然气报警器的其他警报事件，包括：模拟警报、电池错误警报、灵敏度警报及 I2C 通信失败警报。


请在手机中安装米家 app 并获取多功能网关的通信密码。

（译者注：请在米家 app 中与网关配对，之后进入网关页，点选右上角“……” —— 关于 —— 空白处点多下 —— 局域网通信协议 —— 打开并获取密码）

之后在 `configuration.yaml` 文件中填入如下配置：

单个网关

```yaml
# 使用单网关前提下，可不填 mac
xiaomi:
  gateways:
   - mac:
     key: xxxxxxxxxxxxxxxx
```


多个网关

```yaml
# 多个网关必须填入 mac
xiaomi:
  gateways:
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
```



 搜寻固定 IP 的网关

```yaml
#  mac 必须填写
xiaomi:
  interface: '192.168.0.1'
  gateways:
    - mac: xxxxxxxxxxxx
      key: xxxxxxxxxxxxxxxx
```


变量说明：

- **mac** (*可选*): 网关 mac 地址，使用多个网关则必须填写
- **key** (*可选*): 网关通信协议密码。如果想要控制网关灯和开关，则必须填写；传感器在无密码情况下仍可正常运作。
- **discovery_retry** (*可选*): 连接失败重试次数，默认为 3。
- **interface** (*可选*): 所使用的接口，默认为全部(all）。

## {% linkable_title Services %}

HA 支持网关铃声的两个操作：播放 `xiaomi.play_ringtone` 和停止`xiaomi.stop_ringtone`。网关铃声的播放需要 `1.4.1_145` 及以上网关固件支持。 同时必须提供铃声 ID `ringtone_id`  和网关 Mac `gw_mac`。 铃声音量控制 `ringtone_vol` 是可选的。全部铃声 ID`ringtone_id`  有：

- alarm ringtones [0-8]
- doorbell ring [10-13]
- alarm clock [20-29]
- custom ringtones (uploaded by mi home app) starting from 10001

（译者注：中文对应名称大家可前往米家 app 中查询）

自动化示例

```yaml
- alias: Let a dog bark on long press #长按开关后网关发出狗叫
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.switch_158d000xxxxxc2
      click_type: long_click_press
  action:
    service: xiaomi.play_ringtone
    data:
      gw_mac: xxxxxxxxxxxx
      ringtone_id: 8
      ringtone_vol: 8

- alias: Stop barking immediately on single click
  trigger:
    platform: event
    event_type: click
    event_data:
      entity_id: binary_sensor.switch_158d000xxxxxc2
      click_type: single
  action:
    service: xiaomi.stop_ringtone
    data:
      gw_mac: xxxxxxxxxxxx
```


