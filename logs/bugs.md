---
layout: logs
title: Bugs log
permalink: /logs/bugs
---

# 2024

# January 1, 10

`TBE`中添加`bool write_callback`，当接收到来自L2的Ack response时，表明这是从sharers发起的get-m请求，此时可以调用sequencer的`writeCallback`，但是需要注意后续接收完成从L1的Ack response时，不能重复调用callback，因此添加这个标记位

# February 2, 20

---

Get-S请求不在SAT中占用记录，发生冲突时，

1. 最新version的推测共享者一定在sharers中，那么能确保送达invalidation消息

2. 不是最新version的推测访问者，则无法被跟踪到，导致恢复时无法计算正确的状态

---

推测线程load到l1的数据，如果发生回滚，只要没有被invalidate或者evict掉，则仍然是有效的

---

1. 修复推测写入后未生效的错误

2. 修复`S|NonSpEntrySpGetSL2`未添加sharer的问题

3. 考虑增加两个推测消息传递的通道`[l1:spOut] <=> [l2:spIn] [l1:spIn] <=> [l2:spOut]`
