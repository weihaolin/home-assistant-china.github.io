---
layout: page
title: "Web server fingerprint"
description: "Use nmap to scan your Home Assistant instance."
date: 2016-10-06 08:00
sidebar: true
comments: false
sharing: true
footer: true
redirect_from: /details/webserver/
---

它直到像 [https://www.shodan.io](https://www.shodan.io/search?query=Home+Assistant) 这样的工具发起第一次请求查询 Home Assistant 进程的时候才有作用。
若想了解 Home Assistant 进程在 network sacnner 中的详情，你可以使用‘nmap’工具，若你正在使用[nmap device tracker](/components/device_tracker/)，则‘nmap’已经可用。

```bash
$ nmap -sV -p 8123 --script=http-title,http-headers 192.168.1.3

Starting Nmap 7.12 ( https://nmap.org ) at 2016-10-06 10:01 CEST
Nmap scan report for 192.168.1.3 (192.168.1.3)
Host is up (0.00011s latency).
PORT     STATE SERVICE VERSION
8123/tcp open  http    CherryPy wsgiserver
| http-headers: 
|   Content-Type: text/html; charset=utf-8
|   Content-Length: 4309
|   Connection: close
|   Date: Thu, 06 Oct 2016 08:01:31 GMT
|   Server: Home Assistant
|
|_  (Request type: GET)
|_http-server-header: Home Assistant
|_http-title: Home Assistant

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.70 seconds
```

