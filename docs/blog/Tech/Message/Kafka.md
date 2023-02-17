---
title: Kafka
description: Introduction about Redis.
tags:
    - 2023
    - Architecture
    - Message
    - Kafka
---


# [Kafka](https://kafka.apache.org/documentation/#gettingStarted)



## Infrastructure

<div class="row" markdown>
<div class="col" markdown>
![](https://img-blog.csdnimg.cn/img_convert/8dc949e440d59af4ec0ee543b769e183.png)
</div>
<div class="col" markdown>

- Kafka 集群包含一个或多个服务器，服务器节点称为broker
- 每条发布到Kafka集群的消息都有一个类别，这个类别被称为Topic
- Topic中的数据分割为一个或多个partition。每个topic至少有一个partition
- Partition使用多个segment文件存储,同一个partition中的数据是有序的，不同partition间的数据丢失了数据的顺序。如果topic有多个partition，消费数据时就不能保证数据的顺序。
    - 分区的作用就是提供负载均衡的能力，或者说对数据进行分区的主要原因，就是为了实现系统的高伸缩性（Scalability）
    - Partition有多个副本，其中有且仅有一个作为Leader，Leader是当前负责数据的读写的partition,写请求都通过Leader路由，数据变更会广播给所有Follower，Follower与Leader保持数据同步.
- Producer将消息发布到Kafka的topic中。
- Consumer从broker中读取数据。消费者可以消费多个topic中的数据
- ConsumerGroup 一条消息可以发送到不同的consumer group，但一个consumer group中只能有一个consumer能消费这条消息
- Offset,kafka的存储文件都是按照offset.kafka来命名，用offset做名字的好处是方便查找。Consumer保存的元数据是offset值。

</div>
</div>

## Why Kafka has high performance

### Storage
Kafka文件存储中，同一个topic下有多个不同partition，每个partition为一个目录，partiton命名规则为topic名称+有序序号，第一个partiton序号从0开始，序号最大值为partitions数量减1。每个partition(目录)被平均分配到多个大小相等segment(段)数据文件中。但每个段segment file消息数量不一定相等，通过多个小文件段，就容易定期清除或删除已经消费完文件，减少磁盘占用。每一个sgement又包含了index文件和log文件，可以快速定位数据，通过index元数据全部映射到memory，可以避免segment file的IO磁盘操作.

### Using page cache+mmap(Memory Mapped Files)
page cache 加速数据的IO，写数据的时候首先写入缓存，将写入的页进行标记为dirty，之后向外部存储flush；读数据的时候就先读取缓存，没有读取到再去外部存储读取.

### Using compression 
消息的压缩，提高网络IO及磁盘IO性能

### Using ZeroCopy
请求kernel直接把disk的data传输给socket,减少不必要的内核缓冲区跟用户缓冲区间的拷贝，从而减少CPU的开销和减少了kernel和user模式的上下文切换，达到性能的提升.


## 参考
!!! cite ""
    - [KAFKA/Ecosystem](https://cwiki.apache.org/confluence/display/KAFKA/Ecosystem)