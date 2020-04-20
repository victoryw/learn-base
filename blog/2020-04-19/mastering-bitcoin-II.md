---
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
marp: true
---

# [Mastering Bitcoin](https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch02.asciidoc)

## How Bitcoin Works

---

## 概述
***概览***比特币的工作模式，了解用户、钱包软件、网络、矿工、账本的协作关系来完成一笔交易。

通过模拟资金在用户间的转移这样的案例，来追踪 一笔*比特币的交易* ，
* *用户*间的交易如何被*钱包软件* 建立；
* 交易如何通过*比特币网络* 传播
* 交易是如何通过矿工依据*分布式共识机制* 达成共识，最终被记录（上帐）到*区块链*上
* *钱包软件* 是如何确认交易生效以花费交易的

---

### 比特币名词体系
![bg fit](https://raw.githubusercontent.com/bitcoinbook/bitcoinbook/develop/images/mbc2_0201.png)

---

## 案例
* 用户：Joe, Alice, Bob, and Gopesh
* 交易场景：购买一杯咖啡
* 组成：
  * 支付码
  * 交易
  * 传播和共识
  * 花费
---
### 支付码
A URI scheme for making Bitcoin payments (BIP0021)
![](https://raw.githubusercontent.com/bitcoinbook/bitcoinbook/develop/images/mbc2_0202.png)

---
解析：
```
bitcoin:1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA?
amount=0.015&
label=Bob%27s%20Cafe&
message=Purchase%20at%20Bob%27s%20Cafe
```

```
A bitcoin address: "1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA"
The payment amount: "0.015 BTC"
A label for the recipient address: "Bob's Cafe"
A description for the payment: "Purchase at Bob's Cafe"
```
* 地址：接收方的比特币地址
* 数量：**BTC** 和 Decimal
  
---

[BIP0021](https://en.bitcoin.it/wiki/BIP_0021)
```
bitcoinurn     = "bitcoin:" bitcoinaddress [ "?" bitcoinparams ]
bitcoinaddress = *base58
bitcoinparams  = bitcoinparam [ "&" bitcoinparams ]
bitcoinparam   = [ amountparam / labelparam / messageparam / otherparam / reqparam ]
amountparam    = "amount=" *digit [ "." *digit ]
labelparam     = "label=" *qchar
messageparam   = "message=" *qchar
otherparam     = qchar *qchar [ "=" *qchar ]
reqparam       = "req-" qchar *qchar [ "=" *qchar ]
```
---

### 交易
* 交易的定义、交易的组成元素和关系
* 在用户、钱包软件和比特币网络下，完成一笔交易

**交易是所有权转让，将价值从交易输入转移到交易输出**
A transaction tells the network that **the owner** of some bitcoin **value** has **authorized** the **transfer** of that **value** to **another owner**. 

---

#### 一次价值的转移
![width:200px](https://raw.githubusercontent.com/bitcoinbook/bitcoinbook/develop/images/mbc2_0203.png)
* 一次交易会有多笔输入和输出
* 交易的费用 = 总输入值 - 总输出值
* “消费”（"spending"）就是签署一笔交易

---

#### 所有权、授权，价值 (交易输入、输出的构造，关联)
![width:1000px](https://raw.githubusercontent.com/bitcoinbook/bitcoinbook/develop/images/mbc2_0204.png)
* 输出 （地址、值、类型）
* 输入（上一笔输出的引用：交易、地址 和 值） 
* 交易间的输入和输出的关系构成交易链条, 所有权的转移链条
* 授权：私钥、地址、签名；锁定脚本
* 找零输出 和 费用；找零的价值——而不是一笔输入出现在多笔交易中, UTXO不可变

---

#### 交易形式，同一笔交易里输入和输出的关系
* 普通交易 一个输入 => 两个输出（花费、找零）
* 资金归集 多个输入 => 一个输出（花费）
* 资金分配交易 一个输入 => 多个输出（花费1，花费2）

---

#### 创建交易
解释钱包软件完成交易的行为，
* 构建
* 传播
* 接收

---

###### 构建交易
获得正确的输入
* 交易输入：未花费的交易输出，找到足够支付的资金
* 钱包的不同模式：[full-node](https://en.bitcoin.it/wiki/Full_node) 客户端下的钱包和轻量级客户端
* 轻量级客户端，通过 Api 查询未花费的交易输出（服务商或者其他full-node）

---

创建输出
* 交易输出脚本 creates an encumbrance(留置权) on the value and can only be redeemed(赎回) by the introduction of a solution to the script
  脚本大意是这样的：“这个输出将支付给那个能提供与鲍勃的公开地址相匹配的签名的人。
  由于只有鲍勃拥有与其地址匹配的密钥，所以只有鲍勃的钱包软件可以提供这样一个签名来兑现这笔输出
* 找零输出 和 交易费用
[示例](https://www.blockchain.com/btc/tx/0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2)

<!--
  留置权——输出锁定，给谁、多少 和 解锁条件（提一个问题，和验证答案的方式）
-->
<!--
  赎回——输入，满足解锁条件 （提供自己的答案）
-->

---

###### 传播和接收
* 传播给Bob的钱包软件和网络里的其他客户端
  * 传播通过p2p的比特币网络的转发完成的
* Bob的钱包软件通过直接/间接收到交易，
* Bob可以独立认可现实的交易（？怎么叫做收到）（小额低风险）也可以等待共识后认可现实的交易完成。（这么简单么？）


---

### 共识和花费
一个在网络上传播的交易，直到成为全局分布式账本（区块链）的一部分才算真正得到确认。
1. Alice 的交易从网络流入 景 的临时交易池
2. 景创建了一个新区块
3. 景依据优先原则，从临时交易池提取未被纳入区块的交易
4. 景 提取到了 足够交易费用的 Alice 的交易，打包进新区块
5. 景 赢得了 工作量证明 （计算出答案，并且比其他人优先）
6. 景 发布自己的区块277316到网络

---

7. 其他矿工发布新区块基于277316区块或者基于包含277316的区块 —— 区块高度、区块深度
8. 6次确认（？怎么6次确认）后的区块被认为是不可撤销的（被分叉的可能性很小）
9. 客户端通过确认交易所在的区块是不可撤销的验证交易（Full-Node 和 轻量级的节点都是这样么？）
10. 交易有效且可花费的，Bob 可以继续消费

---

![bg fit](https://raw.githubusercontent.com/bitcoinbook/bitcoinbook/develop/images/mbc2_0201.png)
