---
comments: true
---

!!! quote "介绍"

    This lab requires students to **implement simple logical and arithmetic functions**, but using a highly restricted subset of C. For example, they must compute the absolute value of a number using only bit-level operations. This lab helps students understand the bit-level representations of C data types and the bit-level behavior of the operations on data.

## 1. bitXor

> **x ^ y using only ~ and &**  即利用非和与实现异或. e.g. bitXor(4, 5) = 1

涉及到数学的运算：

$$
\begin{align}
    x \wedge y &= \bar{x}y + x\bar{y} \\
    &= (x + y)(\bar{x} + \bar{y}) \\
    &= (x + y) \overline{xy} \\
    &= x\overline{xy} + y\overline{{xy}} \\
    &= \overline{\overline{x\overline{xy} + y\overline{xy}}} \\
    &= \overline{\overline{x\overline{xy}} \overline{y\overline{xy}}}
\end{align}
$$

??? success "实现"

    ```c
    int bitXor(int x, int y) {
        return ~( ~(x & ~(x & y)) & (~(y & ~(x & y))) );
    }
    ```

## 2. tmin

> return minimum two's complement integer（最小二进制补码整数）

即$-2^{32}$，利用移位——即1左移31位

??? success "实现"

    ```c
    int tmin(void) {
        return 1 << 31;
    }
    ```

## 3. isTmax

## 4. allOddBits

## 5. negate

## 6. isAsciiDigit

## 7. conditional

## 8. isLessOrEqual

## 9. logicalNeg

## 10. howManyBits

## 11. floatScale2

## 12. floatFloat2Int

## 13. floatPower2
