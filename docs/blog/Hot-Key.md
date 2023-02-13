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

!!! info "产生条件"

    - 有限时间 
    - 流量高聚

- Type of Hot Key


<div class="row" markdown>
<div class="col" markdown>


!!! info "有预期的热点"

    - 电商活动
    - 秒杀等

</div>
<div class="col" markdown>

!!! info "无预期的热点"

    - 如受到了黑客的恶意攻击
    - 网络爬虫频繁访问
    - 或者突发新闻带来的流量冲击等

</div>
</div>
    

### 热点场景

短时间内被频繁访问导致流量高聚

!!! info "业务场景"

    - MySQL中被频繁访问的数据 ，如热门商品的主键Id
    - Redis缓存中被密集访问的Key，如热门商品的详情需要get goods$Id
    - 恶意攻击或机器人爬虫的请求信息，如特定标识的userId、机器IP
    - 频繁被访问的接口地址，如获取用户信息接口 /userInfo/ + userId


### 热点探测

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

