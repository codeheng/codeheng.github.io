---
comments: true
---


# Introduction

!!! Abstract

    来自[CS 161](https://fa23.cs161.org/), 对课件以及相关资料进行整理

    课程主要包含: **Memory Safety, Cryptography, Web Security, Network Security**

## 什么是安全?

- 在攻击者存在的情况下强制执行期望的属性
    - 数据保密性、用户隐私、数据和计算完整性、真实性、可用性...

## 安全原则

- 了解你的威胁模型
- 考虑人为的因素
- 安全就是经济
- 如果不能阻止，请检测一下！
- 深度防御
- 最小的权限
- 责任分离
- 确保完全调解
- 不要依赖于晦涩的安全性
- 使用故障安全默认值
- 从一开始就设计安全

### Know Your Threat Model

> Threat Model: 一个关于攻击者是谁以及他们拥有什么资源的模型

一切都归结于人: 攻击者  --> 没有攻击者 = 没有问题

为何攻击系统？ --> 钱、 政治、报复、乐趣(Watching the world burn)

### Consider Human Factors

> 关键思想是，安全系统必须可供普通人使用，因此必须在设计时考虑到人类将扮演的角色

### Security is Economics

没有一个系统是100%的安全对抗所有的攻击;系统只需要针对特定级别的攻击进行保护。

- 更高的安全性需要更多的资金来实现，故 **防御的预期收益应该与攻击的预期成本成正比**

### Detect If You Can't Prevent

- 威慑(Deterrence): 在攻击发生之前阻止它
- 预防(Prevention): 当攻击发生时停止攻击
- 侦测(Detection) :得知有攻击(在攻击发生后)
- 回应(Response)  :对攻击采取措施(在攻击发生后)

e.g: 勒索软件  --> 保持异地备份, 如果电脑和房子着火，也没什么大不了的

### Defense in Depth

多种类型的防御应该分层在一起, 故攻击者必须突破所有防御才能成功攻击系统

然而 ^^security is economics^^ ，防御不是免费的

### Least Privilege

> 考虑一个实体或程序需要什么样的权限才能正确地完成它的工作

如果授予不必要的权限，恶意程序或黑客程序可能会使用这些权限来对付

### Separation of Responsibility

> 如果需要拥有特权，请考虑要求多方共同工作来行使它

### Ensure Complete Mediation

确保每个接入点都受到监控和保护