---
title: Spring Cloud Feign 超时设置小计
date: 2018-09-18 06:05:25
tags: ["tech","技术"]
author: baipeng
categories: ["技术"]
layout: post
---

```
设置所有的微服务 超时配置
feign:
  client:
    config:
      default:
        connectTimeout: 50000
        readTimeout: 50000
        loggerLevel: basic
```

```
单独设置某个微服务的超时配置
feign:
  client:
    config:
      server-A: # 设定server-A 服务调用的超时设置
        connectTimeout: 50000
        readTimeout: 50000
        loggerLevel: basic
```
```
如果以上都没有设置正确，设置Ribbon 的超时配置也是生效的。因为Feign 底层还是使用Ribbon 负载调用注册到注册中心的微服务的。
ribbon:
  ReadTimeout: 50000
  ConnectTimeout: 50000

```