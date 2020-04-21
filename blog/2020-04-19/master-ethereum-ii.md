---
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('https://marp.app/assets/hero-background.jpg')
marp: true
---
# [Master Etherenum](https://github.com/ethereumbook/ethereumbook/blob/develop/02intro.asciidoc)
## Ethereum Basics

---
## 概述
应用以太坊：
* 使用钱包来完成创建交易
* 运行简单的智能合约

---
## 使用钱包
### 货币单位
* 以太币 以太坊的数字货币
* 以太币的货币单位 ETH
* 以太币的面额单位 wei等
* 单位间的换算机制 1 ether = 10 ^ 18 wei

---

### 以太坊的钱包
钱包是帮助你管理以太坊账户的软件应用程序
* 保存你的私钥
* 创建以太坊交易数据包
* 以你的身份(通过秘钥)在网络上广播
* 私钥和以太坊地址是1对1的关系
  
钱包的可靠性  
私钥的保存和防窃取、资产的安全转移‘
  
---  
### MetaMask
钱包示例
#### 安装
* 以Chrome 扩展方式安装
* 验明钱包软件真身
#### 创建钱包
* 密码是 MetaMask钱包的密码，不是账户的密码
* 创建了一个外部账户，助记词可以找回密码

----

#### 选择区块链网络
合约和资产是基于区块链网络的
* 主网络、测试网络、本地网络（localhost:8545, 作为私有网络）和其他
* 在不同网络下使用同样的秘钥和以太币地址，合约和资产是独立的
* 一个钱包可以连接到不同网络

---

### 使用钱包创建交易
在 Ropsten Test Network faucet 应用
### 通过交易来获得/发送以太币
* 提供你的以太币地址来创建交易
* 完成交易，发布到区块链网络
* 打包交易到区块链
* 21000 gas 作为手续费
* 1 gas 和 ETH 的换算关系

---

## 创建/使用合约
## 外部账户与合约账户
* 外部账户有钱包创建，拥有私钥
* 合约账户具有智能合约代码，没有私钥（无法启动交易，但可以调用其他合约）

* 合约也有地址，可以发送和接收以太币
* **当交易目标是合约地址时，合约会在EVM中运行**
---

### 创建合约
合约地址
合约的使用：通过交易的方式触发--创建到特定函数的内部交易 和 确认交易
工具：Remix、Solid
* 合约的撰写
* 合约编译为 EVM的二进制代码
* 注册合约到区块链网络（选择网络，关联账户，GAS，提交交易来注册合约）
* 合约账户的地址
---

#### 撰写合约：Faucet 
```
// Our first contract is a faucet
contract Faucet {
  // Give out ether to anyone who asks
  function withdraw(uint withdraw_amount) public {
    // Limit withdraw amount
    require(withdraw_amount <= 1000000000000000000000);

    // Send the amount to the address that requested it
    msg.sender.transfer(withdraw_amount);
  }

  // Accept any incoming amount
  function () public payable {}
}
```

---
#### 编译合约
* 将 Solidity 代码转换为 EVM 字节码
* 使用工具：Remix
* 注意选择 Solidity 版本

---
#### 部署合约
* 将编译好的字节码注册到以太坊区块链上
* 合约注册通过一个特殊的**交易**，交易目标地址0x0
* 通过 Remix IDE 和 MetaMask 完成部署，得到合约地址
* 现在，我们编写的合约已经在**世界计算机**上运行

--- 
### 使用合约
工具：Remix
* 合约地址
* 合约通过交易的方式触发--创建到特定函数的内部交易 和 确认交易

* 推荐教程：[CryptoZombies](https://cryptozombies.io/zh/course/)

--- 
## 比特币 vs 以太坊
* 比特币没有账户的概念，基于UTXO，而以太坊中是具有账户概念
* 比特币的交易和解锁脚本功能相对单一，以太坊合约则非常灵活
* 比特币的交易输出可以被引用，树状关系，而以太坊的合约之间可以互相调用，网状关系
* 以太坊的各个合约部署完毕之后，可以看做是世界计算机上的类库，只是这些类库调用很多是需要花钱（gas）的

