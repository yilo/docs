---
title: Redis
description: Introduction about Redis.
tags:
    - 2023
    - Architecture
    - Redis
---

# Redis

## 场景

### Master-Slave 数据不一致

Redis在满容有驱逐策略的情况下，Master/Slave 均有大量的Key驱逐淘汰，导致Master/Slave 主从不一致。正常情况下Master到达满容后根据驱逐策略淘汰Key并同步给Slave。所以Slave这种情况下不会因满容触发驱逐。  

Slaver 内存陡增可能因素

- 客户端链接
- 输入/输出缓冲区
- 业务数据存取访问
- 网络抖动
- Redis会占用内存开销的一个重要机制:Redis Rehash

### Redis Rehash 内部实现
- 为什么需要rehash
  
!!! cite ""
    链式哈希会产生一个问题，随着哈希表数据越来越多，哈希冲突越来越多，单个哈希桶链表上数据越来越多，查找时间复杂度退化到 O(n)。因此redis 会对哈希表做 rehash 操作。rehash 也就是增加现有的哈希桶数量，让逐渐增多的 entry 元素能在更多的桶之间分散保存，减少单个桶中的元素数量，从而减少单个桶中的冲突; 负载因子(used/size)来表述哈希冲突的激烈程度,负载因子越大，冲突越激烈。size表示哈希表的大小，也就是哈希桶的个数used表示有多少个 键值对实体(dictEntry)，used越多，哈希冲突的情况越多;

<div class="row" markdown>
<div class="col" markdown>

在Redis中，键值对（Key-Value Pair）存储方式是由字典（Dict）保存的，而字典底层是通过哈希表来实现的。通过哈希表中的节点保存字典中的键值对。
Redis Dict 中定义了两张哈希表，是为了后续字典的扩展作Rehash之用：

- 在Cluster模式下，一个Redis实例对应一个RedisDB(db0);
- 一个RedisDB对应一个Dict;
- 一个Dict对应2个Dictht，正常情况只用到ht[0]；ht[1] 在Rehash时使用。
  
</div>
<div class="col" markdown>

![](https://awps-assets.meituan.net/mit-x/blog-images-bundle-2018a/1ff650e3.png)

</div>
</div>

!!! cite ""
    - 渐进式rehash
    - 定时任务检查到服务器空闲时，触发rehash

!!! quote "相关详解"
    - [Redis的渐进式rehash原理](https://zhuanlan.zhihu.com/p/358366217)
    - [Redis 哈希表扩容、缩容以及rehash](https://zhuanlan.zhihu.com/p/589943321)
    - [美团针对Redis Rehash机制的探索和实践](https://tech.meituan.com/2018/07/27/redis-rehash-practice-optimization.html)