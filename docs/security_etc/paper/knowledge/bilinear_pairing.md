---
comments: true
---

# 双线性映射(Bilinear Mapping)

## 介绍

> 是一种二元映射，它作为密码学算法的构造工具，在各区块链平台中广泛应用，比如 **零知识证明、聚合签名** 等技术方案大多基于双线性对构造得来

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

      设$q$为素数，$G_1,G_2$是基于椭圆曲线上的阶为素数q的加法循环群，$G_T$是阶为$q$的乘法循环群，如果函数$e: G_1 \times G_2 \to G_T$ 是一个双线性映射，则具有如下性质：**（此时的定义G写成相加的形式）**

      1. 双线性：$\forall P \in G_1,Q \in G_2$且$a,b \in Z^*_q$，$\exists e(aP, bQ) = e(P, Q)^{ab}$
      2. 非退化性：$\exists P,Q \in G_1$, 满足$e(P,Q) \neq 1$
      3. 可计算性: $\forall P,Q \in G_1$, $e(P, Q)$是多项式时间可计算的

## 摘自[CMU](https://people.csail.mit.edu/alinush/6.857-spring-2015/papers/bilinear-maps.pdf)

> Bilinear maps are the tool of *pairing-based* crypto (双线性映射是基于配对的加密工具)

**bilinear maps 主要做什么？**

- 建立密码组之间的联系
- 使得DDH在其中处理过程中变得容易
- Let you solve CDH "once"

!!! Tips "DDH(Decisional Diffie-Hellman) 和 CDH(Computational Diffie-Hellman)"

      DDH: 给定$g, g^a, g^b, g^z$，判定$a * b = z$

      - 没有bilinear map必须试出来a,b才能知道
      - 有了之后，试一下$e(g^a, g^b) = w^{ab}$，再试一下$e(g, g^z) = w^z$
          - 只要$w^{ab} = w^{z}$ 即$ab = z$

      CDH: 给定$g, g^a, g^b$，找出$g^{ab}$

假设有两个循环群(即$G_1, G_2$)，有一个mapping function(即称为$e$),可以找到相对应另一个新的循环群(即$G_t$)，只要输入$G_1,G_2$，它就可以找到相应$G_t$的成员值。e有以下特别好的特性：**（此时G是写成相乘的形式）**

$$
      e(u^a, v^b) = e(u, v)^{ab} = w^{ab}
$$

其中u是$G_1$的成员，v是$G_2$的成员，w是$G_t$的成员


- 实际上找e不太好找，计算上也不容易
- 只考虑那些高效的bilinear map 称为 **admissible bilinear map**
    - 密码学中最好用的mapping，即$G_1 = G_2$
- 目前找出e的方法：Weil Pairing和Tate Pairing

!!! Note "定义"
      如果$e(g_1, g_2)$生成$G_t$,并且$e$是可有效计算的，则e是admissible bilinear map

      - 设$g_1$和$g_2$分别是$G_1$和$G_2$的生成元

      > 生成元是指一个或一组元素，通过它们可以生成整个群、环或其他代数结构中的所有元素

### 三者之间的联系

- $G1,G_2,G_t$都是同构的因为它们有相同的阶并且是循环的
    - 群$G$的阶是指群中元素的总数 
- 它们是不同的群，因为用了不同的方式表示元素和计算操作
- 一般情况$G_1 = G_2$，并用$G$进行表示
- $G$和 $G_t$可以是合数阶或素数阶(大多数是素数阶)
    - 对它们的工作/使用方式有所影响
- 若$G = G_t$称为self-bilinear map