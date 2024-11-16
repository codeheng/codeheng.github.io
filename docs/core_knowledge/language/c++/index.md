---
comments: true
---

# 介绍

!!! Abstract
    C++不是做业务开发，适合做和OS贴近比较近的那层

    - 量化、安全、高性能、CDN、游戏服务、自动驾驶、基础架构、推荐算法、虚拟化、dpdk网络、存储、流媒体服务器、金融业务
    - （垂直领域）
   
    **C/CPP 结合一门GC语言 e.g Go**


1. 语言特性
    * 数据结构和算法 & 各种设计模式 
    * 库：STL、C++新特性 
    * Linux工程：Makefile/Cmake、git/Svn、htop/top、netstat、tcpdump、iperf 
2. 网络
    * 网络编程：socket、IO多路复用select/poll/epoll、多进/线程、(非)阻塞、同步异步
        + 网络框架如何实现？
    * 网络原理：ethernet、ip、udp/tcp、http
        + TCP协议栈如何实现呢？  
3. 基础组件
    * 内存池、线程池等等、原子操作、ringbuffer....
4. 中间件
    * MySQL、Redis、Nginx、grpc、Message Queue(MQ)
5. 框架(适配行业)
    * workflow(网络范式)、spdk(存储)、openresty(CDN)、dpdk(网络)、cuda(GPU)
6. DevOps云原生：docker & K8S
7. 性能分析 & 分布式