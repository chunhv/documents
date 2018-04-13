# xStorage 关键技术

**<p align="right">Paul 2018-3-19</p>**
 
***
## 目录
[toc]
***

## Chapter 1、Preliminary Book Knowleadge

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

分布式系统的负载平衡重点：优化相邻节点间的交互、适应高度动态的主机可用性、在具有不同信任体系的环境下保持数据的安全性、匿名、可否认能力和对审查的抵抗

路由覆盖的主要任务：路由请求给对象、插入对象、删除对象、节点的增加和移除


## Chapter 2、Preliminary Key Knowleadge

### 1. p2p file system

- p2p 系统分类：（1）基于目录服务器的p2p系统；（2）非结构化p2p；（3）结构化p2p。
- 第一代P2P文件分享网络，像Napster，依赖于中央数据库来协调网络中的查询，第二代P2P网络，像Gnutella，使用泛滥式查询（query flooding）来查询文件，它会搜索网络中的所有节点，第三代p2p网络使用分散式杂凑表来查询网络中的文件，分散式杂凑表在整个网络中储存资源的位置，这些协议追求的主要目标就是快速定位期望的节点。
- 构建DHT常用的分布式检索和路由算法，在不同系统下往往有各自不同的规则，Can, Chord, Pastry, Tapestry都有自己的规则算法。
- Popular Example：
    Napster，
    Gnutella，
- 前基于 DHT 的代表性的研究项目主要包括麻省理工学院的Chord、加州大学伯克利分校的CAN和Tapestry、以及微软研究院的 Pastry.
- 分布式哈希表系统（Distributed Hash Table，DHT）。DHT的主要思想是：首先，每条文件索引被表示成一个(K,V)对，K称为关键字，可以是文件名（或文件的其他描述信息）的哈希值，V是实际存储文件的节点的IP地址（或节点的其他描述信息）。所有的文件索引条目(即所有的（K,V）对)组成一张大的文件索引哈希表，只要输入目标文件的K值，就可以从这张表中查出所有存储该文件的节点地址。然后，再将上面的大文件哈希表分割成很多局部小块，按照特定的规则把这些小块的局部哈希表分布到系统中的所有参与节点上，使得每个节点负责维护其中的一块。这样，节点查询文件时，只要把查询报文路由到相应的节点即可（该节点维护的哈希表分块中含有要查找的(K,V)对）。这里面有个很重要的问题，就是节点要按照一定的规则来分割整体的哈希表，进而也就决定了节点要维护特定的邻居节点，以便路由能顺利进行。
- P2P计算技术正不断应用到军事领域，商业领域，政府信息，通讯等领域。根据具体应用不同，可以把P2P分为大致以下这些类型：
    - 文件内容共享和下载，例如Napster、Gnutella、eDonkey、eMule、Maze、BT等；
    - 计算能力和存储共享，例如SETI@home、Avaki、Popular Power等；
    - 基于P2P技术的协同与服务共享平台，例如JXTA、Magi、Groove等；
    - 即时通讯工具，包括ICQ、QQ、Yahoo Messenger、MSN Messenger等；
    - P2P通讯与信息共享，例如Skype、Crowds、Onion Routing等；
    - 基于P2P技术的网络电视：沸点、PPStream、 PPLive、 QQLive、 SopCast等。
- DHT这类结构最大的问题是DHT的维护机制较为复杂，尤其是结点频繁加入退出造成的网络波动（Churn）会极大增加DHT的维护代价。DHT所面临的另外一个问题是DHT仅支持精确关键词匹配查询，无法支持内容/语义等复杂查询。
- 不同拓扑结构的P2P网络优缺点
<table border="1" align="center" cellpadding="0" cellspacing="1">
  <tr>
    <td width="124" valign="top"><p align="left">比较标准／拓扑结构</p></td>
    <td width="95" valign="top"><p align="left">中心化拓扑</p></td>
    <td width="92" valign="top"><p align="left">全分布式非结构化拓扑</p></td>
    <td width="81" valign="top"><p align="left">全分布式结构化拓扑</p></td>
    <td width="50" valign="top"><p align="left">半分布式拓扑</p></td>
  </tr>
  <tr>
    <td width="124" valign="top"><p align="left">可扩展性</p></td>
    <td width="95" valign="top"><p align="left">差</p></td>
    <td width="92" valign="top"><p align="left">差</p></td>
    <td width="81" valign="top"><p align="left">好</p></td>
    <td width="50" valign="top"><p align="left">中</p></td>
  </tr>
  <tr>
    <td width="124" valign="top"><p align="left">可靠性</p></td>
    <td width="95" valign="top"><p align="left">差</p></td>
    <td width="92" valign="top"><p align="left">好</p></td>
    <td width="81" valign="top"><p align="left">好</p></td>
    <td width="50" valign="top"><p align="left">中</p></td>
  </tr>
  <tr>
    <td width="124" valign="top"><p align="left">可维护性</p></td>
    <td width="95" valign="top"><p align="left">最好</p></td>
    <td width="92" valign="top"><p align="left">最好</p></td>
    <td width="81" valign="top"><p align="left">好</p></td>
    <td width="50" valign="top"><p align="left">中</p></td>
  </tr>
  <tr>
    <td width="124" valign="top"><p align="left">发现算法效率</p></td>
    <td width="95" valign="top"><p align="left">最高</p></td>
    <td width="92" valign="top"><p align="left">中</p></td>
    <td width="81" valign="top"><p align="left">高</p></td>
    <td width="50" valign="top"><p align="left">中</p></td>
  </tr>
  <tr>
    <td width="124" valign="top"><p align="left">复杂查询</p></td>
    <td width="95" valign="top"><p align="left">支持</p></td>
    <td width="92" valign="top"><p align="left">支持</p></td>
    <td width="81" valign="top"><p align="left">不支持</p></td>
    <td width="50" valign="top"><p align="left">支持</p></td>
  </tr>
</table>
- P2P搜索技术中最重要的研究成果应该是基于Small World理论的非结构化搜索算法和基于DHT的结构化搜索算法。尤其是DHT及其搜索技术为资源的组织与查找提供了一种新的方法，在近年来的P2P研究领域成为热点。

## Chapter 3、Modle & Algorithm

### 1. Chord




### 1. Pastry 系统

#### （1）路由算法：

在一个具有N个参与节点的网格中，每个节点都有一个公钥，安全散列函数计算得到对应的GUID，每个GUID都是128位，平均分布在[0, 2^128-1]，并将这些GUID视为一个环，节点间的消息传递通过将消息传输到一个更接近的目的地，Pastry能够在O(logN)步内完成,路由过程一般使用UDP完成。

每个节点保存和自己GUID数值上接近的2L个节点，将GUID视为一个环，0与2^128-1相连,便可以如此把消息传递到任意GUID：对于任意节点A，当收到目的地是D的消息M时，它首先将自己的GUID和D的GUID比较，然后再将叶子的集合GUID和D的GUID比较，最终，消息M将发往和D的GUID在数值上最接近的节点。

并且每个节点都维护一个树形结构的路由表R，首先路由表一句GUID十六进制数的前缀的不同对其进行分类，记录除自身GUID值以外与其他在同一类别的GUID的IP值，对任意节点A，路由过程都会使用该节点的路由表R和叶子集合L中的信息，并在每次路由时更新路由表。

#### (2) 主机加入

新的节点加入，要向其他节点通知使他们更新自己的路由表。首先通过最近邻居算法等与Patry中最近的节点建立连接，假设新节点的GUID是X，并且他附近的节点为A，节点X发送一个专门的Join请求给A，并且这个消息的目标地址X，节点A按正常的方式通过Pastry分发join消息，Pastry将会把join消息发送到其GUID值与X数值上最接近的已有节点Z上去，节点A、Z以及路由join消息沿途经过的节点，都会在常规的Pastry路由表和叶子集合中进行更新，而X自身的路由表应当与Z的路由表相似，再通过与邻居节点的交互，将优化。这便涉及到容错。

#### （3）主机失效或退出

Pastry节点可能失效或者没有任何的预警退出，当某个节点失效，包含该失效节点的GUID的叶子集合应该修正。当发现某个节点失效是，为修复自身叶子集合L，应该在L中寻找靠近失效节点的某个活节点，然后从那个节点L中获取可以代替失效节点的L'（L的子集），对路由表的修复基于一旦发现不可达才修复机制。

#### （4）其他特性

地域性：高度冗余，路由表利用了传输网络中节点的地域树形（IP跳数或者通信延迟）

容错：需要心跳消息判断活节点，为处理故障和对付怀有恶意节点，可以在算法基础上引入小范围的随机性

可靠性：措施包括路由算法的每一跳使用确认

Pastry发布的升级版MSPastry，模拟测试中表现：

MSPastry 可靠性结果：当IP丢包率为0时，每10万个请求中，只有1.5个失败，丢包率为5%，大约每10万个请求中有4.9个失败。

MSPastry 性能结果：RDP值来度量发生在路由层的额外开销，为下列两个数据的比值，通过路由覆盖传递请求的平均延迟，同样两节点间传递同一消息的平均延迟。0%丢包率RDP=1.8,在5%丢包率下RDP=2.2；

MSPastry 开销结果：额外网络负载少于每分钟两条消息，但存在初始安装的开销

### 2. Tapestry 系统

#### （1）

### Kademlia DHT


### S Kademlia DHT


### Merkle Tree

### Merkle Directed Acyclic Graph

### Pas