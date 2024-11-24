---
comments: true
---


## 背景

> Y.A.N.G. Liuqing Research on the application of blockchain technology in Internet of Vehicles[J] China New Telecommunications, 20 (06) (2018), p. 110 

V2V电力交易需要车联网进行通信

-----

> Iqbal, A., Rajasekaran, A. S., Nikhil, G. S., & Azees, M. (2021). A secure and decentralized blockchain based EV energy trading model using smart contract in V2G network. IEEE Access, 9, 75761-75777.

**充电和放电EV将在聚合器的帮助下完成能量交易** : 

- 所有EV和聚合器在可信机构(TA)注册，TA将为EV和聚合器生成公钥和私钥。当电动汽车为了充电/放电的目的来到充电点时，聚合器和电动汽车在交易开始之前以匿名方式相互认证。**只有在认证成功完成的情况下，才允许授权车辆参与交易**，并且授权的EV由状态值“1”指示，而未授权为0

- 交易开始，充电EV会将其 **^^需求数据^^** 发送到充电站的SM或聚合器。聚合器将接收关于充电EV的电力请求和交易时间的细节，将充电EV需求数据广播到已经到达充电站的放电EV 

- 聚合器将匹配充电和放电EV的目的是交换电力和金额,通过使用反向拍卖机制,每轮聚合器通过比较放电EV的报价来匹配一个充电EV和一个放电EV，然后将放电EV的电荷首先转移到充电站并将电荷暂时存储在充电站，然后将电荷从充电站转移到匹配的充电EV。

----


## 聚合 

~~2022-6——"An efficient **data aggregation scheme** with local differential privacy in smart grid"(ET1区,CS2区)~~

!!! Abstract
    SG用户侧有SM，定期采集用电数据给CC，用 **数据聚合** 来对SG供需进行评估，动态调整能源供应和价格

    但会泄露用户隐私，故 **隐私问题是SG数据聚合的关键**

    提出了基于 **随机响应** 的满足 ^^局部差分隐私(LDP)^^ 保护的智能电网数据聚合方案

----

车联网环境下无证书匿名认证方案(2022.电子与信息学报)

!!! Abstract 

    - KGC生成车辆的部分私钥，车辆联合部分私钥和自己的秘密值共同产生实际私钥
    - **RTA(Regional Trusted Authority)** 为车辆生成一个长期的伪身份用于后续的部分密钥生成，而在和RSU通信时车辆自身生成一个短期的伪身份 (**长-短期伪身份** 的联合使用保证了强匿名性和签名的新鲜性，并且避免了RSU生成临时伪身份造成的身份泄露和时延问题)
        * 当有恶意事件发生时，RTA可以通过长-短期伪身份追踪车辆真实身份，并由TA撤销该用户 