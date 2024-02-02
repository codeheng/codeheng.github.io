---
comments: true
--- 

# Diffie-Hellman算法

[之前的疑问](7-MACs.md#aead:~:text=%E5%AE%8C%E6%95%B4%E6%80%A7/%E8%BA%AB%E4%BB%BD%E9%AA%8C%E8%AF%81-,Question,-%E5%AF%B9%E7%A7%B0%E5%AF%86%E9%92%A5)：Alice和Bob如何在不安全的通道上共享对称密钥？

==^^Public key exchange (e.g. Diffie-Hellman)^^==，Diffie-Hellman算法解决了 **在双方不直接传递密钥的情况下完成密钥交换**，这个神奇的交换原理完全由数学来进行支撑

**Diffie-Hellman的目标通常是创建一个临时密钥**，临时密钥用于一系列加密和解密，一旦不再需要它就会被丢弃。因此，Diffie-Hellman算法是一种有效的方法，可以让双方在面对窃听者时就随机值达成一致

