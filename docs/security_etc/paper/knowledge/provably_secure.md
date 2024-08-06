---
comments: true
---

> 安全性证明: 方案安全否？是能否用 ^^形式化的数学语言^^ 来证明

证明安全——即说明敌手$\mathcal{A}$无法成功攻击!

- encryption algorithm：敌手恢复不出来明文消息$m$，或密钥$K$
- signature algorithm：敌手伪造不出来签名

!!! Question "如何描述“恢复不出”，“伪造不出”等表述？"

    密码学语义: Indistinguishability(IND) & Unforgeability等定义

    - 基于定义[安全模型](https://www.cnblogs.com/max1z/p/15992505.html)即为最终要证明的结论

    目标：说明该算法在一定安全假设下符合某安全模型的要求
    
    - **假设条件 --> 证明技术  --> 证明结论**

安全性证明——**归约Reduction**

!!! example "例子"

    DDH(Decisional Diffie–Hellman)是困难的 => ElGamal算法是IND-CPA安全的

    - **证逆否**：ElGamal算法不是IND-CPA安全的 => DDH可被攻破

    **密码学中表述**: 若存在一个PPT(Probabilistic Polynomial Time)敌手在CPA模型下攻破了ElGamal算法的Indistinguishability，即敌手能解决DDH问题

> Cite - Introduction to Security Reduction:
> 
> In computational complexity theory, a reduction transforms one problem into another problem, while in public-key cryptography, **a security reduction reduces breaking a proposed scheme into solving a mathematical hard problem.**
