---
comments: true
---

## V2G(Vehicle to Gird)

### 基本介绍

!!! quote "来自论文"

    刘晓飞,张千帆,崔淑梅.电动汽车V2G技术综述[J].电工技术学报,2012,27(02):121-127.DOI:10.19595/j.cnki.1000-6753.tces.2012.02.018.

随着可再生能源（如太阳能、风能等）接入电力系统，但由于其不连续性导致发电的波动，需要其他能源（如电池能量存储系统）进行补偿，V2G针对上述问题提出的，**其核心思想就是利用大量电动汽车的储能源作为电网和可再生能源的缓冲**

- 当电网负荷过高时，由电动汽车储能源向电网馈电
- 当电网负荷低时，用来存储电网过剩的发电量，避免造成浪费

电动汽车用户可以在电价低时，从电网买电；电网电价高时向电网售电，从而获得一定的收益

电动汽车(不同于电网中其他的电负荷)：具有高度的移动性和不可预测性

**从经济上，V2G的效益是引人注目的(好处)** ： 

1. 利用电动车电池作为电网的缓冲，为电网 **提供辅助服务**，如调峰、无功补偿等
2. 能为车主提供额外的收入，抵消购买电动汽车的部分花费，有利于清洁汽车的普及
3. 可以增加电网稳定性和可靠性，降低电力系统运营成本

**V2G的实现方法(根据应用分为四类) :**

- 集中式、自治式、基于微网的V2G、基于更换电池组的V2G

### V2G安全隐私问题

!!! Quote "来自论文"
    张雄,张海春,刘政林.V2G网络的安全和隐私问题研究[J].汽车实用技术,2021,46(23):12-15.DOI:10.16638/j.cnki.1671-7988.2021.023.004.

V2G系统中存在的安全和隐私方面的挑战将极大地影响下一代汽车接入电网技术的实际应用[^1]
[^1]: N.Saxena,S.Grijalva,V.Chukwuka,et al.Network Security and Privacy Challenges in Smart Vehicle-to-Grid[J].IEEE Wireless Communications,2017,24(4): 88-98.

!!! Note "相关研究"

    Saxena等人[^2]提出了一种新的网络安全和隐私保护架构来支持V2G网络

    Eizaet[^3]等人通过结合 **无证书公钥加密和限制性部分盲签名技术**，提出了一种安全且隐私敏感的代理移动 IPv6协议，既保证了电动汽车的位置隐私，又保持了电动汽车与充电服务之间的会话连续性

    Abdallah 等人[^3]提出了一种针对V2G连接的隐私保护认证方案，**利用哈希函数和位异或操作**，设计了一个健壮的密钥协商协议，在V2G环境中实现了相互认证

    [^2]: N.Saxena,S.Grijalva,V.Chukwuka,et al.Network Security and Privacy Challenges in Smart Vehicle-to-Grid[J].IEEE Wireless Communications,2017,24(4): 88-98
    
    [^3]: M. H. Eiza, Q.Shi, A.Marnerides, et al.Secure and privacy-aware proxy mobile IPv6 protocol for vehicle-to-grid networks[C].IEEE International Conference on Communications (ICC),2016:1-6.

    [^4]: Abdallah A,Shen X.Lightweight Authentication and Privacy- Preserving Scheme for V2G Connections[J].IEEE Transactions on Vehicular Technology,2016:1-1.

## V2V(Vehicle to Vehicle)