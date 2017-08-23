---
layout: page
title: "Tools"
description: "Description of tools which helps when using Home Assistant."
release_date: 2017-02-23 11:00:00
sidebar: true
comments: false
sharing: true
footer: true
---

HA 系统所用的命令是为了简化任务运行、帮助系统迁移及保证系统正常运转。切勿与 HA 中 脚本相混淆[script](/docs/scripts/)。

### {% linkable_title Configuration check %}

你可以在运行 HA 前检测配置文件 `configuration.yaml` 的有效性，下列命令可以帮助你在重启 HA 前就检查你对配置文件所做的修改是否有效。

```bash
$ hass --script check_config
```

### {% linkable_title Existance of configuration %}

下列命令将会检查配置文件 `configuration.yaml`是否存在，如果不存在，系统将会自动生成一个。

```bash
$ hass --script ensure_config
```

### {% linkable_title Secrets %}

下列命令帮助你在 `configuration.yaml` 外保存秘钥，有关秘钥信息的设置，详见 [储存密码](/docs/configuration/secrets/) 文档。

```bash
$ hass --script keyring
```

### {% linkable_title Benchmark %}

测试 HA 的性能，我们可以给 HA 跑跑分，进程将不断运行，退出请按 Control+C。

这将触发并执行百万个事件（所以慎用）

```bash
$ hass --script benchmark async_million_events
```

### {% linkable_title Old scripts %}

下列命令仅使用于重大更新发布时。

- `db_migrator`: 迁移 SQLite 数据库
- `influxdb_migrator`: 将老式 InfluxDB 转换为新格式


