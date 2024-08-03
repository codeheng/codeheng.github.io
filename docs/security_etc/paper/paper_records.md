---
comments: true
---

2022-6——"An efficient **data aggregation scheme** with local differential privacy in smart grid"(ET1区,CS2区)

!!! Abstract
    SG用户侧有SM，定期采集用电数据给CC，用 **数据聚合** 来对SG供需进行评估，动态调整能源供应和价格

    但会泄露用户隐私，故 **隐私问题是SG数据聚合的关键**

    提出了基于 **随机响应** 的满足 ^^局部差分隐私(LDP)^^ 保护的智能电网数据聚合方案

----

2003——"[Certificateless Public key Crypotography](https://eprint.iacr.org/2003/126.pdf)" 提出CL-PKC对抗模型，两种类型的敌手

- Type I adversary: 是外部攻击者，可以选择替换任意合法的公钥，不持有用户部分私钥
      - 它模拟了攻击者欺骗用户用自己选择的公钥验证签名的能力
- Type II adversary: 模拟恶意的KGC(拥有secret master key),即可以获得任何用户的部分私钥，但不可以替换用户公钥 


-----

2008——"ECPP: Efficient **Conditional Privacy Preservation Protocol** for Secure Vehicular Communications"