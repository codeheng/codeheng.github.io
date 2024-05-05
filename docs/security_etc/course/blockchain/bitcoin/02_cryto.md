---
comments: true
---

> 比特币BTC 称为加密货币即Crypto-currency，但实际上区块链上所有交易内容都是公开的

- 主要用到密码学中的 **哈希和签名**

### cryptographic hash function 

**性质1：** collision resistance即若知x，没有什么高效的方法找到y，使得$H(x) = H(y)$ 

- 用途：可用来检测m是否被篡改
- hash函数是否具有此性质，没有理论的证明，一般来自实践
    - MD5之前很安全，但现在就可以人为制造碰撞

**性质2**：hiding即计算过程是单向，不可逆的（$x \to H(x), H(x) \nrightarrow x$）

- 输入要足够大，避免暴力求解，且分布均匀

两个性质结合可实现 digital commitment(digital equivalent of a sealed envelope)即数字版的密封信封，实际为保证随机，会在后面拼接上随机数来取hash，即$H(x \parallel nonce)$

**性质3** ：puzzle friendly 哈希值的计算事先是不可预测的（哈希值无法猜测，只能暴力尝试）

- 这不就是 **挖矿** ：把block header + nonce进行hash，使得其结果小于等于target
    - nonce这个域可以改变，挖矿就是不停寻找nonce，使最终满足条件
- 故这个过程可以作为PoW = Proof of Work **『挖到矿即意味你做了大量工作』**
- 虽然挖矿很难，但验证很简单 （difficult to solve, but easy to verify）

**BTC中用到的哈希函数为SHA-256(Secure Hash Algorithm)**

## 签名

BTC中的账户管理：去中心化，自己决定开户

- 创建 **公私钥对(public key, private key)** 这种则代表一个账户
    - 来自非对称加密体系(asymmetric encryption algorithm)
- 公钥相当于银行帐号，别人知道公钥就可以跟你转账(通信)
- 私钥相当于账户密码，知道后就可以把账户上的钱转走

Q: BTC其实任何信息都是公开的，那公钥私钥是用做什么 -->  **签名**

- 我要发布BTC，则用我的私钥进行签名，别人再用我的公钥进行验证其正确性

生成公私钥必然是随机的，由于256bit，几乎不可能两个人生成一样的来进行窃取

- **前提：有一个好的随机元（a good source of randomness）**
- 同样每一次签名也需要好的随机元，否则就可能泄露私钥

**总：一般是对m取hash，然后对hash进行签名**