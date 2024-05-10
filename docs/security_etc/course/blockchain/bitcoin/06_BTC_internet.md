---
comments: true
---

用户把交易发布到网络上，节点收到后打包区块中，把区块也发布BTC网络上

- **Q: 新发布的交易、区块在比特币网络如何传播？**

## The bitcoin Network

工作在应用层，下面网络层运行P2P Overlay Network（所有节点都对等）

- 节点之间通过TCP通信

**设计原则：simple, robust, but not efficient.**

消息传播采取flooding此方式 （属于best effort）

越大的区块传播越慢，BTC协议对于区块来说，有1M大小的限制