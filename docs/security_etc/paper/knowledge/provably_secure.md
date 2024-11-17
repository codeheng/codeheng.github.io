---
comments: true
---
# Provably security


> 任何加密方案在实际部署之前必须在理论上证明是安全的,
> 
> 可证明安全性已经成为密码协议的基本要求


## 安全性证明介绍

> 安全性证明: 方案安全否？是能否用 ^^形式化的数学语言^^ 来证明

证明安全——即说明敌手$\mathcal{A}$无法成功攻击!

- encryption algorithm：敌手恢复不出来明文消息$m$，或密钥$K$
- signature algorithm：敌手伪造不出来签名

!!! Question "如何描述“恢复不出”，“伪造不出”等表述？"

    密码学语义: Indistinguishability(IND) & Unforgeability等定义

    - 基于定义[安全模型](https://www.cnblogs.com/max1z/p/15992505.html)即为最终要证明的结论

    目标：说明该算法在一定安全假设下符合某安全模型的要求

    - **假设条件 --> 证明技术  --> 证明结论**
    - if 条件A满足,B满足,C满足...，Then 方案在X安全模型下具有安全性

```py
可证明安全 -> 敌人攻破不了方案 -> 敌人攻击方案 = 敌人在解决某个计算问题
-> 敌人能攻破方案 = 敌人能解决这个计算问题，但实际上不可能
-> 若敌人能解决这个问题，就能解决另外一个被公认的计算困难问题
```

**计算困难问题：**

- Computational Problem, 答案空间大
- Decisional Problem, 答案空间T/F

### 安全归约

> Cite - [Introduction to Security Reduction](https://link.springer.com/book/10.1007/978-3-319-93049-7):
>
> &nbsp; &nbsp; &nbsp; &nbsp; In computational complexity theory, a reduction transforms one problem into another problem, while in public-key cryptography, **a security reduction reduces breaking a proposed scheme into solving a mathematical hard problem.**

!!! example "例子"

    DDH(Decisional Diffie–Hellman)是困难的 => ElGamal算法是IND-CPA安全的

    - **证逆否**：ElGamal算法不是IND-CPA安全的 => DDs可被攻破

    **密码学中表述**: 若存在一个PPT(Probabilistic Polynomial Time)敌手在CPA模型下攻破了ElGamal算法的Indistinguishability，即敌手能解决DDH问题

密码学中，方案的证明主要采用Security Reduction

**tight reduction（紧归约）**：若敌人能够以实际T以及概率(优势)$\varepsilon$攻克方案，则我们也能以接近T的时间以及接近$\varepsilon$的概率(优势)解决计算难题

## 模型(Model)

> 即用数学概念和数学语言严谨刻画某一个对象。通过模型，可以把研究对象转为严谨数学题
>
> 有两种广泛使用的安全模型： **ROM(Random Oracle Model) 和 SM(Standard Model)**

**计算模型(Computational Model)**——来自计算复杂度理论

  * 对计算能力建模 --> **目的：给敌手划定范围**
  * We say it is secure if no PPT adversary can break it.
    * PPT: Probabilistic Polynomial Time

**安全模型([Security Model](https://documents.uow.edu.au/~fuchun/book/Lecture1-PS.pdf))**——来自密码学理论

  * ^^敌人的在攻击前知道什么，目标是什么？^^
  * 把攻击一个方案转为求解一个计算问题
  * *表达形式：*
    - 游戏$\mathsf{Game}$方式：challenger($\mathcal{C}$)和adversary($\mathcal{A}$)交互
    - 概率Probability方式：$P[\text{知道的内容}:\text{攻击的目标}]$
        + The **challenger** is the secret key owner of a cryptosystem.
        + The **adversary** is trying to break the cryptosystem. 

安全模型分为四步：

  * Setup: challenger和adversary 初始化
  * Capabilities: Describe what and when the adversary queries.
  * Security Goal: 对于adversary赢得game的条件
  * Advantage: Define a parameter satisfying:
      + if the parameter is **negligible**, the cryptosystem is secure
      + Otherwise, the parameter is **non-negligible** and it is insecure.


安全模型定义(算法层面)：

- 密钥算法: $\mathrm{KenGen(1^k)} \to (pk, sk)$
- 签名算法：$\mathrm{Sign}(sk, m) \to S_m$
- 验证算法：$\mathrm{Verify}(pk, m, S_m) \to T/F$

### ROM

- 敌人和哈希函数相关的计算被限制了
- 敌人不能自己私下通过哈希函数计算得到$H(x)$
- 若敌人想知道必须想证明者询问，并由我们决定并返回$y = H(x)$的值给敌人（y随机）
- 不仅要知道敌人攻击用到的哪些x，而且也能利用"由我们决定的y"完成安全归约证明

> 通过ROM完成安全归约证明时，我们不仅知道敌人询问的x，而且也能自己设定$y = H(x)$(即由我们决定y的值)

此外还有如GGM(generic group model)、AGM(algebraic group model)、CRS(common-reference string model)等模型

!!! warning
    能在这些证明模型下完成安全证明的密码方案比缺乏安全证明的密码方案更靠谱

    但在ROM中具有安全证明的加密方案在部署在现实环境中时可能不安全[^1]

    [^1]: C.Ran, G.Oded and H.Shai, "The random oracle methodology revisited", J. ACM, vol. 51, no. 4, pp. 557-594, 2004.

### SM

标准模型中，对手直接计算hash function，更接近现实情况

- 询问和攻击遵循security model
- 指定时间内， 以定义的概率优势


