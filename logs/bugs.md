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

# February 2, 21

> 推测线程如何成为master线程？

1. l1向l2送出`sbegin`请求后，l2判断其`sequence`优先级，如果最高则设置该线程为`main thread`；l1接收到`main`信号时，则`commit`自身

2. 在l2，`main`线程`commit`时，如果存在运行中的最高优先级线程，则设置其为`main thread`并`commit`，同时向l1发出`main`信号，使其`commit`；所有`main`线程将会忽略`overflow fault`；意味着`overflow`信号不能走单独的通道

3. 在l1，回滚线程被标记为`main`s，则在发送`sbegin`重启时附带`main`信号，此时不需要`commit`自身

4. `main thread`不占用推测状态

5. 推测请求到达l2时需要等待`sbegin`在l2处理完成（发送方已经被设置成`speculative`）才能被处理

6. `l1 overflow`信号需要跟`put`请求绑定，否则会出现`ignore put`->`main-thread`->`ignore l1 overflow`的问题；`l2 overflow`则需要跟着`ack/data response`

7. 应当取消采用`overflow`需要等待执行的概念

8. 在l1，如果发生推测缓存行的替换，则不执行（因此不存在`l1 overflow`），阻塞`request port`，等待成为main_thread，接收到`commit`信号时再执行；或者在等待期间由于`conflict/access/l2 overflow fault`发生回滚