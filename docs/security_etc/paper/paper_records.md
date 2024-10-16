---
comments: true
---

2022-6——"An efficient **data aggregation scheme** with local differential privacy in smart grid"(ET1区,CS2区)

!!! Abstract
    SG用户侧有SM，定期采集用电数据给CC，用 **数据聚合** 来对SG供需进行评估，动态调整能源供应和价格

    但会泄露用户隐私，故 **隐私问题是SG数据聚合的关键**

    提出了基于 **随机响应** 的满足 ^^局部差分隐私(LDP)^^ 保护的智能电网数据聚合方案

----

2003——"[Certificateless Public key Crypotography](https://eprint.iacr.org/2003/126.pdf)" 提出CL-PKC对抗模型，两种类型的敌手

- Type I adversary: 是外部攻击者，可以选择替换任意合法的公钥，不持有用户部分私钥
      - 它模拟了攻击者欺骗用户用自己选择的公钥验证签名的能力
- Type II adversary: 模拟恶意的KGC(拥有secret master key),即可以获得任何用户的部分私钥，但不可以替换用户公钥 


-----

2008——"ECPP: Efficient **Conditional Privacy Preservation Protocol** for Secure Vehicular Communications"



-----

2024年9月10日09:15:28

2022——Electricity trade strategy of regional electric vehicle coalitions based on blockchain （工程技术3区）

### 背景

- 能源危机 + 环境保护
    * 利用EVs，如何设计灵活有效的充放电协调机制，为用户和电网谋利？
- EVs续航不够 & 公共充电资源紧张
- 在高峰负荷期间处于里程焦虑状态的电动汽车将选择无视电价充电。这种充电行为不仅增加了电动汽车用户的充电成本，而且对电网的峰谷负荷也有一定的影响 --> V2V新型充电模式

> 如何组织电动汽车进行V2V交易，以满足驾驶需求，提高电动汽车的经济效益，同时减轻电网负担，是一个值得探索的课题

V2V电力交易需要车联网（IoV --> IoEV）进行通信

- from: Y.A.N.G. Liuqing Research on the application of blockchain technology in Internet of Vehicles[J] China New Telecommunications, 20 (06) (2018), p. 110

车联网背景下，车辆将不再是孤立的单元，而是活跃的网络节点

Q: 分布式EVs网络，如何保证用户信息安全？

### 本文贡献

- 城市地区实现V2V电力交易，缓解充电设备不足的问题
- 联盟链引入V2V，并交易响应共识机制（PoS）
- 基于信誉值的激励方案（根据交易任务数 & 用户评价指标，确定挖矿激励份额分配）

> PS: V2V电力交易仿真之前，需要建立区域交通系统模型，计算电动汽车到目的地的行驶距离，确定电动汽车的电池消耗量

**MATLAB仿真**

----



> Iqbal, A., Rajasekaran, A. S., Nikhil, G. S., & Azees, M. (2021). A secure and decentralized blockchain based EV energy trading model using smart contract in V2G network. IEEE Access, 9, 75761-75777.

“最初，所有EV和聚合器在可信机构（TA）处注册，然后TA将为EV和聚合器生成公钥和私钥。当电动汽车为了充电/放电的目的来到充电点时，聚合器和电动汽车在交易开始之前以匿名方式相互认证。只有在认证成功完成的情况下，才允许授权车辆参与交易，并且授权的EV由状态值“1”指示，而未授权的EV由“0”指示。 在交易开始时，充电电动汽车会将其需求数据发送到充电站的智能电表或聚合器。此外，聚合器将接收关于充电EV的电力请求和交易时间的细节。然后，聚合器将充电EV需求数据广播到已经到达充电站的放电EV。此外，放电车辆将其功率容量和成本发送给聚合器。聚合器将匹配充电和放电电动汽车的目的是交换电力和金额通过使用相反的拍卖机制。每轮聚合器通过比较放电电动汽车的报价来匹配一个充电电动汽车和一个放电电动汽车，然后将放电电动汽车的电荷首先转移到充电站并将电荷暂时存储在充电站，然后将电荷从充电站转移到匹配的充电电动汽车。 因此，充电和放电电动汽车将在聚合器的帮助下完成能量交易。”