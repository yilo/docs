---
title: Message
description: Introduction about Architecture.
tags:
    - 2023
    - Architecture
    - Message
    - Kafka
    - RocketMQ
---

# Message

## Kafka


### At Least Once
消息不会丢失，但有可能被重复发送

### At Most Once
消息可能会丢失，但绝不会被重复发送

### Exact Once
消息不会丢失，也不会被重复发送

Kafka 通过幂等性 [Idempotence] & 事务 [Transaction] 来实现 Exact Once

#### 幂等性 [Idempotence]
```
props.put(“enable.idempotence”, true)
```
初始化时像向 Broker 申请一个 ProducerID  
为每条消息绑定一个 SequenceNumber  

Limitation  

- 保证某个Topic的单分区的幂等性，不出现重复消息，无法实现多个分区的幂等性
- 保证单会话的幂等性，不能跨会话，重启Producer后，由于需要重新申请ProducerID,之前的就丢失了

#### 事务 [Transaction]
Kafka 自 0.11 版本开始也提供了对事务的支持，目前主要是在 read committed 隔离级别上做事情。它能保证多条消息原子性地写入到目标分区，同时也能保证 Consumer 只能看到事务成功提交的消息。

事务型 Producer 能够保证将消息原子性地写入到多个分区中。这批消息要么全部写入成功，要么全部失败。另外，事务型 Producer 也不惧进程的重启。Producer 重启回来后，Kafka 依然保证它们发送消息的精确一次处理。

