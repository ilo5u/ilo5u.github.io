---
layout: note
title: SPEC CPU Benchmark notes
permalink: /notes/spec/
---

# SPEC CPU

> SPECspeed和SPECrate的区别

speed强调单线程性能，衡量单个任务的执行速度有多快

rate强调吞吐，在一定时间内完成的任务量，衡量多个任务并行执行的速度有多快

> Intel Parallel Studio XE-19 (IPS)

IPS编译器，支持IPO（inter-procedural optimization）、PGO（profile-guided optimization）和HLO（high-level optimization）优化，能针对x86架构的硬件特性，在串行程序中实现自动并行化（多线程）和向量化