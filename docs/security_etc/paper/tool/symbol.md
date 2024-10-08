---
comments: true
---

!!! Abstract 
    记录密码学论文中见到的常用符号


| 名称(符号)        |    含义                     | Latex命令 |
| :----------      | :---------------------------| :--------------------|
| 异或($\oplus$)    | 01串/两数之间进行异或操作     | `\oplus` |
| 连接($\Vert$)        | 顺序连接两个串               | `\Vert` |
| 错误标识null symbol($\bot$) | 未定义的值/空值,代表特定情况下某个值是无意义的| `\bot` |
| 代数($\cdot +$)    | 模乘，模加                 | `\cdot`, `+` |
| 公钥($\mathsf{Pk}$) | 可公开的密钥 | `\mathsf{Pk}` |
| 私钥($\mathsf{Sk}$) | 与公钥对应需秘密保存的私钥 | `\mathsf{Sk}` |
| 密钥生成($\mathsf{Gen}(1^{\lambda})$) | 根据安全参数$\lambda$生成公私钥| `\mathsf{Gen}(1^{\lambda})` |
| 哈希($H(\bullet)$) | 可以接受各种不同的输入并输出相应的哈希值 | `H(\bullet)` |
| 敌手($\mathcal{A}$)| 可证明安全中试图解决攻破方案的假想攻击者	| `\mathcal{A}` |
| 挑战者($\mathcal{C}$) | 可证明安全中试图解决困难问题的挑战者	| `\mathcal{C}` |
| 证明游戏($\mathsf{Game}$) | 为安全性证明定义的挑战者与敌手的交互模型 | `\mathsf{Game}` |
| 可忽略概率($\varepsilon$) |  在安全参数$\varepsilon$的多项式级别下的极小量 | `\varepsilon` |
| $\{0,1\}^*$| 由 0 和 1 组成的任意长度的字符串集合 | `\{0,1\}^*`|
| $Z^*_q$ | 模$q$的所有非零整数的乘法群 | `Z^*_q`|