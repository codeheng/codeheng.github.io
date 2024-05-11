---
comments: true
---

> 分叉：原来的系统中为一条链，但分成了两条链

- 对BTC系统当前状态产生分歧导致的分叉(state fork)
    * 挖矿时两个节点差不多同时挖出矿，均发布区块
    * 分叉攻击，人为造成（deliberate fork）
- 比特币协议改变，在分布式系统中不能保证所有节点同时升级软件，假设存在少数节点未升级，导致出现分叉(protocol fork)

根据对比特币协议修改的不同，将分叉分为 **硬分叉和软分叉**

## 硬分叉(hard fork)

!!! warning "什么情况会出现hard fork?"
    对BTC协议增加新协议，扩展新功能，未升级软件的旧节点会不认可这些修改，认为这些特性是非法的。即对BTC协议内容产生分歧，从而导致分叉。
    
    **硬分叉的例子: 对比特币区块大小的修改**（BTC区块大小限制1MB，但是否合适存在争议？）

    - BTC区块太小了，限制了throughput

>  A hard fork (or hardfork), as it relates to blockchain technology, is a radical change to a network's protocol that makes previously invalid blocks and transactions valid, or vice-versa.

只要这部分旧节点永远不更新软件，之前的链便永远不会消失（这种分叉是永久性的）

出现hard fork后，便变成了两条平行的链，造成了社区分裂。

- 社区中有一部分人，会认为之前的链才是好的，导致各个链上的货币独立
- 各自按照各自的规则进行挖
      * e.g. ETH就发生过hard fork，ETC才是以太坊设计原本的协议
      * ETH是黑客攻击ETH上一个智能合约[THE DAO](https://www.gemini.com/cryptopedia/the-dao-hack-makerdao#section-what-is-a-dao)后进行回滚的协议链
          + 将黑客攻击偷取的以太币采用hard fork回滚回到另一智能合约，退还给真正拥有者
      * 对两条链各添加了一个chainID，使两条链真正分开 

## 软分叉(soft fork)

> 如果对BTC协议添加限制，使得原本合法交易在新交易中不合法，便会形成soft fork

比如：假设区块大小从1MB减小至0.5MB（实际上不可能这样做），并且系统中大多数节点更新了软件，少数节点仍然遵从1MB限制的协议。即：新节点认为区块大小最大0.5MB，旧节点认为区块大小最大1MB，且新节点占据大多数

若旧节点如果不升级软件，挖出的区块可能就白挖了(大于0.5MB)

- 对于系统来说，不会存在永久性分叉


**soft fork**：只要系统中拥有半数以上算力节点更新软件，系统就不会产生永久性分叉

**hard fork**：必须系统中所有节点更新软件，系统才不会产生永久性分叉

一些问题和解答：[参考](https://www.cnblogs.com/coderzjz/p/13788649.html#%E4%B8%80%E4%BA%9B%E9%97%AE%E9%A2%98%E5%8F%8A%E5%85%B6%E8%A7%A3%E7%AD%94)