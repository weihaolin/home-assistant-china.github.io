---
layout: page
title: "Xiaomi Router"
description: "Instructions how to integrate Xiaomi routers into Home Assistant."
date: 2017-01-12 12:04
sidebar: true
comments: false
sharing: true
footer: true
logo: xiaomi.png
ha_category: Presence Detection
ha_release: 0.36
---


小米路由器组件通过检测设备与路由器的连接情况，从而提供设备存在状态。

使用小米路由器，请在 `configuration.yaml` 文件中添加如下配置：

```yaml
device_tracker:
  - platform: xiaomi
    host: YOUR_ROUTER_IP
    password: YOUR_ADMIN_PASSWORD
```

变量说明：

- host (必需): 路由器 IP，如192.168.0.1
- username (可选）: 管理员账户名称，默认为 admin.
- password (可选): 管理员账户密码


如何设置被嗅探设备状态，详见[设备追踪文档](/components/device_tracker/)

