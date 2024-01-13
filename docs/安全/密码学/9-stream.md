---
comments: true
---

# Stream Ciphers(流密码)

## 介绍

另一种构造对称密钥加密方案的方法

通过[PRNG](8-PRNGs.md)知道：

- 一个安全的PRNG产生的输出看起来与随机没有区别
- 无法看到内部PRNG状态的攻击者无法了解到任何输出
- **如果使用PRNG的输出作为one-time pad的key呢?**

> 流密码:一种对称加密算法，使用伪随机比特作为one-time pad的密钥