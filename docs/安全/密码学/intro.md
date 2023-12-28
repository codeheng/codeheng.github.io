---
comments:
---

## 1. 密码学简史
密码学(cryptography)一词来自拉丁语词根 *crypt* 和 *graphia*，前者意为秘密，后者意为书写。所以密码学就是研究如何写秘密信息。

- 第一阶段的秘密消息方案可追溯到古代 -->  [凯撒密码Caesar cypher](https://zh.wikipedia.org/zh-cn/%E5%87%B1%E6%92%92%E5%AF%86%E7%A2%BC)
- 第二阶段:机械时代  --> [恩尼格玛Enigma密码机](https://zh.wikipedia.org/zh-cn/%E6%81%A9%E5%B0%BC%E6%A0%BC%E7%8E%9B%E5%AF%86%E7%A0%81%E6%9C%BA)
- 现代密码学  --> Claude Shannon即[香农](https://zh.wikipedia.org/wiki/%E5%85%8B%E5%8A%B3%E5%BE%B7%C2%B7%E9%A6%99%E5%86%9C) --> 标准的加密系统[DES](https://zh.m.wikipedia.org/wiki/%E8%B3%87%E6%96%99%E5%8A%A0%E5%AF%86%E6%A8%99%E6%BA%96)
    *  现代密码学涉及大量的数学
## 2. 定义
> 密码学中最基本的问题是确保在不安全的介质上通信的安全性

为了正式研究密码学，需要定义一个数学严谨的框架

**什么是密码学？**

- Older definition：在不安全信道上进行安全通信的研究
- Newer definition: 在攻击者存在的情况下，对数据和计算的安全性提供严格的保证
    * 不仅仅是保密性(confidentiality )，还有完整性(integrity)和真实性(authenticity)    
### 定义Alice, Bob, Eve, and Mallory
- Alice and Bob: 主要角色,试图通过不安全的通信通道互相发送消息
- Eve: 一个窃听者(eavesdropper)，可以读取通过信道发送的任何数据
- Mallory: 一个操纵者，可以读取和修改通过通道发送的任何数据

**目标:** 设计一种方案，可以通过这种方式混淆Alice和Bob之间的消息，以至于Eve不知道交换内容，而Mallory无法在不被发现的情况下篡改交换的内容。即只使用不安全的信道来模拟理想的通信信道

### 定义Keys(密钥)
^^任何密码系统的最基本构建模块是密钥^^。密钥是一个秘密值，帮助我们保护消息。许多密码算法和函数都需要一个密钥作为输入来锁定或解锁一些秘密值

主要的密钥模型：

- **对称密钥** Alice和Bob都有密钥值，必须使用这个共享的密钥值来保护他们的通信
- **非对称密钥** 每个人都有一个密钥和相应的公钥