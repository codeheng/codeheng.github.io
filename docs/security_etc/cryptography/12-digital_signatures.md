---
comments: true
---

## 定义

非对称加密技术很好，因为不需要去共享密钥; **数字签名是提供数据完整性/真实性的非对称方式**

^^PS: 先假设 Alice 和 Bob 可以在不受 Mallory 干扰的情况下传送公钥^^

**公钥签名:** 只有私钥的所有者才能用私钥对消息进行签名, 而每个人都可以用公钥验证签名

包含三个部分: 

- *$KeyGen() \rightarrow  PK,SK$* : 生成公钥/私钥对,其中PK是验证(公钥)密钥,SK是签名(秘密)密钥
- *$Sign(SK, M) \rightarrow sig$* : 使用签名密钥SK, 对消息 M 进行签名以产生签名sig
- *$Verify(PK, M, sig) \rightarrow {0, 1}$* : 使用PK验证消息 M 上的签名sig;若有效输出1,否则输出0

属性:
  
  - 正确性：对于任何消息生成的签名，验证都应该成功
      - 对于所有$PK,SK \leftarrow KeyGen()$和$M$，验证$(PK, M, Sign(SK, M)) = 1$
      - 效率：签名/验证应该很快
      - 安全性：[EU-CPA](./7-MACs.md/#eu-cpa)，与 MAC 相同

在实践中, 如果想要签名一个消息M, 首先需要对M进行hash, 接着签名H(M)

- 为什么数字签名使用hash? --> 允许签署任意长的消息
- 数字签名为 M 提供完整性和真实性
    - 数字签名作为私钥持有者签署H(M)的证明，因此就知道M是私钥持有者所认可的

## RSA签名

关于[RSA加密](./11-Public-key_Cryptography.md/#rsa): $M^{ed} \equiv M \bmod N$, 先使用e或先使用d没有什么特别的

- 如果使用 d 加密，那么任何人都可以使用 e 解密
    - 给定 $x$ 和 $x^d \bmod N$，由于离散对数问题无法恢复d，因此 d 是安全的

!!! Note
    关于$A \equiv B \bmod N$，表示 $A$ 和 $B$ 在模 $N$ 下是等价的

    具体地说，$A \equiv B \bmod N$ 意味着 $A$ 与 $B$ 相除的余数相同，或者说它们对 $N$ 取模后得到相同的结果。数学上可以表示为 $N$ 整除 $(A - B)$，即 $(A - B)$ 是 $N$ 的倍数。

定义: (与RSA加密相同，但使用私钥对哈希进行加密)

- *KeyGen()*: 与RSA加密相同; 公钥是N和e, 而私钥为d
- *Sign(d, M):*  计算$H(M)^d \bmod N$
- *Verify(e, N, M, sig)* : 验证$H(M) \equiv sig^e \bmod N$


另外还有[DSA](https://docs.google.com/presentation/d/1m_sq-fhcGo-jOrjRh25UXyU1UE0f7snaZfFajMoeQPI/edit#slide=id.g11468c29934_0_270): A signature scheme based on Diffie-Hellman
