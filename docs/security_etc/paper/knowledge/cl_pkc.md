---
comments: true
---

## 介绍

!!! Note

    **无证书公钥密码体制(certificateless public key cryptography，CL-PKC)** 是在基于身份的公钥密码体制 (identity-based public key cryptography，ID-PKC)的基础上提出来的一种新型公钥密码体制[^1]

    [^1]: 张福泰,孙银霞,张磊,耿曼曼,李素娟.无证书公钥密码体制研究.软件学报,2011,22(6):1316-1332

    **没有密钥托管问题，不需要使用证书**

公钥密码必须解决好一个基本问题——**如何保证用户公钥的真实性和有效性？** 

- 传统的是用证书（public key infrastructure, PKI），即CA(证书中心)为用户签发公钥证书
    - 但管理和维护需要巨大的计算、通信和存储代价

^^84年,Shamir[^2] 提出了基于身份的密码系统^^, 避免了基于 PKI 的传统公钥密码系统中对证书的使用和验证过程。**用户的公钥是公开、可以唯一确定用户身份的信息即ID**，故不需要查找公钥，即无需公钥证书的存在。
[^2]: Shamir A. Identity-Based cryptosystems and signature schemes. In: Blakely GR, Chaum D, eds. Advances in Cryptology-Crypto’84. LNCS 196. Heidelberg: Springer-Verlag, 1985. 47−53.

> 基于身份的密码体系，比如[国密SM9算法](https://mengbin.top/2023-10-18-sm/#:~:text=T%20X.1035%E3%80%82-,SM9,-SM9%20%E5%9B%BD%E5%AF%86)，基于椭圆曲线密码学，即双线性映射

- 但存在私钥托管的问题
    - 由 **可信第三方PKG(private key generation)** 利用它掌握的系统唯一主密钥产生
    - 又称为 **KGC** (key generation center 密钥生成中心)

CL-PKC正是解决上述私钥的问题，由Al-Riyami和Paterson[^3] 于2003年而提出 (称为AP定义)
[^3]: Al-Riyami SS, Paterson KG. Certificateless public key cryptography. In: Laih CS, ed. Proc. of the ASIACRYPT 2003. LNCS 2894, Berlin: Springer-Verlag, 2003. 452−473

**在CL-PKC中，存在KGC，它拥有系统的主密钥**

- KGC 的作用是根据用户的身份和系统主密钥计算 **用户的部分私钥**，并安全地传送给用户
- 用户收到后，用户再使用 ^^自己的部分私钥和自己随机选择的一个秘密值^^ 生成自己 **完整的私钥**
- 而公钥由 ^^自己的秘密值、身份和系统参数^^ 计算得出,并以可靠的方式公布
- 之后,就可以用自己的私钥进行解密和签名。

这样KGC就无法得知任何用户的私钥，故解决了上述问题

!!! warning "补充"

    无证书签密作为研究热点之一，它将 **签名与加密** 相结合，形成无证书签密方案以及无证书聚合签密方案，也有学者将 **[盲签名技术](https://zh.wikipedia.org/wiki/%E7%9B%B2%E7%AD%BE%E5%90%8D)** 和CL-PKC相结合，提出了 ^^无证书盲签名方案、无证书盲签密方案^^ 等

## 无证书加密定义

1. 系统设置算法Setup: 
      - 输入: 安全参数 $1^k$
      - 输出: 系统私钥 $msk$ 和系统公开参数 $params$
2. 提取部分私钥算法Extract-Partial-Private-Key：
      - 输入：$params,msk$ 和用户身份ID
      - 输出：该用户的部分私钥$d_{ID}$
      - (该算法由 KGC 运行,并通过安全信道把 $d_{ID}$ 发送给用户)
3. 用户密钥设置算法Set-User-Key: 
      - 输入：params 和用户身份 ID
      - 输出：秘密值 $x_{ID}$ 和公钥 $pk_{ID}$
4. 加密算法 Encrypt: 
      - 输入：params、接收方身份 ID、公钥 $pk_{ID}$ 和消息 M
      - 输出：密文 C
5. 解密算法 Decrypt:
      - 输入：params、接收方身份 ID、私钥 $sk_{ID}$ 和密文 C
      - 输出：消息 M 


早期的无证书加密方案都是基于 **[双线性映射](./bilinear_pairing.md)**

- 计算代价相对于模指数运算要高出很多

如今更倾向于，比如：**零知识证明、多方计算、同态加密**


## 无证书签名

