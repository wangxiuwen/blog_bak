---
title: systemctl 列表
date: 2018-09-21 06:52:28
tags: ["tech","技术"]
author: wangxiuwen
categories: ["技术"]
layout: post
---

```
systemctl list-unit-files | grep enabled
systemctl list-unit-files --state=enabled
systemctl list-units --type=service --state=running
systemctl list-units --type=service --state=active
```