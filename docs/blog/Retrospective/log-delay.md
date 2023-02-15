---
title: 日志查询延迟
description: 
tags:
    - 2023
    - 复盘
---

# 日志查询延迟

### 现象&Root Cause

日志查询延迟20Mins+

> - 共享日志资源，ES资源不足，部分logstore突增10times,导致日志写入堆积
> - ES长尾问题 Tail Latency
> - 降级策略未及时生效  
>   Agent已降级采样频率，由于检测时间较长，导致数据已大量上报至Kafka，并写入ES请求，需要等待ES消费完.  
>   提高检测频率，并对积压数据进行淘汰

### 解决方案