---
layout: logs
title: Bugs log
permalink: /logs/bugs
---

# 2024

# January 1, 10

`TBE`中添加`bool write_callback`，当接收到来自L2的Ack response时，表明这是从sharers发起的get-m请求，此时可以调用sequencer的`writeCallback`，但是需要注意后续接收完成从L1的Ack response时，不能重复调用callback，因此添加这个标记位