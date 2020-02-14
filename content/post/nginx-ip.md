---
title: "两行Nginx配置构建获取公网IP地址API"
date: 2020-02-14T10:06:23+08:00
tags:
  - Nginx
categories: []
draft: false
---

我们经常会有获取客户端公网 IP 地址的需求，一般都会通过 Java、Python 等语言来实现。
本文介绍如何通过 Nginx 的两行配置信息就可以实现获取客户端公网 IP 地址。
<!--more-->

## 文本格式

Nginx 配置如下：

```nginx
location /ip {
    default_type text/plain;
    return 200 $remote_addr;
}
```

响应信息如下：

```bash
$ curl https://example.com/ip
2001:1b48:103::189
```

`default_type text/plain` 用来指示浏览器将响应信息作为普通文本来展示，Web 浏览器可以直接显示 IP 地址。

## JSON 格式

以下配置可以将响应信息格式化为 JSON：

```nginx
location /json_ip {
    default_type application/json;
    return 200 "{\"ip\":\"$remote_addr\"}";
}
```

JSON 格式的响应信息如下：

```bash
$ curl -s https://example.com/json_ip | jq
{
    "ip": "2001:1b48:103::189"
}
```

通过上述简单配置，即可创建一个获取客户端公网 IP 的 API。