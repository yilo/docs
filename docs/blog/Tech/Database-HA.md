---
title: Database-HA
description: Introduction about high availablity of Database.
tags:
    - 2023
    - Architecture
    - Database
    - High Availablity
---

# High Availablity Of Database

## Main stream database 

### [Oceanbase](https://www.oceanbase.com/docs/community-observer-cn-10000000000449889)

OceanBase 数据库是一个金融级分布式关系数据库，提供社区版和企业版

<div class="row" markdown>
<div class="col" markdown>
![](https://help-static-aliyun-doc.aliyuncs.com/assets/img/zh-CN/7726968461/p355597.jpg)
</div>
<div class="col" markdown>
OceanBase 数据库中存储的数据分布在一个 Zone 的多个数据节点上，其它 Zone 存放多个数据副本。如图所示的 OceanBase 数据库集群中的数据有三个副本，每个 Zone 存放一份。这三个 Zone 构成一个整体的数据库集群，为用户提供服务。

- 数据库集群部署在一个机房的多台服务器时:服务器（Server）级无损容灾：能够容忍单台服务器不可用，自动无损切换。

- 集群的服务器在一个地区的多个机房时：机房（Zone）级无损容灾：能够容忍单个机房不可用，自动无损切换。

- 集群的服务器在多个地区的多个机房地区时：（Region）级无损容灾：能够容忍某个城市整体不可用，自动无损切换。
</div>
</div>