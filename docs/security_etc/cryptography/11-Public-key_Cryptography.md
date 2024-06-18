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

Diffie-Hellman密钥交换非常棒: 它让Alice和Bob通过一个不安全的通道分享秘密

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
      - Bob推出the (inverse) shared secret并且用密文与之相乘

### 安全性

Diffie-Hellman: 给出$g^a \bmod p$和$g^b \bmod p$很难恢复$g^{ab} \bmod p$

- 而EIGamal 传送这些值在通过不安全的信道
    - Bob的公钥$B$ & 密文$R, M \times B^r \bmod p$
- Eve不能推出$g^{br}$, 故她并不可以恢复出M

### 问题

Q: EIGamal 加密是IND-CPA安全?

- No, 对手可以发送$M_0 = 0, M_1 \neq 0$
    - 需要额外的填充和其他修改以使其在语义上安全    
- 可延展性：对手可以篡改消息
    - 对手可以操纵$C_1' = C_1, C_2' = 2 \times C_2 = 2 \times M \times g^{br}$使其看上起$2 \times M$被加密

## RSA加密

### 定义

*KeyGen()*: 

- 随机选择两个大素数$p,q$
    - 高效算法：选择随机数，然后使用测试来查看该数字是否为素数
- 计算$N = pq$
    - N长度通常是在2048bits和4096bits区间内
- 选择$e$
    - 要求: e与$(p-1)(q-1)$ 互质 & $2 < e < (p-1)(q-1)$
- 计算$d = e^{-1} mod (p-1)(q-1)$
    - 计算乘法逆的有效算法：扩展欧几里得算法
- **公钥N和e, 私钥d**

*Enc(e, N, M)*: 假设$M < N$, 输出$M^e \bmod N$

*Dec(d, C)* : 输出$C^d \bmod N$

!!! Note
    对于正确性需要满足: $Dec(d, Enc(e, N, M)) = (M^e)^d = M \bmod N$

### 正确性

**中国剩余定理(CRT)**: 要检查$x=y \bmod pq$成立?, 可以看$x=y \bmod p$和$x=y \bmod q$是否成立

可以从key generation得知$N = pq$, $d = e^{-1} \bmod (p-1)(q-1)$, 因此$ed = 1 \bmod (p-1)(q-1)$, 即$ed-1$是$(p-1)(q-1)$的倍数

- 正确性的目标: $M^{ed} = M \bmod N$, 使用N的定义即$M^{ed} = M \bmod pq$
- 通过CRT, 可以看$M^{ed} = M \bmod p$和$M^{ed} = M \bmod q$是否成立

由d和e在key generation的定义得知$ed - 1 = (p-1)(q-1)$, 由[费马小定理](https://baike.baidu.com/item/%E8%B4%B9%E9%A9%AC%E5%B0%8F%E5%AE%9A%E7%90%86/4776158)(FLT)得知: 对于任何的素数p, $a^{p-1} = 1 \bmod p$  --> 目标: $M^{ed} = M \bmod p$

- 情况1: M是p的倍数 --> $M = 0 \bmod p$,则$M^{ed} = 0 \bmod p$, 因为乘了M(p的倍数)很多次
- 情况2: M不是p的倍数

$$
\begin{aligned}
    M^{ed} &= M^{ed - 1} M  \qquad (\bmod p) \\ 
           &= M^{some\ multiple\ of\ p-1} M \qquad (\bmod p) \qquad (using\ definition\ of\ ed - 1) \\ 
           &= M \qquad (\bmod p) \qquad \qquad \qquad \qquad \qquad (using\ FLT)
\end{aligned}
$$

PS: 对于q也是如此,同理 (证:$M^{ed} = M \bmod q$)

### 安全性

RSA: 给定N和$C=M^e \bmod N$, 很难找到M; 

- 如果能因式分解N，就能知道 p 和 q
- 之后就可以计算 $d = e^{-1} \bmod (p - 1)(q - 1)$ 并使用 $C^d \bmod N$ 进行解密
- **目前最好的解决方案是分解N，但不知道是否有更简单的方法**
    - 若RSA问题与因式分解问题一样困难，那么只要因式分解很难，则该方案就是安全的
    - 因式分解问题被认为是困难的。最著名的算法是指数时间的

### 问题
RSA加密是IND-CPA安全吗？--> no, 它是确定性的. 任何时候都没有使用随机性！

- 发送使用不同公钥加密的相同消息也会泄露信息
- [Side channel](https://csrc.nist.gov/glossary/term/side_channel_attack#:~:text=Definitions%3A,and%20electromagnetic%20and%20acoustic%20emissions.): 不好的实现也会泄露信息
    - 解密消息所需的时间取决于消息和私钥
    - 此攻击已成功用于破解 OpenSSL 中的 RSA 
- 结果：需要一个概率填充方案

## [OAEP](https://docs.google.com/presentation/d/1m_sq-fhcGo-jOrjRh25UXyU1UE0f7snaZfFajMoeQPI/edit#slide=id.g11468c29934_0_119)

> 最优非对称加密填充(Optimal asymmetric encryption padding)：引入随机性的 RSA 变体

- 与对称加密中使用的"padding"并不同，用于添加随机性而不是虚拟字节

**Idea: RSA只能加密"看起来随机"的数字，所以用随机密钥加密消息**

**公钥加密的问题**

- 由于模运算符，我们只能加密小消息 & 数学很多，计算机计算速度很慢
- 故非对称不适用于大消息

> 混合加密：用对称加密在随机生成的密钥 K 下对数据进行加密，并用非对称加密对 K 进行加密
> 
> - 好处：可以使用对称加密快速加密大量数据，并且仍然拥有非对称加密的安全性
> - 几乎所有密码系统都使用混合加密