[TOC]

# 由来

遇见了未来 bitcoin 上交易必将拥堵，提供新的解决方案

属于 L2 和 L3, 专注不需要 bitcoin 分叉的解决方案，给区块链扩容的新方案

可降低链上交易成本

ETH 上智能合约的问题

* 缺乏扩展性：全网每个节点都必须运行同一段代码，当合约中有复杂计算将造成全网拥堵
* 缺乏隐私性：合约是全网公开运行的，可能会泄露敏感信息

由 Peter todd 在 2016 提出，后面由 LNP/BP (LNP lightning proposal / Bitcoin proposal ) 协会主导开发，负责人为 Dr.Maxim , 该组织为非盈利组织跟ETH协会运作模式完全不同，由 Bitfinex 等公司提供捐赠来运营

# 关键技术

## 一次性密封 single-use-seal

基于 UTXO 只能花费一次的特性，创建UTXO时候密封，花费UTXO的时候打开密封，如果想再次花费就是 double-spending，btc 的共识规则可以保障防止双花

交易状态的哈希提交到链上, 实际状态数据则保存在链下

- 使用 RGB 协议的交易数据都保存在链下、
- 比特币区块链仅用作防止资产重复花费的承诺层
- 不受比特币交易的数据结构的限制
- 兼容闪电网络

## 客户端验证 CSV

原因：针对传统区块链来说，让所有人都必须验证所有的事这一假设从根本上无法实现，解决 ETH 这类传统合约的问题

不由所有网络成员来验证，而只是让参与方自行验证

合约不是存储在链上，而是在客户端，因此合约需要开发人类可读的

## 优势

- 降低了对区块空间的使用
- 扩展性
- 隐私性：链上看不到支付数据
- 灵活性

## 注意事项

RGB 存储数据不存储在链上，而是在本地，存储位置叫 stash

 因为RGB不是智能合约，所以其他节点不会有合约信息，丢失 stash 意味着丢失合约以及合约状态，即：资产， 所以需要备份

# 现状

2023 年 4 月，发布 RGB v0.10 ， 9月 stable

目前正在开发 v0.11 版本，变化会很大

# 资源

https://github.com/LNP-BP/LNPBPs LNP/BP 协会的各种规范和提案

https://rgb.info/  官网，原理相关

https://github.com/RGB-WG 代码仓库，所有相关组件源码，都是基于 rust 开发，后面大部分网站这里都有仓库，可直接 clone 仓库在本地用 zola 或者 gitbook 查看网站

https://rgb.tech/ 官网，技术相关

https://blackpaper.rgb.tech 黑皮书，目前没写的章节很多

[LNP/BP youtube s](https://www.youtube.com/@LNPBP/videos) 有很多教学视频

其他非官方

[awesome-rgb-protocol](https://github.com/22388o/awesome-rgb-protocol)





## 相关项目

## PePe-RGB

@PepeRgb20 已经开始在 SideSwap 上进行空投，当前 0.3u

发行在 Liquid network ，一个 BTC 侧链

## UTXO Exchange

结束 zealy 准备空投

## Bitmask 钱包

支持 Taproot 钱包和 RGB 钱包

# 未来发展和创业方向

未来会对接支付网络，用链下交易代替闪电网络，因为闪电网络需要用户保持在线，要求太高了

> 闪电网络问题：闪电网络通道 inbound/outbound liquidity problem 钱充值多了，通道会炸（ 待研究）

1. 交易市场
	NFT
	DIBA
	token
	UTXO exchange
	Bitswap
2. 方便发布合约的工具，和 UTXO 关联，类似 ETH Remix
3. wallet
4. 付费的区块浏览器 rgbscan，机构大户注重隐私，散户还好，可以有偿上传数据
5. 合约商店 contract shop



# 重要概念和使用流程

## 发行资产

人员角色

* Developer：合约开发人员
* Issuers  合约发行人，无需编程基础

* Contract owner(users)：用户

跟合约相关的3个概念

* interface：类似各个语言的接口，如 java 的interface ，rust 的 Trait ，定义功能
* schema：包含所有业务逻辑，比如允许的操作，验证规则，各种数据类型的变量
* contract：就是 schema 的一个 instance，实例化的时候设置各种构造方法的参数

如果当前没有满足要求的 contract，可以让开发人员开发 schema 和 interface，rust 编写

当前合约可以用 yaml，toml 来编写，未来可以用 Contractum 来编写，目前官方内容还不算太完善

发行资产就是利用一个合约来配置各种参数

* 资产名称
* 总供应链
* 受益人
* 谁有权增发，重命名资产

发行资产无需 gas

### schema 类型

* NIA 非通胀资产  RGB20，发完了就改不了参数了，适合作为稳定币
* UDA 唯一数字资产 RGB21，类似 NFT

* CFA 收藏品可替代资产 RGB25 , 比如游戏代币

  

## 转移资产

本质就是：UTXO 的所有者创建状态变更，定义谁的哪个UTXO将持有这个资产

资产转移是通过 contract consignment 合同寄售的形式来转移，有两种形式：

* issuer to user: 其实就是发型资产时候，指定受益人

* user to user  状态转移，就是用户之间的资产转移

想要资产的 user 必须先将 contract 导入自己的 RGB 钱包，也进一步说明了合约不是存储在链上

### 批量转移

同时向多人RGB付款时候，仍然只需要一个btc交易，前提是只能有 1个 UTXO 输入

如果有多个 UTXO 在 btc 主网就要多交钱

这引申出了一些新的服务 - UTXO 服务商，例如：交易所可以 30 分钟聚合一次取款请求为一个 UTXO 输入 来减小 fee

## 交易验证

只验证跟自己钱包相关的交易

需要验证资产从初始状态到当前状态的所有交易

如果 RGB 验证失败，但是 btc 交易广播到主网，那么这笔 RGB 资产会永久丢失

[Bulletproofs](https://blog.blockstream.com/en-bulletproofs-faster-rangeproofs-and-much-more/) 利用该技术隐藏交易历史状态转换的金额，zk 

## 通信协议

- [Storm](https://github.com/Storm-WG/storm-spec)：基于闪电网络构建的 p2p 消息传递和存储系统
- [RGB Proxy Server](https://github.com/RGB-Tools/rgb-http-json-rpc)：一个标准化的HTTP JSON-RPC服务器

# 官方代码仓库说明

## RGB Core

客户端共识和验证的逻辑
处理序列化需要启用 serde

## RGB Schema

example 目录中有使用示例，其中 rgb20.rs 跟 rgb20-usdt代码基本一样
主要是不同合约类型的构造方法
可以生成相应的rgb合约文件
运行后会生成二进制格式的rgb文件以及文本格式的rgba文件

## RGB std

实际上就是 rgb-wallet lowest level
钱包库，依赖 rgb-core
包括子项目 rgb-std  0.11 没了
各个 schema 的构造方法 rgb20 rgb21 等

## RGB

rgb  high-level
RGB smart contracts: command-line tool & wallet runtime library for desktop and mobile integration
该存储库提供面向客户端的库，桌面应用程序和移动钱包可以使用该库来集成 RGB 支持。它还提供在命令行中运行的二进制文件 rgb ，并在本地公开所有RGB功能，只需要Electrum服务器
cli 相关命令参考 command.rs，其中定义了每个cli命令的参数和执行命令的逻辑

不需要在Electrum服务器之外建立任何节点或服务器连接

## RGB Node

由于没有发布 v0.10 版本，不兼容当前 RGB 协议 目前仓库是 archive 只读状态

## AluVM
基于 RISC 的虚拟机
基于寄存器，不允许随机内存访问

目前文档还不全

# 发布合约和转账例子

用户 iss 拥有所有合约发行后的所有 USDT，然后他给 alice 转账

参考 `https://github.com/oneforalone/rgb20-usdt.git`, 以下为更加详细的说明

## 前期准备

### 准备钱包

* 下载钱包 [Sparrow](https://sparrowwallet.com/)

* 创建 2 个钱包，名字为 iss 和 alice，注意：script Type 要选择 Taproot(P2TR)

* 分别领水 https://bitcoinfaucet.uo1.net/ and https://coinfaucet.eu/en/btc-testnet/

* 领水成功后 UTXOs 中出现记录代表领水成功

### 修改例子代码

修改 rgb20-usdt 中 main.rs 中的 assetOwner 为 iss 的某个 UTXO
```
let txid = "1c5bb5a57b5ea992f31681ed20a78d183929dbd0ae3cd52bfe275d0076fbdda7"; // 改为实际值
let vout_index = 2; // 改为实际值
```

然后，运行 main.rs ，新的 contract 会生成在 contracts 目录下

contracts/rgb20-usdt.contract.rgb
contracts/rgb20-usdt.contract.rgba



## 测试转账

### 流程概述

* 创建俩钱包，代表两个用户
* 两个钱包分别导入合约
* 收款人生成收款发票，然后发给付款人
* 付款人做一个转账生成一个 PSBT 文件，但不广播到主网
* 付款人结合收款发票和PSBT文件生成转账文件
* 付款人将转账文件发给收款人
* 收款人验证并接受转账文件
* 付款人将交易广播到主网

说明：目前有 2 个步骤需要发送数据，一个是收款人要把发票信息给付款人，以及付款人要将转账文件发给收款人进行确认

以下是更加详细的步骤：

### 详细流程

#### 创建钱包

```
# 创建 iss 用户的钱包
$ rgb -d .iss create default 
tpubDCLAUWto7EcB3a5sCRov5hLfyBS8fZKV88hgmTGMQY2WBFFK8v7ZCwA9C6tyR8u3Pdr84n7hJMkezfycSLMm9PH1uasf91a28bAJT5oKSKe
# 导入合约到钱包
$ rgb -d .iss import contracts/rgb20-usdt.contract.rgb

# 创建 alice 用户的钱包
$ rgb -d .alice create default tpubDDNiHw3fR1m26qYX464pcRoho5cUDp5UXG4frTrak4cSXjj8uhDsqRMq2NiZUeiwQUEybgnwxPXqAq1YgzBrxLuXpiPuQL8oQH8yraR5ENd

# 导入合约到钱包
$ rgb -d .alice import contracts/rgb20-usdt.contract.rgb
```

说明：

* -d 代表将数据存储在哪个目录，这里以用户名代替，即：stash 目录，真实场景需要备份，如果丢失，所有RGB资产就丢失了
* tpub 为 Sparrow 中该用户钱包的 tpub 值
* 导入合约需要指定合约的 rgb 文件路径

#### 查看合约状态

```
# 导入合约后可以查看钱包所有合约
$ rgb -d .iss contracts

# 导入合约后可以查看钱包某个合约的状态
$ rgb -d .alice state rgb:2gX6o7v-TEVjVMvHG-Dt1tJVAKH-XYEkEKQxh-jmRfyYGLE-myVaPq1 RGB20
```

说明：

* contracts 会输出所有合约 id 集合
* 查看合约状态需要指定合约 id 和接口名称，即：rgb:2gX6o7v-TEVjVMvHG-Dt1tJVAKH-XYEkEKQxh-jmRfyYGLE-myVaPq1 和  RGB20

####  创建收款发票

需要收款人 alice 来创建，而不是付款人 iss

```
$ rgb -d .alice invoice 2gX6o7v-TEVjVMvHG-Dt1tJVAKH-XYEkEKQxh-jmRfyYGLE-myVaPq1 RGB20 750 tapret1st:61daeed36641335435a58abef51f3c90366327e010bde68d6161ddf177b5c35b:0

$ rgb:2gX6o7v-TEVjVMvHG-Dt1tJVAKH-XYEkEKQxh-jmRfyYGLE-myVaPq1/RGB20/750+utxob:JWthMh6-vMVmY7gMN-CStnnWfJw-arYjNkKyj-54MHzzDdP-aMwJnB
```

说明：

* invoice 后面跟合约id，不要带 rgb: 前缀
* RGB20 是指定接口
* 750 是收款金额
* tapret1st 是要 alice 接受转账的 UTXO
* 命令成功会输出 invoice 的信息，这个信息要发送给付款人 iss，让 iss 进行transfer 操作

####  付款人 Transfer 

iss 用户首先要生成 psbt 文件，通过 Sparrow 让 iss 用户给他自己的某个地址转账，比如：100 sat，到 boadcast 之前点击 save Trascaction，即可生成 psbt 文件，切记不要广播到链上，否则就是是普通的 btc 转账了，比如生成的文件存储到了 psbt/iss_transfer.psbt

用 alice 的 invoice 信息结合 psbt 文件进行 transfer

```
$ rgb -d .iss set-host psbt/iss_transfer.psbt

PSBT file 'psbt/iss_transfer.psbt' is updated with all required commitments and ready to be signed.


$ rgb -d .iss transfer psbt/iss_transfer.psbt rgb:2gX6o7v-TEVjVMvHG-Dt1tJVAKH-XYEkEKQxh-jmRfyYGLE-myVaPq1/RGB20/750+utxob:JWthMh6-vMVmY7gMN-CStnnWfJw-arYjNkKyj-54MHzzDdP-aMwJnB iss_transfer.rgb

Transfer is created and saved into 'iss_transfer.rgb'.
PSBT file 'psbt/iss_transfer.psbt' is updated with all required commitments and ready to be signed.
Stash data are updated.
```

说明：

* psbt/iss_transfer.psbt 为刚刚生成的 psbt 文件
* rgb:2gX6o7v-TEVjVMvHG-Dt1tJVAKH-XYEkEKQxh-jmRfyYGLE-myVaPq1/RGB20/750+utxob:JWthMh6-vMVmY7gMN-CStnnWfJw-arYjNkKyj-54MHzzDdP-aMwJnB  为 alice 的 invoice 信息
* iss_transfer.rgb 为最终输出文件，这个文件要发送给 alice 让 alice 验证

####  收款人 Accept

alice 拿到 iss 生成的 iss_transfer.rgb 需要验证并 accept 到本地

```
# validate
$ rgb -d .alice validate iss_transfer.rgb

# accept
$ rgb -d .alice accept -f iss_transfer.rgb

Consignment has non-mined terminal(s)
Non-mined terminals:
- 4410a8b888ce051fdbba9191f1e50bc73bd0106d7d5d8d91b2c51508e6123203
Validation warnings:
- terminal witness transaction 4410a8b888ce051fdbba9191f1e50bc73bd0106d7d5d8d91b2c51508e6123203 is not yet mined.

Transfer accepted into the stash
```

说明：

* 先验证 rgb 文件，没有问题再 accept
* 提示 accept 到了本地 stash

####  付款人广播交易完成全流程

iss 用户打开 Sparrow 对刚才的 Trascation 点击 boardcase 广播到网络

最终状态查看

```
# 查看 alice 状态
rgb -d .alice state rgb:2gX6o7v-TEVjVMvHG-Dt1tJVAKH-XYEkEKQxh-jmRfyYGLE-myVaPq1 RGB20


Global:
  spec := (naming=(ticker=("RGB20"), name=("USDT"), details=1(("USD Tether Token"))), precision=8)
  data := (terms=(""), media=~)
  issuedSupply := (3000000000)
  created := (1702301818)

Owned:
  assetOwner:
    amount=750, utxo=61daeed36641335435a58abef51f3c90366327e010bde68d6161ddf177b5c35b:0, witness=4410a8b888ce051fdbba9191f1e50bc73bd0106d7d5d8d91b2c51508e6123203 # owner unknown
    amount=2999999250, utxo=4410a8b888ce051fdbba9191f1e50bc73bd0106d7d5d8d91b2c51508e6123203:0, witness=4410a8b888ce051fdbba9191f1e50bc73bd0106d7d5d8d91b2c51508e6123203 # owner unknown
    amount=3000000000, utxo=1c5bb5a57b5ea992f31681ed20a78d183929dbd0ae3cd52bfe275d0076fbdda7:2, witness=~ # owner unknown
    
    
# 查看 iss 状态
rgb -d .iss   state rgb:2gX6o7v-TEVjVMvHG-Dt1tJVAKH-XYEkEKQxh-jmRfyYGLE-myVaPq1 RGB20

Global:
  spec := (naming=(ticker=("RGB20"), name=("USDT"), details=1(("USD Tether Token"))), precision=8)
  data := (terms=(""), media=~)
  issuedSupply := (3000000000)
  created := (1702301818)

Owned:
  assetOwner:
    amount=2999999250, utxo=4410a8b888ce051fdbba9191f1e50bc73bd0106d7d5d8d91b2c51508e6123203:0, witness=4410a8b888ce051fdbba9191f1e50bc73bd0106d7d5d8d91b2c51508e6123203 # owner unknown
    amount=3000000000, utxo=1c5bb5a57b5ea992f31681ed20a78d183929dbd0ae3cd52bfe275d0076fbdda7:2, witness=~ # owner unknown
```

说明：

* iss 的 assetOwner 少了750
* alice 的 assetOwner 多了 750

### 源码简要说明

rgb 命令行工具仓库 `https://github.com/RGB-WG/rgb.git`

cli 工具源码是 `command.rs`

#### create

用 runtime 来创建钱包

#### import

先分析导入文件类型是啥，然后导入

可以导入的类型为

* Iface

* Schema

* Impl

* Constract 上面例子中是合约

* Transfer

#### state

查看 wallet 中某个 contract 的状态

通过 runtime 拿到 contract，然后遍历全局状态和Owned的状态并打印

#### invoice

生成一个发票，指定合约、金额以及接受付款的的 UTXO

内部创建一个 seal 密封，然后构建 Invoice 对象

#### setHost

反序列化 psbt 文件，然后找到符合条件的output，插入一些信息

> 具体信息是做什么可能需要再研究 Tapscript

#### Transfer

用 runtime 转账给 invoice

这个代码流程较多和复杂

会做资金的转移和找零等操作，然后将 state transition 加入到 PSBT 最终生成一个 transfer 文件

#### validate

加载 transfer 文件验证 Consignment

#### Accept

还是会先 validate，然后用 runtime 接受这个 transfer，具体是用 stock 的 consume_consignment

处理密封，这个方法较为复杂

这个应该就是改变本地 stash 中的一些数据







