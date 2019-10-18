---
title: kube-proxy
date: 2019-04-19 11:43:24
tags: ["tech","技术","k8s"]
author: wangxiuwen
categories: ["技术"]
layout: post
---

kube-proxy 在 Linux 系统上当前支持三种模式，可通过 --proxy-mode 配置：

```
userspace：这是很早期的一种方案，但效率上显著不足，不推荐使用
iptables：当前的默认模式。比 userspace 要快，但问题是会给机器上产生很多 iptables 规则
ipvs：为了解决 iptables 的性能问题而引入，采用增量的方式进行更新
```

iptables ：

```
当开始访问的时候先要经过 PREROUTING 链，转到 KUBE-SERVICES 链，当查询到匹配的规则之后，请求将转向 KUBE-SVC-SMQNAAUIAENDDGYQ 链，进而到达 KUBE-SEP-QX7VDAS5KDY6V3EV 对应于我们的 Pod。(注：为了简洁，上述 iptables 规则是部署一个 Pod 时的场景)
iptables 规则实际是如何创建和维护的，参考下 proxier 的具体实现
```

NodePort 类型的 Service 查看方式：

```
 kubectl -n work get all
netstat  -ntlp |grep 30154 // kube-proxy 监听在此 NodePort
docker run --rm -it --network host redis:alpine redis-cli -p 30154
```

查看当前集群的 Service 和 Endpoint:

```
kubectl -n work get svc
kubectl -n work get endpoints
kubectl -n work get pod -o wide
```

扩容：
```
kubectl  -n work scale --replicas=2 deploy/saythx-redis
kubectl  -n work get all
kubectl -n work get endpoints
```

kube-proxy 的 session affinity:

--待完成--