---
comments: true
---

## 量子计算

> Quantum computing is a type of computation whose operations can harness the phenomena of **quantum mechanics(量子力学)**, such as **superposition(叠加)**, **entanglement(纠缠)**, etc

量子计算并不会真正计算，而是利用量子现象，通过量子的行为自然得到结果

传统密码——基于密钥（具体算法是公开的）

- 对称(AES)和非对称(RSA)
      - 目前都是基于数学问题的单向困难性
          - 破解传统密码需要数学
      - 但比如RSA计算机是无法破解，但量子计算机可以
- 如何防范？"以子之矛攻子之盾" --> 量子密码（依赖量子力学，物理学原理）
      - 量子保密通信的专业名称——**量子密钥分发QKD(Quantum key distribution)**
      - 破解量子密码需要物理（不可能通过数学破解）

> 研究保密的科学本身就是被保密的——《码书》（密码学很多进展不会公开）