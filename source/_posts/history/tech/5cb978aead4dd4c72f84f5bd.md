---
title: k8s Troubleshoot
date: 2019-04-19 03:28:46
tags: ["tech","技术"]
author: wangxiuwen
categories: ["技术"]
layout: post
---

### 应用部署问题
```
kubectl -n work describe pod/saythx-redis-xxx // describe
kubectl -n work get events // 查看 Events。kubelet 或者 kube-scheduler 等组件会接受某些事件等，event 便是用于记录集群内各处发生的事件之类的
```

 修正错误
- 修正配置文件

```
kubectl apply -f redis-deployment.yaml
```
- 在线修改配置

```
// 这种做法比较适合比较紧急或者资源是直接通过命令行创建等情况。 非特殊情况尽量不要在线修改
kubectl -n work edit deploy/saythx-redis 
```

- 测试:
```
//  --net host 是使用宿主机网络； --rm 表示停止完后即清除； -it 分别表示获取输入及获取 TTY
docker run --rm -it --net host redis redis-cli -p 32355
127.0.0.1:32355> ping
```

另外一种排查思路:

```
kubectl -n work get endpoints
```

### 集群问题

查看集群中节点的状态：
```
 kubectl get  node/node01 -o yaml
```