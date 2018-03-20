# xStorage 初步构想和总体思路

**<p align="right">Paul 2018-3-15</p>**
 
***
## 目录
[toc]
***

## 一、初步构想

### 1. xStorage 定位

A peer-to-peer file store product (protocol) to achive the distribute storage cheaper, faster, safer, and more privacy.
    
一款端到端的安全、高效、低成本、高隐私的分布式文件存储产品(协议)。

### 2. xStorage 特性
    
- #### Decentralized (去中心化)
    p2p网络
- #### Safe
    网络安全、信息安全
- #### Anonymity & Privacy(匿名)
    信息所用者可匿名、信息私有
- #### Cheap & Efficient
    网络维护、信息存储
- #### Open & BitCoin Motivate
    端节点自由进出、类似Bitcoin的激励机制

## 二、相关前沿项目及其优劣分析

xStorage 相关前沿项目，主要分为两类： 1）围绕blockchain相关的协议改进和拓展的项目；2）围绕分布式文件存储进一步去中心化的项目。

### 1. [IPFS](https://ipfs.io/) (Inter Planetary File System)
    
IPFS is the Distributed Web. A peer-to-peer hypermedia protocol to make the web faster, safer, and more open^[https://ipfs.io/].

IPFS aims to replace HTTP and build a better web for all of us. **Here's how IPFS works**：

Let's take a look at what happens when you add files to IPFS:

Each file and all of the blocks within it are given a unique fingerprint called a cryptographic hash.

IPFS removes duplications across the network and tracks version history for every file.

Each network node stores only content it is interested in, and some indexing information that helps figure out who is storing what.

When looking up files, you're asking the network to find nodes storing the content behind a unique hash.

Every file can be found by human-readable names using a decentralized naming system called IPNS.

换种说法，IPFS提供了一个高吞吐量、按内容寻址的块存储模型，及与内容相关超链接^[https://zh.wikipedia.org/w/index.php?title=%E6%98%9F%E9%99%85%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F&oldid=48588037]。支持：

a) 在 /ipfs 和 /ipns 下挂载全球文件系统
    
b) 挂载的个人同步文件夹，拥有版本功能

c) 文件加密，数据共享系统

d) 可用于所有软件的带版本的包管理器（已经实现^[https://github.com/whyrusleeping/gx]）

e) 可以作为虚机的根文件系统

f) 可以作为数据库：应用可以直接操作 Merkle DAG，拥有 IPFS 提供的版本化、缓存以及分布式特性

g) 可以做（加密）通讯平台

h) 各种类型的 CDN

i) 永久的 Web，不存在不能访问的链接

- #### Advantage
    
    HTTP is inefficient and expensive
    
    Humanity's history is deleted daily       
    
    The web's centralization limits opportunity     
    
    Our apps are addicted to the backbone

    网络永不宕机、信息永不消失
    
    开源、开放
    
    类Bitcoin的激励机制
    

- #### Disadvantage
    
    带宽和存储资源消耗大

    不支持用户匿名、信息私有
    
    缺少商业运维
    

### 2. [ZeroNet](https://zeronet.io/)

ZeroNet uses Bitcoin cryptography and BitTorrent technology to build a decentralized censorship-resistant network.^[http://zeronet.readthedocs.io/en/latest/]

开放，自由，去中心化的网络，使用Bitcoin加密和BitTorrent网络。^[https://zeronet.io/]

Features^[https://github.com/HelloZeroNet/ZeroNet]:

a) Real-time updated sites

b) Namecoin .bit domains support

c) Easy to setup: unpack & run

d) Clone websites in one click

e) Password-less BIP32 based authorization: Your account is protected by the same cryptography as your Bitcoin wallet

f) Built-in SQL server with P2P data synchronization: Allows easier site development and faster page load times

g) Anonymity: Full Tor network support with .onion hidden services instead of IPv4 addresses

h) TLS encrypted connections

i) Automatic uPnP port opening

j) Plugin for multiuser (openproxy) support

k) Works with any browser/OS


- #### Advantage
    
    去中心化

    无托管费用

    网络永不宕机、信息永不消失

    动态内容、实时更新，支持多用户站点
    
    开源、开放

- #### Disadvantage
    
    带宽和存储资源消耗大

    不支持用户匿名、信息私有
    
    缺少激励机制


### 3. [Twister](http://twister.net.co/)

Twister is the fully decentralized P2P microblogging platform leveraging from the free software implementations of Bitcoin and BitTorrent protocols.

Twister是一款测试性的P2P微型博客自由软件。它是完全分散式的，所以没有什么单独的位置可以攻击，进而无人可以让它停止工作。这个软件系统使用端对端加密以保护信息交互安全。软件基于BitTorrent和比特币，并且意图建立一个分散式的Twitter克隆^[https://zh.wikipedia.org/zh-hans/Twister]。

### 4. [Mysterium Network](https://mysterium.network/)

Decentralised VPN built on blockchain.

Open Sourced Network allowing anyone to rent their unused Network traffic, while providing a secure connection for those in need.

Mysterium Network acts as a marketplace, its Open Source software allows anyone to join the network both as a provider – selling unused network traffic or as a customer –  buying VPN service from other Mysterium Network providers.

Mysterium Network is based on a complex architecture revolving around P2P, Blockchain, Smart Contracts, State Channels, etc.. Read our WHITEPAPER for a detailed description on how the protocol will enable the creation of completely decentralized VPN network, powered by decentralized micro payments.

### 5. [FIGTOO](http://www.figtoo.com/#/) 无花果

基于赤链技术的自治共享存储网络，全新的存储方式。

IPFS FIGTOO(无花果)，共享全球存储资源，利用赤链技术，将文件分片存储，构建去中心化的云存储，成为全球赤链分布式文件存储的基础设施。^[https://cdn.thiwoo.com/RedChain/reeed_white.pdf]

- #### Advantage

    颗粒化上传:限制文件最小尺寸，对文件进行编译，上传后的文件会被切割成非常小的片段
    
    数据在线冗余:数据多份存储，保证数据的安全性，根据节点的在线情况进行弹性冗余
    
    数据完整，快速处理:多份存储保证数据的完整性,颗粒化存储保证数据处理（上传和下载）快速

- #### Disadvantage

    带宽和存储资源消耗大

    不支持用户匿名、信息私有
    
### 6. Google File System (GFS)

Google File System (GFS or GoogleFS) is a proprietary distributed file system developed by Google to provide efficient, reliable access to data using large clusters of commodity hardware. A new version of Google File System code named Colossus was released in 2010.^[https://en.wikipedia.org/wiki/Google_File_System]

GFS专门为Google的核心数据即页面搜索的存储进行了优化。

GFS只能有一个主服务器——代码不允许存在多个主服务器。这看起来是限制系统可扩展性和可靠性的一个缺陷。

### 7. General Parallel File System (GPFS)

GPFS(General Parallel File System ,GPFS) 是 IBM 公司第一个共享文件系统，起源于 IBM SP 系统上使用的虚拟共享磁盘技术( VSD )。作为这项技术的核心， GPFS 是一个并行的磁盘文件系统，它保证在资源组内的所有节点可以并行访问整个文件系统；而且针对此文件系统的服务操作，可以同时安全地在 使用此文件系统的多个节点上实现。GPFS允许客户共享文件，而这些文件可能分布在不同节点的不同硬盘上；它提供了许多标准的 UNIX 文件系统接口，允许应用不需修改或者重新编辑就可以在其上运行。

### 8. Lustre

Lustre是一款开源的，基于对象存储的集群并行分布式文件系统，具有很高的扩展性、可用性、易用性、性能等，在高性能计算中应用很广泛，世界十大超级计算中心当中的七个以及超过50%的全球top50超级计算机都在使用Lustre。可以支持上万个节点，数以PB的数量存储系统。

基于对象的文件系统将文件的元数据和文件数据分离开来，存储在不同的服务器上。文件系统的客户用利用元数据服务器和对象存储服务器来为用户提供一个完整的文件系统抽象。基于对象的文件系统具有性能优势，文件系统客户通过对象存储服务器来访问文件内容，而元数据服务器只有在文件打开时才需要连接。

### 9. [Ceph](https://ceph.com/)

Ceph is a distributed object, block, and file storage platform.

Ceph 是一个专注于分布式的、弹性可扩展的、高可靠的、性能优异的存储系统平台，可用于为虚拟机提供块存储方案或通过 FUSE 提供常规的文件系统。Ceph 是个高度可配置的系统，管理者可以控制系统的各个方面。它提供了一个命令行界面用于监视和控制其存储集群。Ceph 也包含鉴证和授权功能，可兼容多种存储网关接口，如 OpenStack Swift 和 Amazon S3。

    
### 10. [NDN](https://named-data.net/) (Named Data Net)

命名资料网络（Named Data Networking，NDN）是一个未来的互联网架构（Future Internet Architecture），灵感来自多年在网络使用上的实证研究，导致人们逐渐意识到，现今的互联网架构中在旧有的 IP协定上悬而未决的问题[1][2] 。 NDN根源于一个Van Jacobson在2006年首次公开展示的专案计划“内容中心网络”（Content-Centric Networking，CCN）。NDN的目标在于检视如何依据Jacobson的提议，从现今以主机为中心的网络架构IP演进到以资料为中心的网络架构NDN。一般相信这项概念上的简单转变，将对人们如何设计、开发、部署和使用网络以及应用程序产生深远的影响。

互联网目前主要用作信息散布的平台，但这并非 IP 的强项。因此，未来互联网的“瘦腰”（thin waist）架构应当基于命名资料，而非以数字定址的主机。基本原则是，通信网络应该允许用户专注于他或她需要的资料，也就是所谓的“内容”（content），而不必指定资料要取自哪个实际位置，也就是所谓的“主机”（host）。由于当前的互联网中，绝大多数（“高达流量的90％”）的网络使用方式都是由单一来源传播到多个用户[4], 命名资料网络的许多机制均具有广泛的优势潜力，例如内容快取，以减少拥塞并提高传递速度，简化网络设备配置，以及在资料层级就直接内建网络安全性。

## 三、涉及的关键问题及当前的在研项目

### 1. 提高存储性能

传统Bitcoin，容量有限，交易速度太慢，单纯基于区块来构建维护hash table，不满足大规模使用后，大容量、高频次存储需求，需要扩容和加速。

目前研究提高BlockChain的传输交易速率已经成了研究的一个关键方向，相关技术有Segregated Witness(隔离见证)，在研项目有Lightning Network^[https://en.wikipedia.org/wiki/Lightning_Network], EOS^[https://eos.io/]等，其中EOS宣称The mainnet will probably start in a single thread mode with approximately 50,000 TPS。

### 2. 解决用户匿名和信息私有

作为端到端的分布式文件存储系统，应当解决用户的匿名性和信息的保密性，信息应当匿名加密后再适度冗余的分发到各个端节点。

TumbleBit^[https://github.com/BUSEC/TumbleBit], An Untrusted Bitcoin-Compatible Anonymous Payment Hub,TumbleBit’s anonymity properties are similar to classic Chaumian eCash: no one, not even the Tumbler, can link a payment from its payer to its payee.

ZeroLink^[https://github.com/nopara73/ZeroLink], the product of the combined efforts between developers from SamouraiWallet and HiddenWallet, promises to make using Bitcoin fully anonymous, something that has never been achieved before.

Zerolink is not only an excellent anonymity technique; it’s a whole process that’s called the Wallet Privacy Framework.

The Wallet Privacy Framework consists of a pre-mix and post-mix wallet, as well as a mixing technique. Three layers of anonymization are implemented to ensure that users are not deanonymized through tactics such as network analysis.


## 四、落地推进的总体思路

### 1. 构建软硬件总体框架，分层次选定关键问题

解析开源项目，快速构建软硬件架构总体框架，分层次选定关键问题，探寻解决的技术途径，形成一系列小问题的解决方案。

### 2. 最小化系统边界，分步骤实现原型（协议）

考虑落地的现实性，截取xStorage中关键的部分，构建一个系统原型（协议）样例，并做实验，根据实验结果，形成SCI一篇。

### 3. 进度安排

4月份确定技术方案；8月前实现基本框架协议，达到实验的条件；10月份形成1.0版本；12底择机进一步落地，撰写完成SCI一篇。

## Reference
