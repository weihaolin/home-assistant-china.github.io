---
layout: page
title: "Events"
description: "Describes all there is to know about events in Home Assistant."
date: 2016-03-12 12:00 -0800
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /topics/events/
---

家庭助理的核心是事件总线。事件总线允许任何组件触发或者侦听事件，它是核心.例如，任何状态的更改将在事件总线上声明为 包含之前和现在的状态改变。

家庭助理包含一些内置的事件，被用来协调各个组件之间。

### {% linkable_title Event `homeassistant_start` %}
当所有组件从配置文件中初始化时 事件HOMEASSISTANT_START 会被触发。这个事件将会启动定时器触发time_changed事件。

### {% linkable_title Event `homeassistant_stop` %}
当家庭助理停止运行时 事件 HOMERASSISTANT_STOP 将会被触发.他应当用来关闭任意已打开连接或者释放资源


### {% linkable_title Event `state_changed` %}
当状态改变时 事件 STATE_CHANGED 将被触发.  old_state和new_state都是状态对象 [关于状态对象的文档](/topics/state_object/)

属性 | 描述
----- | -----------
`entity_id` | 已更改实体的实体id。例如：`light.kitchen`
`old_state` | 变更前实体的先前状态。如果实体是新的，则该属性将被忽略。
`new_state` | 实体的新状态。如果实体从状态中被删除，则该属性将被忽略。


### {% linkable_title Event `time_changed` %}
事件time_changed每秒钟由计时器触发，并包含当前时间

属性 | 描述
----- | -----------
`now` |  [日期对象](https://docs.python.org/3.4/library/datetime.html#datetime.datetime) 一个包含目前UTC的时间对象


### {% linkable_title Event `service_registered` %}
在家庭助理中当有新的服务被注册时候该事件将会被触发 事件 SERVICE_REGISTERED

属性 | 描述
----- | -----------
`domain` | 服务对象。例子： `light`.
`service` | 要调用的服务。例如: `turn_on`


### {% linkable_title Event `call_service` %}
事件call_service 触发调用一个服务。

属性 | 描述
----- | -----------
`domain` | 服务对象。例子： `light`.
`service` | 要调用的服务。例子： `turn_on`
`service_data` | 使用服务调用参数的字典。例如：`{ 'brightness': 120 }`.
`service_call_id` | 具有唯一调用id的字符串。例如：`23123-4`.


### {% linkable_title Event `service_executed` %}
事件service _executed由服务处理程序触发，以表示服务已完成。

属性 | 描述
----- | -----------
`service_call_id` | S具有唯一调用id的字符串。例如：`23123-4`.


### {% linkable_title Event `platform_discovered` %}
当发现组件发现新的平台时将触发 事件 PLATFORM_DISCOVERED

属性 | 描述
----- | -----------
`service` | 被发现的服务。例如: `zwave`.
`discovered` | 包含发现信息的字典。例如： `{ "host": "192.168.1.10", "port": 8889}`.


### {% linkable_title Event `component_loaded` %}
当发现组件发现了一个新平台时将触发 事件COMPONENT_LOADED

属性 | 描述
----- | -----------
`component` | 刚刚初始化的组件的对象。例如: `light`.
