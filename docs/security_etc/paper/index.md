---
comments: true
---

# 介绍

!!! Abstract
    **电气工程期刊找问题，计算机期刊找方法**

    记录读的方向论文 & 论文里面涉及的技术知识 --> 隐私保护，加密，电力交易为主
    
    一个人能走多远，取决于TA知道的起点(经典方案)以及技巧(可探索的范围)


## 背景方向
- V2G & V2V
    - 充放电隐私(charge and discharge)
    - 隐私保护数据发布(Privacy-preserving data publication)
- [车路云](https://www.tsinghua.edu.cn/info/1182/109825.htm)(导师群里分享——"场景描述清楚，容易创新") PS: [视频介绍](https://www.bilibili.com/video/BV1V1421y7n3/?spm_id_from=333.337.search-card.all.click&vd_source=41a19477b1cd284eb33c00c0aae3f725)， [白皮书](https://13115299.s21i.faiusr.com/61/1/ABUIABA9GAAgzKiYngYo_oOy7AY.pdf)
- 能源互联网EI
    - P2P能源交易: energy transaction matching（能源交易匹配）
        - 将买方和卖方的交易需求进行匹配的过程
- **车联网** --> internet of vehicles(VANET)
      -  charging 、bidding、auction
- Intelligent vehicle(IV)、electric vehicle(EV)
- **smart grid**: privacy protection、identity authentication、secure data aggregation、grid billing 

> 基于xxx和 xxx的xx研究 （三个创新点）

智能网联汽车(Intelligent Connected Vehicles,ICVs)/ & V2V : 

- V2V communication安全: 基于区块链的安全V2V通信方案
- 匿名身份认证方案: 具有匿名身份隐私保护的V2V matching方案
      * 声誉评估(POR) 
      * 智能合约SC、基于Game theory的双向拍卖机制
- 区块链网络中改进xxx: 基于雾计算的轻量级数据聚合隐私保护方案
- 用户身份认证(User authentication): 
- 高效支付(Payment) 

**智能网联电动汽车能源交易隐私保护的研究**

- 车辆连接IoEV进行身份认证 --> 安全认证方案（Schnorr）
    * 智能网联电动汽车下匿名认证方案方案
- 安全存储IoEV数据避免未经授权的访问和修改
- 如何交易？ 以及支付来保证隐私安全
    * 基于博弈论和联盟链的能源交易方案
        + 引入区块链，实验怎么做？（MATLAB等仿真软件）
    * V2G/V2V下能源交易及安全支付隐私保护方案

IoEV中实现能源和电力最大合理化 （充放电管理——能量交易）

到底是否需要和电扯上关系？纯研究车联网行？！（要怎么扯上关系？）

**EI中电动汽车能源交易及隐私保护的研究**

- 电动汽车接入能源互联网的隐私保护的研究


## 可能所需知识

!!! Note

    - 加密(encryption)保证消息的机密性
    - 数字签名([digital signature](https://hackmd.io/@PIncentivus/r1xkfbGO3))保证消息的完整性、身份的不可伪造性和操作的不可否认性
    - **数字签密可同时实现两者** （计算和通信效率更高）

- **区块链** + SG --> 联盟链
    - 共识算法：~~PoW、PoS~~、**PoR**、PoA、PBFT
    - DR
    - EVs
    - SC(智能合约)
- ~~群签名和环签名~~(管理成员，开销大) --> 基于ID(存在密钥托管) --> **无证书(CL-PKC)**
    - 聚合签名 --> **无证书聚合签名**(CLAS) --> 无配对无证书聚合签名(PCAS)
          - Pairing-free certificateless aggregate signature 
    - Conditional Privacy-Preserving Certificateless Signature(CPP-CLS)
    - 盲签名(用于认证)：允许请求者获得隐藏消息内容的签名者的消息签名
- Paillier同态加密（部分同态加密）：用于V2G网络数据聚合
- Anonymization(匿名化)： 
- ~~双线性映射~~（计算开销大） --> pairing-free
- **ECC** --> ECDLP
    - ECDSA --> **Schnorr signature**
- 从single到Multiple: 
    - 多个相同对象
        * Multi-Signatures：为了提高存储和通讯效率 --> aggregate signature
        * Signatures with Batch Verification: 为了减少签名验证时间
        * certificateless signatures: 通过捆绑基于ID签名和传统数字签名
    - 多个不同对象：
        * Signcryption(签密)：将加密和签名结合，在一个逻辑步骤内进行实现
        * 异构签密:  允许双方在不改变各自现有安全基础设施的前提下实现安全通信
- The pseudonym mechanism(假名) + ZKP
- ~~差分隐私(DP)、联邦学习(federated learning)~~
- online/offline Computing
    * offline: 某部分输入未知时，完成 **大部分** 计算
    * online：在输入全弄清后，只需 **轻量** 计算

!!! Quote "来自——[郭福春.致我公钥密码研究生的一封信](https://documents.uow.edu.au/~fuchun/jow/001-revisited.pdf)"
    目前从事科研，不代表毕业了你这辈子就一定要跟科研打交道。这一段时间的认真学习，让你学到的是思考的方法(自己发现问题、解决问题)和态度习惯(枯燥但能坚持下来的态度)

    做好自己不喜欢但必须做好的事情。一个好习惯，影响后阶段，你若不坚持，后面怎么办


## 会议

- 三大密码学顶会: [CRYPTO美密](https://www.iacr.org/meetings/crypto/)、[EUROCRYPT欧密会](https://www.iacr.org/meetings/eurocrypt/)、[ASIACRYPT亚密会](https://asiacrypt.iacr.org/)
- 四大安全类会议：[Security & Privacy](https://onlinelibrary.wiley.com/journal/24756725)、[USENIX Security](https://www.usenix.org/conferences)、[CCS](https://dl.acm.org/conference/ccs)、NDSS(Network and Distributed System Security)

导师推荐期刊如下：
    
- [IEEE Transactions on Power Systems](https://ieeexplore.ieee.org/xpl/RecentIssue.jsp?punumber=59)
- [Computer & Industrial Engineering](https://www.sciencedirect.com/journal/computers-and-industrial-engineering)
- [IEEE TRANSACTIONS ON POWER DELIVERY](https://ieeexplore.ieee.org/xpl/RecentIssue.jsp?punumber=61)
- [IEEE Transactions on Smart Grid](https://ieeexplore.ieee.org/xpl/RecentIssue.jsp?punumber=5165411)