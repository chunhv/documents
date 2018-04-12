# xStorage 项目开发计划


**<p align="right">Paul 2018-4-11</p>**
 
***
## 目录
[toc]
***

通过研究区块链和去中心化网络存储等前沿技术，完成xStorage (分布式文件存储系统)产品。

## 1. 目标

基于区块链技术和去中心化网络存储协议（如IPFS），结合相关项目的开源成果，研发一款支持异构平台（集群）上的一款低成本、高安全的p2p的分布式文件存储产品 xStorage。

## 2. 特点

结合研究进展，目前能看到的改进点和特性，系统应具备：

（1）良好的开放性和可伸缩性（存储节点可以自由的进入和移出整个系统）；

（2）数据存储支持匿名（数据用户信息匿名）、加密、冗余和容错；

（3）提供系统级负载均衡、故障检测和恢复机制（例如通过算法保证整个系统中，存储的单位信息块始终保持一定数量）；

（4）开放API接口，支持安装应用扩展（实现类似Google Box可以自动在上面编辑word，ppt，以及运行脚本等，一般通过集成WebDav协议实现）

## 3. 主要工作

IPFS已经提供了一套去中心化的网络存储协议，但要完成上述目标，按功能内容来分，主要工作可以分为以下部分：

### （1）异构平台（集群）移植适配工作

该工作主要是，目前IPFS发布了基于Go语言的Alpha版本，支持Linux，Mac OS，Windows平台，暂不支持Android和IOS，无法构建包含PC、手机、及物联网设备在内的异构集群。而且另一方面，作为提供基础设施的区块链技术，目前在同时存在手机和物联网设备的异构集群上支持也不好，只是刚刚出现。

拟解决思路是，当前IPFS社区正围绕构建Javascript版本的js-ipfs，以及python版本的py-ipfs，即将发布Alpha版本，限于精力，先解决下列问题，等其发布后，利用在手机及物联网设备上搭建Nodejs运行环境运行js-ipfs，并套用区块链在异构设备上的实现方法，构建xStorage的异构平台（集群）。

### （2）开放性和可伸缩性支持工作

该工作主要是，做IPFS和区块链的结合，区块链技术很好的解决了开放性和可伸缩性，IPFS的去中心化网络存储协议，与区块链结合具有结构上的天然优势，目前IPFS官方团队宣称已经在ETH（以太坊）上以DApp的方式实现了协议的搭建和运行，但遗憾的这个工作暂未开源，也还没发布相关的实现细节文档，但在ETH或者EOS上开发DApp是有不少参考资料的。

拟解决思路是，按照IPFS社区的调性，是会公开自己的源码和技术方案的，仿照移植，在EOS区块链上搭建IPFS协议；若不公开，自己需要结合区块链基础平台（ETH或者EOS）提供的DApp进行适配开发。

### （3）数据存储支持匿名（数据用户信息匿名）、加密、冗余和容错工作

该工作主要是，在模型和算法层面的工作，也是项目中需要沉下去的工作，指的是保证存储节点信息的私密性和安全性，由于区块链的实现机制，每笔交易的用户信息对系统全节点可见，对于存储系统而言，无法保证数据所属用户的身份信息匿名；ipfs有一定的冗余和容错机制，但并未支持数据加密存储；

拟解决思路，（1）首先熟悉现在常用的数据存储支持匿名（数据用户信息匿名）、加密、冗余和容错的模型和算法；（2）充分考虑结合区块链相关的Merkle Tree、Merkle Directed Acyclic Graph等数据压缩存储的算法，利用某种秘钥算法实现用户身份信息匿名、以及数据的加密（主要是集成，不自己改写算法，因靠算法实现的模型，远没有靠秘钥算法公开实现的模型安全）；（3）对于数据的冗余和容错结合解析的ipfs机制，以及系统级的负载均衡来考虑实现。

### （4）提供系统级负载均衡、故障检测和恢复机制

该工作主要是，在模型和算法层面的工作，也是项目中最需要沉下去的工作，指的是构建的xStorage系统要在系统层面上负责构建负载均衡，并支持故障检测和恢复机制，例如：（1）由于节点的自由进出，带来了网络的动态变化，要构建以算法为规约的始终保持任意单位信息块在系统中保持一定的冗余数量，当检测到低于安全边界时，则执行故障恢复机制等；（2）构建负载均衡时应当考虑，存储内容的节点存放位置应当是与地域和提供带宽等要素紧密关联的；（3）搭建的系统应当支持在各个存储节点上自动升级算法（类似矿机自动升级，但现在矿机仍未能实现跟随代币的软、硬分叉自动升级挖矿算法），来实现对复杂系统故障的检测和恢复处理。

拟解决思路是，（1）首先快速的学习掌握常见的第三代p2p系统中基于DHT（分布式杂凑表）的分布式索引和路由算法，以及区块链中基于Merkle Tree等的压缩存储算法；（2）然后解析IPFS的协议中数据交换层和数据定义层中，索引、路由和存储逻辑和实现算法，对其在整个系统级的负载平衡进行重构和优化，并增加故障检测和处理机制，提供支持主动传播升级算法的机制；（3）最后需要结合区块链基础平台（ETH或者EOS）提供的DApp进行适配开发，主要是要考虑如何将EOS提供的智能合约API接口和xStorage的去中心化的网络存储协议进行结合，是切分到数据交换层和数据定义层（只维护一个共同的DHT表），还是应当将系统的负载均衡、故障检测和处理，以及主动传播升级理念嵌入到智能合约中来实现。

### （5）开放API接口，支持安装应用扩展工作

该工作主要是，参考国内外云存储提供商的提供服务的不同的形式，确定的xStorage应用层之上如何开放的机制，以提供标准API接口以支持云盘上直接安装各种服务扩展，例如：流媒体播放，文件编辑等，如此就可以直接在云上操作，而不是每次都下载后修改完再上传。国外多采用此处方式，国内只有坚果云采用这种方式，百度云等均不开放自己的API。

拟解决思路是，目前云上的应用在云存储上的扩展主要通过WebDav（基于Http协议实现的权限和版本控制协议，使应用可以对WebServer直接读写），目前解决这个问题的有两种思路：（1）仿照WebDav改写IPFSDav协议，使应用扩展通过兼容ipfsDav协议可以直接操作xStorage系统中的文件；（2）在每一个存储节点上构建一个http协议到ipfs协议的中间层，系统直接实现WebDav的协议兼容。两种思路各有悠长，前一种思路目前IPFS社区仍未有精力来做，后一种思路由于不需要深度解析ipfs协议，已经有热心的开发者直接已开源项目的形式构建这层中间层。

（ps：主要工作内容的（3）（4）相对于（1）（2）（5）来讲，这部分内容是定位到模型和算法上的，（1）（2）（5）也有很多问题要解决，但更多的都是机制已经较为明晰，难度在代码开发上，考虑到我个人精力有限和这部分代码开发和移植涉及的平台和语言太多，需要的精力太大，而且相关的开源社区一直在推进自己的代码扩展和平台兼容等工作，几乎一个月是一个状态，所以我的应对策略是先把精力放在模型和算法上，而不是优先花精力解决代码问题，之前我说的就是这个意思。）

## 4. 计划

之前确实想的是实现目标和特点会随着新技术的涌现和自身对协议的解析的深入了解会变，所以只有逻辑上的步骤计划，在项目xStorage目标和特性（系统边界）为上述不变的基本假设下，计划安排如下：


<p class=MsoNormal align=right style='text-align:right'><span lang=EN-US>&nbsp;</span></p>

<div align=center>

<table class=MsoTableGrid border=1 cellspacing=0 cellpadding=0 width=0
 style='width:766.55pt;border-collapse:collapse;border:none'>
 <thead>
  <tr style='page-break-inside:avoid;height:31.75pt'>
   <td width=75 style='width:56.45pt;border:solid windowtext 1.0pt;padding:
   0cm 5.4pt 0cm 5.4pt;height:31.75pt'>
   <p class=MsoNormal align=right style='text-align:right'><b>内容</b></p>
   <p class=MsoNormal><b>日期</b></p>
   </td>
   <td width=104 style='width:78.0pt;border:solid windowtext 1.0pt;border-left:
   none;padding:0cm 5.4pt 0cm 5.4pt;height:31.75pt'>
   <p class=MsoNormal align=center style='text-align:center'><b>（<span
   lang=EN-US>1</span>）异构平台移植适配</b></p>
   </td>
   <td width=113 style='width:3.0cm;border:solid windowtext 1.0pt;border-left:
   none;padding:0cm 5.4pt 0cm 5.4pt;height:31.75pt'>
   <p class=MsoNormal align=center style='text-align:center'><b>（<span
   lang=EN-US>2</span>）开放性和可伸缩性支持</b></p>
   </td>
   <td width=227 style='width:6.0cm;border:solid windowtext 1.0pt;border-left:
   none;padding:0cm 5.4pt 0cm 5.4pt;height:31.75pt'>
   <p class=MsoNormal align=center style='text-align:center'><b>（<span
   lang=EN-US>3</span>）数据存储支持匿名（数据用户信息匿名）、加密、冗余和容错</b></p>
   </td>
   <td width=198 style='width:148.8pt;border:solid windowtext 1.0pt;border-left:
   none;padding:0cm 5.4pt 0cm 5.4pt;height:31.75pt'>
   <p class=MsoNormal align=center style='text-align:center'><b>（<span
   lang=EN-US>4</span>）提供系统级负载均衡、故障检测和恢复机制</b></p>
   </td>
   <td width=142 style='width:106.35pt;border:solid windowtext 1.0pt;
   border-left:none;padding:0cm 5.4pt 0cm 5.4pt;height:31.75pt'>
   <p class=MsoNormal><b>（<span lang=EN-US>5</span>）开放<span lang=EN-US>API</span>接口，支持安装应用扩展</b></p>
   </td>
   <td width=162 style='width:121.8pt;border:solid windowtext 1.0pt;border-left:
   none;padding:0cm 5.4pt 0cm 5.4pt;height:31.75pt'>
   <p class=MsoNormal align=center style='text-align:center'><b>结果<span
   lang=EN-US>/</span>或方案<span lang=EN-US>/</span>或思维<span lang=EN-US>/</span>或成果的物化体现</b></p>
   </td>
  </tr>
 </thead>
 <tr style='page-break-inside:avoid'>
  <td width=75 style='width:56.45pt;border:solid windowtext 1.0pt;border-top:
  none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center'><b><span
  lang=EN-US>3</span>月</b></p>
  </td>
  <td width=104 style='width:78.0pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center'><span lang=EN-US>-</span></p>
  </td>
  <td width=113 style='width:3.0cm;border-top:none;border-left:none;border-bottom:
  solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center'><span lang=EN-US>-</span></p>
  </td>
  <td width=227 style='width:6.0cm;border-top:none;border-left:none;border-bottom:
  solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center'><span lang=EN-US>-</span></p>
  </td>
  <td width=198 style='width:148.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center'><span lang=EN-US>-</span></p>
  </td>
  <td width=142 style='width:106.35pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center'><span lang=EN-US>-</span></p>
  </td>
  <td width=162 style='width:121.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>1</span>）确定了研发<span
  lang=EN-US>xStorage</span>产品构想，并调研相关技术和项目现状，评估必要性与可行性，梳理细化处<span lang=EN-US>xStorage</span>在开源项目的基础上的增量和区别。</p>
  </td>
 </tr>
 <tr style='page-break-inside:avoid'>
  <td width=75 style='width:56.45pt;border:solid windowtext 1.0pt;border-top:
  none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center;layout-grid-mode:
  char'><b><span lang=EN-US>4</span>月</b></p>
  </td>
  <td width=104 style='width:78.0pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>js-ipfs</span>、<span lang=EN-US>py-ipfs</span>进展；</p>
  </td>
  <td width=113 style='width:3.0cm;border-top:none;border-left:none;border-bottom:
  solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>IPFS</span>社区在<span lang=EN-US>ETH</span>上的系统的开源化工作进展；</p>
  </td>
  <td width=227 style='width:6.0cm;border-top:none;border-left:none;border-bottom:
  solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）学习现代对称<span lang=EN-US>\</span>非对称式加密算法；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>2</span>）学习<span lang=EN-US>Kademlia DHT</span>、<span lang=EN-US>S
  Kademlia DHT</span>、<span lang=EN-US>Merkle Tree</span>、<span lang=EN-US>Merkle
  Directed Acyclic Graph</span>等<span lang=EN-US>p2p</span>系统和区块链常用的数据分布式检索和路由以及存储算法；</p>
  </td>
  <td width=198 style='width:148.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）快速学习《分布式系统：概念和设计》、《云计算与分布式系统：从并行处理到物联网》；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>2</span>）学习区块链中<span lang=EN-US>POF</span>、<span lang=EN-US>POS</span>、<span
  lang=EN-US>DPOS</span>解决拜占庭容错的身份一致性算法；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>3</span>）利用云主机搭建<span lang=EN-US>ipfs</span>，接入<span lang=EN-US>ipfs</span>公网测试环境运行；</p>
  </td>
  <td width=142 style='width:106.35pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>WebDav</span>在<span lang=EN-US>ipfs</span>上的移植和兼容工作；</p>
  </td>
  <td width=162 style='width:121.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>1</span>）整理出可以用来构建<span
  lang=EN-US>xStorage</span>核心存储协议的相关技术笔记</p>
  <p class=MsoNormal align=left style='text-align:left'><span lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>2</span>）编译<span
  lang=EN-US>ipfs</span>源码，搭建接入<span lang=EN-US>ipfs</span>公网的测试环境，跑通<span
  lang=EN-US>ipfs</span>重要组件，抽取出组件之间的模块关系图（功能组件粒度）</p>
  </td>
 </tr>
 <tr style='page-break-inside:avoid'>
  <td width=75 style='width:56.45pt;border:solid windowtext 1.0pt;border-top:
  none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center;layout-grid-mode:
  char'><b><span lang=EN-US>5</span>月</b></p>
  </td>
  <td width=104 style='width:78.0pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>js-ipfs</span>、<span lang=EN-US>py-ipfs</span>进展；</p>
  </td>
  <td width=113 style='width:3.0cm;border-top:none;border-left:none;border-bottom:
  solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>IPFS</span>社区在<span lang=EN-US>ETH</span>上的系统的开源化工作进展；</p>
  </td>
  <td width=227 style='width:6.0cm;border-top:none;border-left:none;border-bottom:
  solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）总结提出数据存储支持匿名（数据用户信息匿名）、加密的方案；</p>
  </td>
  <td width=198 style='width:148.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）从公网上剥离，利用云主机搭建<span lang=EN-US>ipfs</span>私网测试运行环境；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>2</span>）解析<span lang=EN-US>ipfs</span>开源代码，分析<span lang=EN-US>ipfs</span>；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>3</span>）总结提出考虑系统级负载均衡的数据存储的冗余和恢复机制；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>4</span>）学习区块链中软、硬分叉后的升级处理办法，以及不同链间进行数据交换的侧链相关研究（直觉认为这里面是解决主动传播的自动升级策略的相关解决思路）；</p>
  </td>
  <td width=142 style='width:106.35pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>WebDav</span>在<span lang=EN-US>ipfs</span>上的移植和兼容工作；</p>
  </td>
  <td width=162 style='width:121.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>1</span>）搭建少量节点的<span
  lang=EN-US>ipfs</span>私网的测试环境，解析<span lang=EN-US>ipfs</span>开源代码，代码做注释标注至函数级，抽取了模块关系图（代码类粒度）；</p>
  <p class=MsoNormal align=left style='text-align:left'><span lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>2</span>）提出<span
  lang=EN-US>xStorage</span>实现数据存储中用户匿名和加密的模型和算法；</p>
  <p class=MsoNormal align=left style='text-align:left'><span lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>3</span>）提出<span
  lang=EN-US>xStorage</span>支持数据冗余和恢复的系统级负载均衡机制；</p>
  </td>
 </tr>
 <tr style='page-break-inside:avoid'>
  <td width=75 style='width:56.45pt;border:solid windowtext 1.0pt;border-top:
  none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center;layout-grid-mode:
  char'><b><span lang=EN-US>6</span>月</b></p>
  </td>
  <td width=104 style='width:78.0pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>js-ipfs</span>、<span lang=EN-US>py-ipfs</span>进展；</p>
  </td>
  <td width=113 style='width:3.0cm;border-top:none;border-left:none;border-bottom:
  solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）熟悉<span lang=EN-US>EOS</span>上智能合约实现原理和<span lang=EN-US>DApp</span>开发机制；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>2</span>）结实<span lang=EN-US>EOS</span>上开发智能合约的兴趣小组</p>
  </td>
  <td width=425 colspan=2 style='width:318.9pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）针对核心存储部分拿出自己的解决模型（协议），提出一体化的综合模型：系统级负载均衡、故障检测和恢复机制，以及数据存储支持匿名（数据用户信息匿名）、加密、冗余和容错方法；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>2</span>）搭建核心存储协议的模拟仿真测试环境，测试协议的可行性和性能效果，将测试结果与主流产品常用的存储协议的模型和算法进行比较；</p>
  </td>
  <td width=142 style='width:106.35pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>WebDav</span>在<span lang=EN-US>ipfs</span>上的移植和兼容工作；</p>
  </td>
  <td width=162 style='width:121.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>1</span>）提出整个<span
  lang=EN-US>xStorage</span>产品的核心存储协议的模型和算法；</p>
  <p class=MsoNormal align=left style='text-align:left'><span lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>2</span>）并编写模拟仿真测试程序，给出仿真的测试结果，并与主流产品常用的存储协议的模型和算法做对比；</p>
  <p class=MsoNormal align=left style='text-align:left'><span lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>3</span>）在区块链<span
  lang=EN-US>EOS</span>上开发<span lang=EN-US>DApp</span>的方法笔记；</p>
  </td>
 </tr>
 <tr style='page-break-inside:avoid'>
  <td width=75 style='width:56.45pt;border:solid windowtext 1.0pt;border-top:
  none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center;layout-grid-mode:
  char'><b><span lang=EN-US>7-8</span>月</b></p>
  </td>
  <td width=104 style='width:78.0pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>js-ipfs</span>、<span lang=EN-US>py-ipfs</span>进展；</p>
  </td>
  <td width=539 colspan=3 style='width:403.95pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）<span lang=EN-US>xStorage</span>核心协议在<span lang=EN-US>EOS</span>区块链平台上实现（可能需要其他<span
  lang=EN-US>C++</span>开发者协助，大部分还是自己来实现，这个时候才进入代码的密集开发阶段）；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>2</span>）提出面向<span lang=EN-US>PC</span>平台的<span lang=EN-US>xStorage</span>的产品的架构模型、确定实现机制，并就其中的某些细节与相关兴趣小组广泛讨论（如：区块链平台上主动传播的自动升级策略等）；</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  </td>
  <td width=142 style='width:106.35pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>WebDav</span>在<span lang=EN-US>ipfs</span>上的移植和兼容工作；</p>
  </td>
  <td width=162 style='width:121.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>1</span>）在区块链<span
  lang=EN-US>EOS</span>上实现<span lang=EN-US>xStorage</span>核心存储协议；</p>
  <p class=MsoNormal align=left style='text-align:left'><span lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>2</span>）给出整个<span
  lang=EN-US>xStorage</span>全系统架构方案，并细化到各个部分的模型和算法级别；</p>
  </td>
 </tr>
 <tr style='page-break-inside:avoid'>
  <td width=75 style='width:56.45pt;border:solid windowtext 1.0pt;border-top:
  none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center;layout-grid-mode:
  char'><b><span lang=EN-US>9-10</span>月</b></p>
  </td>
  <td width=643 colspan=4 style='width:17.0cm;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）测试面向<span lang=EN-US>PC</span>平台的建设在<span lang=EN-US>EOS</span>上的<span
  lang=EN-US>xStorage</span>原型<span lang=EN-US>Demo</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>2</span>）根据<span lang=EN-US>js-ipfs</span>、<span lang=EN-US>py-ipfs</span>研究进展，提出异构平台的解决方案，很可能就是<span
  lang=EN-US>nodejs</span>上跑<span lang=EN-US>js-ipfs</span>，<span lang=EN-US>pc</span>端的<span
  lang=EN-US>nodejs</span>跑<span lang=EN-US>js-ipfs</span>改成<span lang=EN-US>js-xStorage</span>工作自己做，面向<span
  lang=EN-US>Andorid</span>，<span lang=EN-US>IOS</span>，树莓派等开发板的就是威客网上找个外包做移植封装。</p>
  </td>
  <td width=142 style='width:106.35pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）关注跟踪<span lang=EN-US>WebDav</span>在<span lang=EN-US>ipfs</span>上的移植和兼容工作；</p>
  </td>
  <td width=162 style='width:121.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>1</span>）完成支持<span
  lang=EN-US>PC</span>平台的建设在<span lang=EN-US>EOS</span>区块上的<span lang=EN-US>xStorage</span>产品原型程序，并进行功能测试；</p>
  <p class=MsoNormal align=left style='text-align:left'><span lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>2</span>）给出适配异构平台的解决方案；</p>
  </td>
 </tr>
 <tr style='page-break-inside:avoid'>
  <td width=75 style='width:56.45pt;border:solid windowtext 1.0pt;border-top:
  none;padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=center style='text-align:center;layout-grid-mode:
  char'><b><span lang=EN-US>11-12</span>月</b></p>
  </td>
  <td width=784 colspan=5 style='width:588.3pt;border-top:none;border-left:
  none;border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>1</span>）测试异构平台（集群）上<span lang=EN-US>xStorage</span>产品原型，根据结果优化核心存储协议和模型</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>2</span>）根据<span lang=EN-US>WebDav</span>在<span lang=EN-US>ipfs</span>上的移植和兼容工作，提出开放系统<span
  lang=EN-US>API</span>接口，支持安装应用扩展的方案思路，并代码实现</p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'><span
  lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left;layout-grid-mode:char'>（<span
  lang=EN-US>3</span>）开始找资金支持建立测试环境（花费主要是设备和<span lang=EN-US>EOS</span>代币），开始上规模节点数量的测试环境，测试迭代优化后的版本，定为<span
  lang=EN-US>Alpha</span>版的<span lang=EN-US>xStorage</span></p>
  </td>
  <td width=162 style='width:121.8pt;border-top:none;border-left:none;
  border-bottom:solid windowtext 1.0pt;border-right:solid windowtext 1.0pt;
  padding:0cm 5.4pt 0cm 5.4pt'>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>1</span>）完成异构平台（集群）上<span
  lang=EN-US>xStorage</span>产品原型程序，并进行功能测试；</p>
  <p class=MsoNormal align=left style='text-align:left'><span lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>2</span>）完成<span
  lang=EN-US>xStorage</span>上支持<span lang=EN-US>WebDav</span>协议的方案，给出<span
  lang=EN-US>xStorage</span>上安装应用扩展的方案；</p>
  <p class=MsoNormal align=left style='text-align:left'><span lang=EN-US>&nbsp;</span></p>
  <p class=MsoNormal align=left style='text-align:left'>（<span lang=EN-US>3</span>）发布<span
  lang=EN-US>xStorage</span>产品<span lang=EN-US>Alpha</span>版（测试版）</p>
  </td>
 </tr>
</table>

</div>

<p class=MsoNormal><span lang=EN-US>&nbsp;</span></p>

</div>

