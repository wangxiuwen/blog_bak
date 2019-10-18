---
title: warning setlocale LC_CTYPE cannot change locale (zh_CN.UTF-8)
date: 2018-09-13 07:35:26
tags: ["tech","技术"]
author: wangxiuwen
categories: ["技术"]
layout: post
---

报错：

```
warning: setlocale: LC_CTYPE: cannot change locale (zh_CN.UTF-8)
```

解决：

```
	vim  /etc/locale.gen
	# 取消注释
	zh_CN.UTF-8 UTF-8
```
执行：
```
locale-gen
```