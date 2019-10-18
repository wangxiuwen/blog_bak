---
title: ansible 终端切换用户执行命令
date: 2018-09-18 04:09:31
tags: ["tech","技术"]
author: wangxiuwen
categories: ["技术"]
layout: post
---

```
ansible hostA -S -R  remoteUser -m shell -a "pm2 l"
```