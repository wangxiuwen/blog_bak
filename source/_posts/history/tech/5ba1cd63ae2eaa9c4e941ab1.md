---
title: python 报错 AttributeError  'module' object has no attribute 'SSL_ST_INIT'
date: 2018-09-19 12:15:31
tags: ["tech","技术"]
author: wangxiuwen
categories: ["技术"]
layout: post
---

报错：
```
root@xxx:~# pip
Traceback (most recent call last):
  File "/usr/local/bin/pip", line 7, in <module>
    from pip._internal import main
  File "/usr/local/lib/python2.7/dist-packages/pip/_internal/__init__.py", line 42, in <module>
    from pip._internal import cmdoptions
  File "/usr/local/lib/python2.7/dist-packages/pip/_internal/cmdoptions.py", line 16, in <module>
    from pip._internal.index import (
  File "/usr/local/lib/python2.7/dist-packages/pip/_internal/index.py", line 15, in <module>
    from pip._vendor import html5lib, requests, six
  File "/usr/local/lib/python2.7/dist-packages/pip/_vendor/requests/__init__.py", line 86, in <module>
    from pip._vendor.urllib3.contrib import pyopenssl
  File "/usr/local/lib/python2.7/dist-packages/pip/_vendor/urllib3/contrib/pyopenssl.py", line 46, in <module>
    import OpenSSL.SSL
  File "/usr/lib/python2.7/dist-packages/OpenSSL/__init__.py", line 8, in <module>
    from OpenSSL import rand, crypto, SSL
  File "/usr/lib/python2.7/dist-packages/OpenSSL/SSL.py", line 118, in <module>
    SSL_ST_INIT = _lib.SSL_ST_INIT
AttributeError: 'module' object has no attribute 'SSL_ST_INIT'
```
解决：
```
rm -rf /usr/lib/python2.7/dist-packages/OpenSSL
rm -rf /usr/lib/python2.7/dist-packages/pyOpenSSL-0.15.1.egg-info
sudo pip install pyopenssl
```