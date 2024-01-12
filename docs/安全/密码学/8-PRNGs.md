---
comments: true
--- 

## 介绍

PRNGs = Pseudorandom Number Generators(伪随机数生成器)

之前[留的疑问](7-MACs.md#aead:~:text=%E5%AE%8C%E6%95%B4%E6%80%A7/%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81-,Question,-%E5%AF%B9%E7%A7%B0%E5%AF%86%E9%92%A5): **对称密钥加密方案需要随机性，那么如何安全地生成随机数**?

### 随机性

对于 **对称加密**，随机性是非常重要的!

- 随机的key
- 随机的IV/Nonce
- 通用唯一标识符...等等

如果攻击者可以预测一个随机数，可能会灾难性地失败，那如何安全生成随机数？

在密码学中，随机性意味着 **随机 + 不可预测**

### Entropy(熵)

> Entropy is a scientific concept that is most commonly associated with **a state of disorder**

**熵： 不确定性的度量，即衡量结果的不可预测性**

High entropy = unpredictable outcomes = desirable in cryptography

均匀分布具有最高的熵(每个结果的可能性都相等，如: 公平抛硬币)

- 通常用比特来衡量(3 bits的熵= 8个值的均匀随机分布)

**真正的随机性** ： 

为了生成真正的随机数，需要熵的物理来源

- CPU上不可预知的电路
- 以非常精细的时间尺度测量人类活动(例如按下一个键的微秒)

无偏熵通常需要结合多个熵源

- 熵的总比特数是所有输入的熵比特数的总和
    * Many poor sources + 1 good source = good entropy

^^真正随机性的问题：昂贵并且产生太慢 & 物理熵源通常是有偏差的^^

### PRNGs 介绍

伪随机数生成器(PRNGs):一种使用少量真正随机性来生成大量看上去随机输出的算法

- 也被称为 ^^确定性随机比特生成器^^ deterministic random bit generators(DRBGs)

**用途：**

- 生成一些昂贵的真正随机性(例如CPU上的噪声电路)
- 使用真正的随机性作为PRNG的输入
- 使用PRNG快速而廉价地生成看起来随机的数字

**PRNG是确定性的**：根据一组算法生成输出, 然而对于无法看到内部状态的攻击者来说，在计算上无法将输出与真正的随机性区分开来


## PRNG定义

1. `PRNG.Seed(randomness)`: 使用熵初始化内部状态
      - 输入：一些真正的随机比特 
2. `PRNG.Reseed(randomness)`：使用现有状态和熵更新内部状态
      - 输入：更多真正的随机比特  
3. `PRNG.Generate(n)`：生成n个伪随机比特
      - 输入：一个数字n； 输出：n个伪随机比特
      - 根据需要更新内部状态

**属性：**

- 正确性：是确定的
- 高效性： 产生伪随机比特是效率高的
- 安全性：与随机无法区分
- 附加安全性：防回滚(Rollback resistance)

### Seeding 和 Reseeding

PRNG 应使用所有可用的熵源进行播种(Seed)

- 将许多low-entropy源组合在一起应该会得到high-entropy输出
- 如果一个源的熵为0，它不应该减少输出的熵

Reseeding用于增加更多的熵，因为它变得可用

- 用0熵进行Reseeding不应该减少内部状态或输出

### PRNG: 安全

Q: 能设计一个真正随机的PRNG吗?

PRNG不可能是真正随机的

- 给定初始种子，输出是确定的
- 如果初始种子是s位长，那么只有$2^s$个可能的输出序列

安全的PRNG在计算上从陌生人到攻击者来说是无法区分的

- 比如向攻击者展示一个真正的随机序列和一个从安全PRNG输出的序列，攻击者不应该能够在概率> ^^$1/2$ +可忽略不计的情况下(negligible)^^ 确定哪个是哪个

等价定义：攻击者无法预测PRNG的未来输出

### PRNG: Rollback Resistance