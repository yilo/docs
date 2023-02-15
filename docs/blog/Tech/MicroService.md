---
title: MicroServices
description: Introduction Of MicroService Governance.
tags:
    - 2023
    - Architecture
---

# MicroService

## Governance



### 流量治理方案

流量不稳定的风险，如：

> - 大促时瞬间洪峰流量导致系统超出最大负载，load 飙高
> - 热点商品击穿缓存，DB 被打垮
> - 调用端被不稳定服务拖垮，线程池被占满，导致整个调用链路卡死
  
- 通过制定合适的规则来适配不同场景下的对应方案:

![FlowControl](flowcontrol-rules.drawio)

- [OpenSergo](https://opensergo.io/)

OpenSergo is an open, language-agnostic cloud-native service governance specification that is close to business semantics

#### 流量控制

### 服务治理方案

微服务架构在处理用户请求时，需要依赖多个外部服务，通过RPC调用完成处理，当某些原因，下游服务响应变慢或不可用时，上游服务因等待下游返回而短时间堆积大量线程，直到到达超时时间才释放，流量到达一定数量后，上游服务器内存被占满，导致服务不可用，而且故障会继续向上传导，导致服务雪崩。

> - 缺乏依赖隔离
> - 缺乏容错机制
> - 缺乏自我保护

#### 服务熔断
【弱依赖场景】
当下游响应变慢或不可用时，上游感知到后主动不去调用下游服务，返回业务预设的数据，从而保证自身的服务稳定，等待下游服务恢复后重新调用。

#### 服务降级
【弱依赖场景】
因bug或服务资源不足，导致服务响应变慢，而主动放弃资源占用大的计算或对下游的调用，来增加响应速度和处理能力。被降级的服务将直接返回给定的 mock 值⽽不会触发实际调⽤。

### 缓存数据一致性

- 同一份数据，存在DB和缓存不一致可能性；
- 同一份数据，存在缓存副本之间不一致可能性；

#### 解决方案

- 最终一致性

<div class="row" markdown>
<div class="col" markdown>

先删缓存，在更新数据库

> - Thread1 delete item in Cache
> - Thread1 update item in DB
> - Thread2 get item from Cache or DB

</div>
<div class="col" markdown>

Risk
> After deleted item in Cache, the update in DB not completed, Request read nth from Cache, then read from DB, it is OLD value, cause dirty read

</div>
</div>

---
<div class="row" markdown>
<div class="col" markdown>

延时双删缓存

> - Thread1 delete item in Cache
> - Thread1 update item in DB
> - Thread1 sleep a while, then delete item in Cache again

</div>
</div>

---

<div class="row" markdown>
<div class="col" markdown>

- MQ发送更新缓存消息

![](https://static001.geekbang.org/infoq/20/2023723f8f214985eaea195aacfcb186.png)


![](https://static001.geekbang.org/infoq/f0/f0a1cb65833826b3dc8f7b1b1356b896.png)

</div>
</div>
- 强一致方案
  


---

#### 参考

<center><embed src="/docs/assets/pdf/微服务治理.pdf" type="application/pdf" width="100%" height="500"></center>