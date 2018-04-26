# xStorage IPFS Framework Analysis

**<p align="right">Paul 2018-4-20</p>**

***
## 目录
[toc]
***

## Chapter 1. IPFS 项目家族构成

### Protocol Labs

We believe the internet has become humanity's most important technology. We build protocols, systems, and tools to improve how it works. Today, we are focused on how we store, locate, and move information.

IPFS的家族有很多模块项目，而且每一个模块项目都可以单独的拿出来去使用。

![image](https://github.com/xStorage/documents/raw/master/Resource/ipfs-familly)

### 1. [IPFS](https://ipfs.io)

IPFS is a new protocol to decentralize the web. IPFS enables the creation of completely decentralized and distributed applications, using content addressing and digital signatures.

IPFS makes the web faster, safer, and more open.

### 2. [Filecoin](https://filecoin.io)

Filecoin is a cryptocurrency powered storage network. Miners earn Filecoin by providing open hard-drive space to the network, while users spend Filecoin to store their files encrypted in the decentralized network.

### 3. [libp2p](https://libp2p.io)

libp2p is a modular networking stack. libp2p brings together a variety of transports and peer-to-peer protocols, making it easy for developers to build large, robust p2p networks.

### 4. [IPLD](https://ipld.io)

IPLD is the data model for the Decentralized Web. It connects all data through cryptographic hashes, and makes it easy to traverse and link to.

### 5. [Multiformats](http://multiformats.io/)

The Multiformats Project is a collection of protocols to future-proof systems, today. Self-describing formats make your systems interoperable and upgradable.


IPFS的主要价值，按照现在的商业的里面的行业来讲的话，它的主要价值其实是三块。一个是云存储，一个是CDN，一个是云服务器。

参与到这个项目，目前最主要的一种方式，就是挖矿，也是目前大家最关注的一个参与方式。

第二个参与的机会就是做服务平台，目前可以基于这个东西去做一些开发框架，例如开发服务平台、存储服务平台、检索服务平台。

第三块，是应用项目，比挖矿的难度会高，但是比服务平台难度低。包括存储类的，例如基于IPFS去做一个云盘。

## Chapter 2. IPFS Framework

IPFS 是由一个八层协议栈构成的架构。从底层到上层分别是：

- 身份       S/Kademlia生成     对等身份信息生成

- 网络       任意传输层协议     ICE NET & NAT穿透

- 路由       分布式松散哈希表（DSHT）    定位对等点和存储对象需要的信息

- 交换       BitTorrent& BitSwap     管理区块如何分布

- 对象       Merkle-DAG     内容可寻址的不可篡改、去冗余的对象链接

- 文件       类似Git 版本控制的文件系统：blob,list, tree, commit

- 命名       具有SFS（Self-CertifiedFilesystems）IPNS：DAG对象命名可变

- 应用       在IPFS上运行的应用程序利用最近节点提供服务提供效率、降低成本

### 1. 身份层和路由层

对等节点身份信息的生成以及路由规则是通过Kademlia协议生成制定，KAD协议实质是构建了一个分布式松散Hash表（distributedhash table），简称DHT，每个加入这个DHT网络的人都要生成自己的身份信息，然后才能通过这个身份信息去负责存储这个网络里的资源信息和其他成员的联系信息。

### 2. 网络层

LibP2P可以支持任意传输层协议。

![image](https://github.com/xStorage/documents/raw/master/Resource/libp2p)

ICE NAT traversal框架整合STUN、TURN和其他类型的NAT协议，该框架可以让客户端利用各种NAT方式打通网络，从而完成NAT通信，这对于IPFS的p2p网络非常重要。

### 3. 交换层

类似迅雷这样的BT工具，IPFS团队把BitTorrent进行了创新，叫作Bitswap，它增加了信用和帐单体系来激励节点去分享，用户在发送给其他节点数据可以增加信用值，从其他节点接受数据降低信用值。如果用户只去接收数据而不分享数据，信用分会越来越低而被其他节点忽略掉。

### 4. 对象层和文件层

共同管理IPFS上80%的数据结构。

大部分数据对象都是以MerkleDag的结构存在，这为内容寻址和去重提供了便利。
文件层是一个新的数据结构，和DAG并列，采用Git一样的数据结构来支持版本快照。

### 5. 命名层

具有自我验证的特性（当其他用户获取该对象时，使用指纹公钥进行验签，即验证所用的公钥是否与NodeId匹配，这验证了用户发布对象的真实性，同时也获取到了可变状态），并且加入了IPNS这个巧妙的设计来使得加密后的DAG对象名可定义，增强可阅读性。

### 6. 应用层

IPFS核心价值就在于上面运行的应用程序，可以利用它类似CDN的功能，在成本很低的带宽下，去获得想要的数据，从而提升整个应用程序的效率。

## Chapter 3. IPFS 源码解析
