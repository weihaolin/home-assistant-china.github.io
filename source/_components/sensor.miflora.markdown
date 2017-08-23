---
layout: page
title: "Mi Flora plant sensor"
description: "Instructions on how to integrate MiFlora BLE plant sensor with Home Assistant."
date: 2016-09-19 12:00
sidebar: true
comments: false
sharing: true
footer: true
logo: miflora.png
ha_category: DIY
ha_release: 0.29
ha_iot_class: "Local Polling"
---

[花花草草监测仪平台](https://www.aliexpress.com/item/Newest-Original-Xiaomi-Flora-Monitor-Digital-Plants-Flowers-Soil-Water-Light-Tester-Sensor-Monitor-for-Aquarium/32685750372.html) `miflora` 帮助你在 HA 中接入监测仪。 花花草草监测仪使用蓝牙传输信息，由于低功耗蓝牙传输协议的设置，每次只能与一个设备进行信息交流，此插件适应了这一特性。

在设备中开启扫描获取监测仪的 Mac 地址： 

```bash
$ sudo hcitool lescan
LE Scan ...
F8:04:33:AF:AB:A2 [TV] UE48JU6580
C4:D3:8C:12:4C:57 Flower mate
[...]
```

（译者注：另可通过安卓手机搜寻蓝牙设备，过程更为简单快捷）

寻找名为 `Flower care` 或 `Flower mate` 的设备，记录下 Mac。

将下列配置添加至`configuration.yaml` 文件：

```yaml
sensor:
  - platform: miflora
    mac: 'xx:xx:xx:xx:xx:xx'
    monitored_conditions:
      - temperature
```
变量说明：

- **mac** (*必需*): 监测仪的 mac 地址
- **monitored_conditions** array (*可选*): 监测仪需要检测的数据，默认监测所有数据：
  - **moisture**: 土壤湿度
  - **light**: 亮度
  - **temperature**: 温度
  - **conductivity**: 肥力（土壤导电性）
  - **battery**: 电量
- **name** (*可选*): 昵称
- **force_update** (*可选*): 即使数据没有变化，依旧强制推送数据
- **median** (*可选*): 监测仪有时候只会显示数据的峰值，使用这个变量，数据将取各次推送的平均值，保证准确性。默认为 3 。
- **timeout** (*可选*): 数据推送时间间隔，默认为 10。（秒） 
- **retries** (*可选*): 推送失败后的重试次数，默认为 2。
- **cache_value** (*可选*): 缓存数据容量，默认为 1200。
- **adapter** (*可选*): 蓝牙适配器接口，默认为 hci0 。运行`hciconfig` 获取蓝牙适配器列表。

请注意监测仪默认每 15 分钟推送一次数据，这意味着如果设定为取 3 次数据的传送值，则一共需要花费30分钟 HA 才会显示数据。但是，通常情况下，由于数据在短时间内变动不大，因此可以容忍。如果强制缩短获取信息的时间，不利于电池的寿命。

完整的配置如下：

```yaml
sensor:
  - platform: miflora
    mac: 'xx:xx:xx:xx:xx:xx'
    name: Flower 1
    force_update: false
    median: 3
    monitored_conditions:
      - moisture
      - light
      - temperature
      - conductivity
      - battery
```

