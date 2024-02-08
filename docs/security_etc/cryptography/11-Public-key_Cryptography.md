---
comments: true
---

## 介绍

在公钥方案中，每个人有两个密钥: ^^公钥(每个人都知道)和私钥(只有某个人才知道)^^

- 密钥是成对的,即每个公钥对应一个私钥
    - 每个人都可以用公钥加密
    - 只有接收方可以使用私钥解密
- 运用数论(e.g. 模运算，因式分解，离散对数问题),而对称加密使用XOR和位移位
- 信息是数字,对比对称密钥加密(消息是位字符串)

**好处**: 不再需要假设Alice和Bob已经共享了一个秘密
  
**缺点**: 比对称密钥加密慢得多(因数论计算比XOR和位移位要慢)

## 公钥加密: 定义

- *KeyGen() --> PK,SK* : 生成公钥/私钥对，其中PK为公钥，SK为私钥
- *Enc(PK, M) --> C* : 使用公钥PK对明文M进行加密生成密文C
- *Dec(SK, C) --> M* : 使用私钥SK解密密文C

正确性: 解密密文得到最初加密的消息

- Dec(SK, Enc(PK, M)) = M,对于所有PK,SK <-- KeyGen()和M

## ElGamal加密

Diffie-Hellman密钥交换非常棒: 它让Alice和Bob通过一个不security_etc的通道分享秘密

- 问题: Diffie-Hellman本身不能发送消息, $g^{ab} \bmod p$ 是随机的

**Idea : 修改Diffie-Hellman，使其支持直接加密和解密消息**

### Protocol 

- *Keygen()* : 
    - Bob生成私钥$b$和公钥$B = g^b \bmod p$
        - Bob正在完成他那一半的Diffie-Hellman交换
- *Enc(B, M)* : 
    - Alice生成一个随机$r$并计算$R = g^r \bmod p$
        - Alice正在完成她那一半的Diffie-Hellman交换
    - Alice计算$M × B^r \bmod p$
    - Alice发送$C_1 = R, C_2 = M × B^r \bmod p$
- *Dec(b, $C_1$, $C_2$*)
    - Bob计算$C_2 × C_1^{-b} = M × B^r × R^{-b} = M × g^{br} × g^{-br} = M \bmod p$