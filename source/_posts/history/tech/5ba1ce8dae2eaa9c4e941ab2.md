---
title: multitail 多个文件
date: 2018-09-19 12:20:29
tags: ["tech","技术"]
author: wangxiuwen
categories: ["技术"]
layout: post
---

`tail` 只能 tail 一个文件，使用 `multitail` 可以同时查看多个文件

```
apt install multitail
multitail -i 1.log -i 2.log
```