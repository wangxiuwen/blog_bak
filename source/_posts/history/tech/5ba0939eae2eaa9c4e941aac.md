---
title: Spring boot 2.0 升级 jpa AbstractUUIDGenerator  实现改动
date: 2018-09-18 01:56:46
tags: ["tech","技术"]
author: baipeng
categories: ["技术"]
layout: post
---

```
//    public Serializable generate(SessionImplementor session, Object obj) {
////        SnowFlake snowFlake = new SnowFlake(machineId);
//        String id = this.tablePrefix + Long.toString(snowFlake.nextId());
//        return id;
//    }

    @Override
    public Serializable generate(SharedSessionContractImplementor session, Object object) throws HibernateException {
        String id = this.tablePrefix + Long.toString(snowFlake.nextId());
        return id;
    }
2.0 一下版本使用会报错。由于generate的参数变化了，所以在动态加载函数的时候  java.lang.AbstractMethodError: *.model.entity.KeyUtils.generate(Lorg/hibernate/engine/spi/SharedSessionContractImplementor;Ljava/lang/Object;)Ljava/io/Serializable; 

```