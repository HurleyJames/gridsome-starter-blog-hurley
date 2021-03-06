---
title: 《区块链技术指南》阅读笔记
date: 2021-02-03T21:00:00+08:00
published: true
slug: notes-blockchain-tech
tags:
- Blockchain
cover_image: "./images/notes-blockchain-tech.png"
canonical_url: false
description: 客观探索区块链概念的来龙去脉，系统剖析关键技术和原理，讲解实践应用。
---

:::note 🐣 Born

区块链是源于 2009 年初上线的比特币（Bitcoin）开源项目，而实际上，它是记账问题发展到分布式线下场景的应用。本文将会从记账问题引出，剖析区块链和分布式账本技术的来龙去脉。

:::

## 一、分布式记账与区块链

为了正常地进行商业活动，参与者通常需要找到一个**多方均可信任**的**第三方**来复杂记账，确保交易记录的准确。我们最常使用到的支付宝，在初始阶段就是扮演这样一个桥梁来连接「淘宝」和「用户」。然而，如果商业规模越来越复杂，参与的企业越来越多（企业与广大用户是不同的概念），在很多场景下，是很找到符合要求的第三方记账机构。所以，需要探讨在分布式场景下，进行协同记账的可能性。

那么，可以设计出一个简易的分布式账本：

1. **多方都可以对账本进行读写操作**。这样子，每一方都参与到了记账的工作中，可以说是完成了协同工作的部分，但是，如果有一方恶意篡改记录怎么办呢？
2. 那么，就需要引入**验证机制**。这就会用到密码学中的**数字摘要（Digital Digest）**技术。每当有新的交易记录被添加到账本上时，参与的各方都可以使用 Hash 算法来对**完整的交易历史记录**进行数字摘要，产生当前交易历史的「指纹」。如果指纹不匹配，那就说明交易记录被篡改过。同时，通过追踪指纹更改的位置，就可以定位到被篡改的历史记录。
3. 这个验证机制的确能够解决记录篡改的问题。但是，每次都记录完整历史记录的数字摘要，实在是太浪费时间。当记录变得非常庞大时，计算成本也就非常的高。因此，当发生新的交易时，只对新的交易进行二额外的验证。这个前提是，从头到摘要位置的完整历史的部分是未被篡改的。

在上述三个步骤的前提下，一个最简单的区块链结构就已经诞生了。

### Applications

从 2009 年至今，有关区块链的项目、技术开始层出不穷。从最早的加密货币——比特币，到 2015 年诞生的以太坊（Ethereum）项目，完成了区块链 1.0 到 2.0 的转变。以太坊项目对比特币存在的缺陷进行了改善，重要在于了对智能合约（Smart Contracts）的支持，这意味着可以通过图灵完备的编程语言对区块链进行开发。

2015 年底，Linux 基金会牵头，发起了超级账本（Hyperledger）的开源项目。与前两个面向公有链的项目不同，Hyperledger 主要是面向**联盟链**场景，关注企业在权限管理、隐私保护和安全性能等方面的核心诉求。这些开源项目的释放，都加速了区块链行业的发展，为更多、更复杂的应用场景提供了技术支持。

### 基本原理

区块链的基本原理并不复杂，最核心的部分有三个基本概念：

* **交易（Transaction）**：一次对账本的操作。例如一条转账记录。
* **区块（Block）**：记录一段时间内发生的所有的交易记录和状态结果，称为对当前账本状态的一次共识。
* **链（Chain）**：是按照区块的生成顺序串联而成，相当于整个账本状态变化的日志记录。它会串联起一个又一个的区块。

区块链的目标就是实现一个分布式的数据记录账本，**这个账本只允许添加、不允许删除**。所以，其基本结构其实可以看做是一个线性链表。它由一个个的区块组成，后继区块中会记录前置区块的哈希值。而这个区块是否合法，就是通过计算哈希值的方式来进行快速校验。

### 比特币工作过程

关于比特币，我们经常会听到「**挖矿**」这个概念。其实我认为，国外人起名字一直都是比较嘻哈，然后我们国内又是直接翻译过来，所以翻译出来的词语对我们而言很崭新很陌生，不明所以。挖矿是 mining，但并不是传统意义上的挖矿。它是通过大量计算机的算力，来计算某个值。假设算出来了这个值，那么就是挖到比特币了。这个值是什么呢？

Nonce。翻译过来就是「**随机串**」。因为如果算出来的哈希值满足一定条件，就可以判定这个区块是合法的，就可以将它广播出去，让其它节点对这个区块的合法性进行判断。如果验证后发现确实合法，就可以添加到自己维护的区块链结构中，那么交易就得到了确认，就产生了一笔新的交易。

那么，寻找 nonce 是需要大量的算力，如果只用一台普通计算机去计算，那可能需要计算很多很多年。所以，这种基于**算力**的共识机制就被称为**工作量证明（Proof of Work, PoW）**。工作量越大，计算出的概率就越大。

哦对了，忘记提到好处了。上面的这段话可能还有人疑惑，计算出了之后能干嘛呢？能获得比特币！截止 2021 年，现在的比特币已经经过了好几次的暴涨了，那么 1 个比特币等价于多少人民币呢？216310.76 CNY！

### 技术分类

根据参与者的不同，可以分为：

* **公有链（Public Chain）**
* **联盟链（Consortium Chain）**
* **私有链（Private Chain）**

公有链，就是任何人都可以参与使用和维护的。参与者多为匿名，前面提到了最火的两个项目，比特币和以太坊，都是公有链的典型。我曾基于以太坊区块链智能合约开发部署过一条简单的测试链，任何人就都可以访问到其中的信息。

私有链，则是由集中管理者进行管理限制。只有内部的，少数人可以使用，信息并不公开。所以，其实会认为私有链与传统的中心化记账方式差异并不大。

联盟链则是处于两者之间了，会由若干个组织一起进行合作来维护一条区块链。该区块链并不是全开放，也不是全部不公开，使用者必须是带有权限的限制访问，而部分重要信息也会得到保护。典型的项目就是 Hyperledger。

### 分布式共识

其实，我个人一直这个翻译是非常晦涩难懂的。Consensus 的确是共识的意思，中文里也的确有这个词，例如「达成共识」。但是，我们似乎很少单独地将这个词拎出来作为一个概念。但是区块链技术中，我们会经常发现「共识」这个词。我的理解是这个词在这里可以作：**协议**、**标准**或者**方式**的意思。

回到原题，共识是整个分布式系统经典的技术问题。这也是我对分布式、区块链都比较感兴趣的原因。它就像一座桥梁，将区块链与分布式账本连接了起来。

最开始，我们上面提到的**工作量证明**，就是为了规避少数人的恶意篡改行为，通过概率模型来保证最后参与方共识到最长的链。但是，这类算法的主要问题在于效率低下和能源浪费。所以，还有以权益为抵押的 **PoS** 算法和 **DPoS** 算法。

### 交易性能

一般情况下，区块链并不适用于高频交易的场景，但是因为需要作用于金融系统，所以如何提高交易能性能，包括**吞吐量**和**确认延迟**两个方面，都至关重要。这个我本人也编写过以太坊区块链的智能合约，在交易时间确认上，的确是相对较慢，一旦用户数量或者交易数量庞大，就容易造成以太坊网络的严重拥堵。

所以，提高性能的方法有一方面，提升单个节点的性能，同时设计优化后的策略和算法；另一方面就是将交易处理 offload 到线下。

## 二、分布式系统核心技术

### 一致性问题

一致性问题是如何产生的呢？因为随着业务场景的复杂，计算机规模会越来越庞大，从而会形成集群化。那么，集群系统就需要确保一致性。节点要处于相同的状态，同一时刻会收到相同的请求，这些都是一致性问题内的范畴。

规范来看，分布式系统要达成一致性的过程，需要满足：

* **可终止 （Termination）**：一致的结果在有限时间内能够完成。可终止性就意味着可以保障提供服务。但是有时候就会出现问题，并不能得到保证。例如，“服务中断”等。
* **合同（Agreement）**：不同节点最终完成决策的时间是相同的。合同就意味着**安全性**，这意味着要么不给出结果，要么得出的结果都是达成了共识的。
* **合法性（Validity）**：决策的结果必须是某个节点提出的提案。即达成的结果必须是节点执行操作的结果，不能是另外的方案。

### 共识问题

共识问题经常会与上面提到的一致性问题一起讨论。但实际上，一致性问题含义更为宽泛，所以，达成了某种共识并不意味着就保障了一致性。

> 共识算法解决的是分布式系统对某个提案（Proposal），大部分节点达成一致意见的过程。计算机世界里采用的是典型的“多数人暴政”，足够简单、高效。

共识算法可以分为 Crash Fault Tolerance (CFT) 和 Byzantine Fault Tolerance (BFT) 两类。对于拜占庭问题，又分为拜占庭错误和非拜占庭错误。一般，出现故障但是并不会伪造信息的称为“**非拜占庭错误**”，伪造信息的情况称为“**拜占庭错误**”，对应的节点就是拜占庭节点。

对于非拜占庭错误的情况，有一些经典的算法，如 Paxos 算法、Raft 算法等。这种容错算法性能比较好，处理快，不会容忍超过一半的故障节点。

对于容忍拜占庭错误的情况，就包括了以 PBFT 为代表的**确定性系列算法**、PoW 为代表的**概率算法**等。确定性算法一旦达成了共识就不可逆转，而概率性算法的结果则是临时的。**拜占庭类容错算法**往往性能较差，不会容忍超过 1/3 的故障节点。

关于 Paxos、Raft 和拜占庭算法的剖析，您可以看我的这篇文章：[Paxos、Raft 和 PBFT 算法剖析]()。

### FLP 不可能原理

> FLP 不可能原理：在网络可靠，但允许节点失败（即便只有一个）的最小化异步模型系统中，不存在一个可以解决一致性问题的确定性共识算法。

FLP 不可能原理告诉我们，不要浪费时间，试图为异步分布式系统设计面向任意场景的共识算法。

### CAP 原理

> CAP 原理：分布式系统无法同时确保一致性（Consistency）、可用性（Availability）、分区容忍性（Partition），设计中往往需要弱化对某个特性的需求。

所以，分布式系统最多只能保证三项特性中的两项特性。

* **一致性**：任何事务都应该是原子的，所有副本上的状态都是事务成功提交后的结果，并且保持强一致；
* **可用性**：系统能够在有限时间内完成对操作请求的应答；
* **分区容忍性**：系统中的网络可能会发生分区故障，即节点之间的通信无法保障。

#### 弱化一致性

对于一个结果一致性要求不是很敏感的应用，就允许在新版本上线过后的一段时间内才更新成功，这期间允许不保证一致性。例如，一些网站静态页面、一些实时性较弱的查询数据库等，如果 CouchDB、Cassandra 数据库等，都是遵循这个原则来设计的。

#### 弱化可用性

对于结果一致性很敏感的问题，比如银行取款，当系统出现故障就会立即拒绝服务。MongoDB、Redis、MapReduce 等都是基于此设计的。而 Paxos、Raft 等共识算法，就是主要处理这种情况。

#### 弱化分区容忍性

现实中，网络分区出现概率相对是较小的，但是也不可完全避免。一些关系型数据库包括 ZooKeeper 都是考虑了这种设计。

## 三、密码学与安全技术

### Hash 算法与数字摘要

Hash 算法，翻译为哈希算法或者散列算法，又常被称为指纹（fingerprint）或者摘要（digest）算法，是非常基础的一种算法。它是将任意二进制长度的明文串映射为较短的二进制串（哈希值），并且不同的明文是很难被映射为相同的哈希值的。

所以，一个优秀的 Hash 算法，需要满足：

* **正向快速**：给定文字和哈希算法，在有限时间和有限资源内就能快速计算出 Hash 值；
* **逆向困难**：给定 Hash 值，在有限时间内基本不可能能够逆推出原文；
* **输入敏感**：如果原始的输入信息发生了任何改变，新产生的哈希值都会产生很大的变化；
* **碰撞避免**：两段内容不同的明文，哈希值是不太可能一致的，也就是不太可能会发生碰撞

### 常见算法

目前常见的 Hash 算法，包括了 Message Digest (MD) 系列和 Secure Hash Algorithm (SHA) 系列算法。

MD 算法主要包括了 MD4 和 MD5 两个算法，MD4 已经被证明了不安全，MD5 则在 2004 年被成功碰撞了，所以安全性也已经不满足应用于商业场景。

SHA 算法也有好几个系列，SHA-1 已经与 2005 年被碰撞了，后来就制定了更安全的 SHA-224、SHA-256、SHA-384 以及 SHA-512算法等。这些都统称为 SHA-2 算法。

### Merkle 树

梅克尔树（又叫哈希树），是一种典型的二叉树结构，由一个根节点、一组中间节点和一组叶节点组成。它的主要特点是：

* 最下面的叶子节点，包括存储数据或者哈希值
* **非叶子节点（包括中间节点和根节点）都是他的两个孩子节点内容的哈希值**，即两个兄弟节点的标签串联起来作为哈希函数的输入，经过计算会得到父节点的哈希值

### Bloom Filter 结构

布隆过滤器，是一种基于 Hash 的高效查找结构，能够快速（在常数时间内）回答“某个元素是否在一个集合内”的问题。因此，该结构因为其高效性，被大量运用到如信息检索、垃圾邮件识别、网页 URL 去重化、缓存穿透等领域里。

当我们往数组或者 list 中添加新的数据时，索引值和插入数据的值是没有直接关系的。因此，当我们遍历数组或者列表的时候，往往需要遍历整个集合，所有的元素。如果存在大量的数据，这就会影响查找的效率。

但是，如果我们使用 HashMap 这个键值对的数据结构去存储，那么索引值就是根据插入项的值来确定的，搜索速度就非常快了。布隆过滤器就是受此启发而来。

为了将数据项添加到布隆过滤器中，我们会提供 K 个不同的哈希函数。通过使用多个哈希函数，来产生多个索引值。其判断的最终依据是：

> 当我们搜索一个值的时候，若该值经过了 K 个哈希函数运算后的任何一个索引位都为 0，那么该值就肯定不在集合中。但是如果所有的哈希索引值均为 1，则只能说明搜索的值可能存在于集合中。

## 四、比特币

比特币（BitCoin, BTC）是基于区块链技术的一种数字货币的实现，而比特币网络是历史上首个经过大规模长时间校验的数字货币系统。它的特点是：

* 去中心化：这意味着没有任何独立个体可以对网络中的交易进行破坏
* 匿名性：比特币网络中的账户地址都是匿名的，无法从交易信息关联到具体的个体
* 通胀预防：比特币的发行是需要通过挖矿来进行的，而发行量每四年会减半，总量是 2100 万枚比特币，是无法被超发的

### PoW

工作量证明机制，是通过计算来猜测一个数值 nonce。因为 Hash 问题具有不可逆的特点，所以除了暴力计算，就很难有有效的算法来解决。所以，谁的算力多，谁先解决问题的概率就越大，如果某个人拥有了超过全网一半以上的算力时，那么从概率上讲，他就拥有了控制整个网络的链的权利。这就是所谓的 **51% 攻击**。

### PoS

基于权益证明（Proof of Stake, PoS），最早是在 2013 年提出的，类似于现实生活中的股东机制。那么，拥有股份越多的人，就越容易获取记账权。PoS 这样做是为了解决在 PoW 中大量资源被浪费的缺点。除此之外，还有一些改进的算法，如授权股权证明机制（DPoS）。通俗来讲，就是股东们投票选出一个董事会，只有董事会中的董事才有资格来进行代理记账。

### 闪电网络

比特币最为人诟病的缺点就是交易性能：交易速度远低于传统的金融交易系统，而确认时间又很长。所以，社区提出了闪电网络的创新设计。它的思路就很简单，将大量交易都放到比特币区块链之外来进行，只把关键部分放到链上进行确认。闪电网络主要通过引入**智能合约**来完善链下的交易渠道。

### 侧链

侧链（Sidechain）协议允许在比特币区块链和其它区块链之间进行互转。因为越来越多的“山寨币”的出现，在碎片化整个数字货币的市场，再加上以太坊区块链的竞争，比特币区块链就希望能够通过借助侧链的方式来扩展整个比特币的底层协议。

简单来说，它就是以比特币区块链为**主链**，其它的区块链为侧链，二者通过**双向挂钩**（Two-way peg），可以实现从比特币从主链迁移到侧链来进行流通。

## 五、以太坊

在我看来，以太坊最重要的好处是从单一的基于 UTXO 的数字货币交易，延伸到了图灵完备的通用计算领域。用户可以自行设计任意复杂的智能合约逻辑。基于以太坊项目，智能合约的开发者使用官方提供的工具和以太坊开发的编程语言 Solidity，可以很容易地开发出在以太坊网络上的“去中心化”应用（Decentralized Application, DApp）。这些应用将运行在以太坊的虚拟机（Ethereum Virtual Machine, EVM）上，用户可以通过以太币（Ether）来购买交易需要的燃料（Gas），从而维持所部署的应用。

* 支持图灵完备的智能合约，设计了编程语言 Solidity 和虚拟机 EVM；
* 选用了内存需求较高的哈希函数，避免出现强算力矿机、矿池攻击；
* 叔块（Uncle Block）激励机制，降低矿池的优势，并减少区块产生间隔（10 分钟降低到 15 秒左右）;
* 采用账户系统和世界状态,而不是 UTXO，容易支持更复杂的逻辑；
* 通过 Gas 限制代码执行指令数，避免循环执行攻击；
* 支持 PoW 共识算法，并计划支持效率更高的 PoS 算法。

### 智能合约

智能合约（Smart Contract）我认为是以太坊中最重要的一个概念，就是以计算机程序的方式来缔结和运行各种合约。之前提到过，比特币系统中是不存在账户的概念的，而以太坊中则支持在不同的账户之间转移数据，以实现更为复杂的逻辑。

以太坊账户分为两种类型：**合约账户（**Contracts Accounts）和**外部账户**（Externally Owned Accounts, EOA）。

* **合约账户**：存执执行的智能合约的代码，只能被外部账户用来调用激活；
* **外部账户**：是以太币的拥有者的账户，对应到某个公钥。账户信息包括 nonce、balance、storageRoot、codeHash 等字段。

除此之外，**交易**的概念，是指从一个账户到另一个账户的消息数据。每个交易包括以下的字段：

* `to`：目标账户的地址
* `value`： 可以指定转移的以太币的数量
* `nonce`：交易相关的字串，用于防止交易重叠
* `gasPrice`：交易所要消耗的燃料价格
* `gasLimit`：交易消耗的最大的燃料值
* `data`：交易附带的字节码的信息，可以用于创建和调用智能合约
* `signature`：签名信息

既然有比特币，那么以太坊区块链中也有以太币。当然相对而言，就没有那么火热了。以太币的作用主要是用来购买材料的，支付给矿工来维护网络运行智能合约的费用。以太币的最小单位是 wei，一个以太币等于 $10^{18}$ 个 wei。

燃料（Gas）是指每执行一条合约指令会消耗的固定的燃料。它可以用来跟以太币进行兑换。

### 相关工具

前面提到了以太坊支持使用图灵完备的编程语言进行开发，所以相应的开发工具和开发库等目前都已经比较完善，这里列出一些常见的：

* **Geth**：go-ethereum 的独立客户端 Geth 是最常用的以太坊客户端之一。用户可以通过安装它来接入以太坊网络，并成为一个完整的节点。它也可以作为一个 HTTP-RPC 服务器，对外暴露 JSON-RPC 接口，供用户与以太坊网络交互。
* **Misk**：是官方提供的，一套包含图形界面的钱包客户端。
* **Truffle**：一个功能丰富的以太坊应用开发的环境。
* **Embark**：一个 DApp 的开发框架，支持集成以太坊、IPFS 等。
* **Remix**：一个用于编写智能合约的 Solidity 的网页版的 IDE。

## 六、Hyperledger

作为一个由 Linux 基金会牵头的联合项目，超级账本由很多面向不同场景的子项目构成，主要有 12 个顶级的大项目。其中，最主要的就是 Fabric 项目。

Hyperledger Fabric 是最早就加入 Hyperledger 的项目，它的定位是面向企业的分布式账本平台，基于 Go 语言实现。它的特点有：

* 完备的权限控制和安全保障：成员必须被许可才能加入网络，例如通过证书、加密、签名等手段来保证安全。这样能够保证只有参与交易的节点才能访问数据。
* 模块化设计，可插拔架构：数据库可以任意选择，身份管理机制也可以选择，共识机制和加密算法也是可插拔的，可以根据实际情况来替换。
* 高性能，可扩展：Fabric 采用模块化架构把交易处理划分为 3 个阶段：通过 Chaincode 进行分布式业务逻辑处理和协商；交易排序（Peers）；交易的验证和提交（Orderers）。Peer 节点和 Orderer 节点可以独立扩展，可以动态增加。

> Hyperledger Fabric 网络中的节点必须经过授权认证后才能加入，从而避免了 PoW 的资源开销，大幅提高了交易处理的效率。Hyperledger Fabric 采用了高度模块化的系统设计理念，将**权限认证模块**（MSP）、**共识服务模块**（Ordering Service）、**背书模块**（Endorsing peers）、**区块提交模块**（committing peers）等进行分离部署，使得开发者可以根据具体的业务场景替换模块，实现了模块的插件式管理（plugin/plug-out）。

### 基本概念

1. **Ledger**：账本，节点维护的区块链和状态的数据库。
2. **World State**：直译过来就是“世界状态”，其实就是一个数据库，存储的是账本的当前值。
3. **Channel**：通道，是私有的子网络，通道中的节点共同维护账本，实现数据的隔离和保密。每个 Channel 对应一个版本，由加入该 Channel 的 Peer 维护，一个 Peer 可以加入多个 Channel，维护多个账本。
4. **Org**：Orginazation，管理一系列成员的组织。一个 Channel 内可以有多个组织。
5. **Chaincode**：链码，其实就是智能合约，运行在节点内的程序，提供业务逻辑接口，对账本进行查询或更新。
6. **Endorse**：背书，指一个节点执行了一个交易并对结果进行签名后返回响应的过程。
7. **Ordering Service**：排序服务，将交易排序后放入区块中，并广播给网络各节点。
8. **PKI**：Public Key Infrastructure，一种遵循标准的利用公钥加密技术为电子商务的开展提供一套安全基础平台的技术和规范。
9. **MSP**：Membership Service Provider，成员管理服务，基于 PKI 实现，为网络成员生成证书，并管理身份。