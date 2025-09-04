---
title: Kafka
markmap:
  colorFreezeLevel: 2
---

## ISR `In-Sync Replicas (和Leader保持同步的副本动态集合)`
- 生产消息（写多少副本才算成功？）`Request.required.acks= -1`
- Leader挂掉后，选择哪个Follower为新的Leader
- 如果ISR为空怎么办？min.insync.replicas配置ISR中至少有多少个副本，若小于配置的数量则写入失败（建议min.insync.replicas和unclean.leader.election.enable=false禁止极端情况落后副本被选举个Leader同时设置）

## 生产者消息幂等
- 生产者发送消息时，设置`enable.idempotence=true`，开启幂等性
- 原理：生产者初始化时，broker会针对每个Producer分配<PID, Epoch>，生产者会为发送的消息分配一个从 0 开始单调递增的序列号，Broker 端会为每个 <PID, Epoch>维护一个缓存，记录最后成功写入的消息的 SN
- 如果需要跨分区、跨会话，必须通过设置`transactional.id`来启用Kafka事务，并且Broker端也会根据Epoch来阻止僵尸实例提交数据


