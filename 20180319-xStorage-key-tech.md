# xStorage 关键技术

**<p align="right">Paul 2018-3-19</p>**
 
***
## 目录
[toc]
***

## Chapter 1、Preliminary Knowleadge

### 一. 《分布式系统：概念与设计》

#### 1. 分布式系统特征

**概念**：分布式系统是其组件分步在联网的计算机上，组件之间通过传递消息进行通信和动作协调的系统；

**特征**：组件的并发性、缺乏全局时钟、组件故障的独立性；

**挑战**：处理组件的异构性、开放性、安全性、可伸缩性、故障处理、组件的并发性、透明性和提供服务质量；
    
信息资源的安全性包括三个部分：机密性（防止泄露给未授权的个人）、完整性（防止被改变或被破坏）、可用性（防止对访问资源的手段的干扰）；
    
可伸缩分步式系统key problem：控制物理资源的开销、控制性能损失、防止软件资源用尽、避免性能瓶颈；
    
故障处理key problem：检测故障、掩盖故障、容错、故障恢复、冗余；
    
透明性：访问透明性、位置透明性、并发透明性、复制透明性、故障透明性、移动透明性、性能透明性、伸缩透明性
        
#### 2. 系统模型

**模型**：物理模型、体系结构模型、基础模型（交互模型、故障模型、安全模型）

分布式系统的困难和威胁：使用模式的多样性、系统环境的多样性、内部问题、外部威胁



### 1. p2p file system

- p2p 系统分类：（1）基于目录服务器的p2p系统；（2）非结构化p2p；（3）结构化p2p。
- 第一代P2P文件分享网络，像Napster，依赖于中央数据库来协调网络中的查询，第二代P2P网络，像Gnutella，使用泛滥式查询（query flooding）来查询文件，它会搜索网络中的所有节点，第三代p2p网络使用分散式杂凑表来查询网络中的文件，分散式杂凑表在整个网络中储存资源的位置，这些协议追求的主要目标就是快速定位期望的节点。
- 构建DHT常用的分布式检索和路由算法，在不同系统下往往有各自不同的规则，Can, Chord, Pastry, Tapestry都有自己的规则算法。

- Popular Example：
    Napster，
    Gnutella，

### Kademlia DHT


### S Kademlia DHT


### Merkle Tree

### Merkle Directed Acyclic Graph

### 