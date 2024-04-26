---
comments: true
---
> 双线性对是一种二元映射，它作为密码学算法的构造工具，在各区块链平台中广泛应用，比如 **零知识证明、聚合签名** 等技术方案大多基于双线性对构造得来

!!! Note "双线性"
      什么是线性？ 即$f(x + y) = f(x) + f(y)$ (函数的输入相加等于分别计算函数再相加)

      那双线性？**让函数接收两个输入，而且对于每一个输入，都保持上面的线性特征**, 如下所示：

$$
\begin{aligned}
      f(x + y, z) &= f(x, z) + f(y, z) \\
      f(x, y + z) &= f(x, y) + f(y, z)
\end{aligned}
$$

进一步即$f(u + x, y + z) = f(u, y) + f(u, z) + f(x, y) + f(x, z)$

!!! Note "双线性映射完整定义（Bilinear Map）"

      设$q$为素数，$G_1,G_2$是基于椭圆曲线上的阶为素数q的加法循环群，$G_T$是阶为$q$的乘法循环群，如果函数$e: G_1 \times G_2 \rightarrow G_T$ 是一个双线性映射，则具有如下性质：

      1. 双线性：$\forall P \in G_1,Q \in G_2$且$a,b \in Z^*_q$，$\exists e(aP, bQ) = e(P, Q)^{ab}$
      2. 非退化性：$\exists P,Q \in G_1$, 满足$e(P,Q) \neq 1$
      3. 可计算性: $\forall P,Q \in G_1$, $e(P, Q)$是多项式时间可计算的