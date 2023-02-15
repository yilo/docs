---
title: Hot Key - Solution
description: Introduce background of Hot Key, and normal solutions.
tags:
    - 2023
    - Architecture
---
## Hot Key - Solutions

### Background

- What is Hot Key
  
  在一段时间内，被广泛关注的物品或事件，例如微博热搜，热卖商品，热点新闻，明星直播等等：

- 产生条件

> - 有限时间 
> - 流量高聚

- Type of Hot Key

---

<div class="row" markdown>
<div class="col" markdown>


- 有预期的热点

> - 电商活动
> - 秒杀等

</div>
<div class="col" markdown>

- 无预期的热点

> - 如受到了黑客的恶意攻击
> - 网络爬虫频繁访问
> - 或者突发新闻带来的流量冲击等

</div>
</div>
    

### 热点场景

短时间内被频繁访问导致流量高聚

- 业务场景

>   - MySQL中被频繁访问的数据 ，如热门商品的主键Id
>   - Redis缓存中被密集访问的Key，如热门商品的详情需要get goods$Id
>   - 恶意攻击或机器人爬虫的请求信息，如特定标识的userId、机器IP
>   - 频繁被访问的接口地址，如获取用户信息接口 /userInfo/ + userId


### 热点探测的价值

<div class="row" markdown>
<div class="col" markdown>


- 提升性能

    > 对热点数据提前进行本地缓存，即本地预热，就能大幅提升机器读取数据的性能，减轻下层缓存集群的压力；

    > 本地缓存与实时数据存在不一致的风险，缓存级数越多，数据不一致的风险就越大；

</div>
<div class="col" markdown>


- 规避风险

突发场景下形成的热Key,对业务系统带来极大的风险；

<div class="row" markdown>
<div class="col" markdown>

- 对数据层的风险

    > 瞬时过高并发的请求热点集中导致分片集群压力过载而瘫痪，从而击穿到DB引起服务器雪崩。

</div>
<div class="col" markdown>

- 对应用服务的风险

    > 在单位时间所能接受和处理的请求量是有限的,独占大量资源的请求会导致其他请求无法及时响应

</div>
</div>

</div>
</div>

### 功能需求

<div class="row" markdown>
<div class="col" markdown>

- 热点规则

> 配置热Key的上报规则,实时扫描监测Key 

</div>
<div class="col" markdown>

- 热点上报

> 应用服务将Hot Key访问状态上报到集中计算单元

</div>
<div class="col" markdown>

- 热点统计

> [各类]算法计算Key的热度

</div>
<div class="col" markdown>

- 热点推送

>  热度达到临界值,推送热点信息至应用服务

</div>
<div class="col" markdown>

- 热点缓存

> 应用服务实例对Hot Key 进行本地缓存

</div>
</div>

---

<div class="row" markdown>
<div class="col" markdown>

实时性

</div>
<div class="col" markdown>

高性能

</div>
<div class="col" markdown>

准确性

</div>
<div class="col" markdown>

一致性

> 保证数据一致性，不能出现数据错误。当Hot Key有变化，需要及时失效本地缓存

</div>
<div class="col" markdown>

可扩展

> 计算集群需要具备水平扩展的能力

</div>
</div>

### 主流方案 Main stream solutions
