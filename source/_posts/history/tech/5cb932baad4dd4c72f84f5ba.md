---
title: linux 删除第一行输出
date: 2019-04-19 10:30:18
tags: ["tech","技术"]
author: wangxiuwen
categories: ["技术"]
layout: post
---

```
awk '{if(NR>1)print}'
```

或
```
sed -n '1!p'
```