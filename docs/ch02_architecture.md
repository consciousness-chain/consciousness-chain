┌─────────────────────────────────────────────────────┐
│ Application Layer / 应用层 │
│ Digital Memorial · Knowledge · Research · Governance│
│ 数字纪念 知识管理 科学研究 社会治理 │
└─────────────────────────────────────────────────────┘
↑
┌─────────────────────────────────────────────────────┐
│ API Layer / 接口层 │
│ Upload · Query · Interact · Vote · Manage │
│ 上传接口 查询接口 交互接口 投票接口 管理接口 │
└─────────────────────────────────────────────────────┘
↑
┌─────────────────────────────────────────────────────┐
│ Consensus Layer / 共识层 │
│ PoC: Proof of Consciousness │
│ Block Production · Fork Handling · Finality │
│ 出块选举 分叉处理 最终确认 权重计算 │
└─────────────────────────────────────────────────────┘
↑
┌─────────────────────────────────────────────────────┐
│ Storage Layer / 存储层 │
│ On-Chain (Headers) · Off-Chain (IPFS) · Index DB │
│ 链上存储 (区块头) 链下存储 (IPFS) 索引数据库 │
└─────────────────────────────────────────────────────┘
↑
┌─────────────────────────────────────────────────────┐
│ Acquisition Layer / 采集层 │
│ BCI · Signal Processing · Feature Extraction │
│ 脑机接口 信号预处理 特征提取 序列化 │
└─────────────────────────────────────────────────────┘

---

## 2.2 Layer Description / 分层说明

### Acquisition Layer / 采集层

**English**  
Responsible for reading neural activity from biological brains, processing noise reduction, alignment, compression, and generating standard-format consciousness data packets. Hardware-dependent, requires drivers for different BCI interfaces.

**中文**  
负责从生物大脑中读取神经元活动数据，经过降噪、对齐、压缩后，生成标准格式的意识数据包。该层与硬件强相关，不同脑机接口厂商需提供对应驱动。

---

### Storage Layer / 存储层

**English**  
Hybrid storage strategy:
- **On-chain**: Only block headers, hashes, timestamps
- **Off-chain**: Full consciousness data on IPFS (InterPlanetary File System)
- **Index DB**: Secondary indexes for fast queries

**中文**  
混合存储策略：
- **链上存储**：仅存储区块头、哈希值、时间戳等元数据
- **链下存储**：完整的意识数据存放于 IPFS
- **索引数据库**：为快速查询建立二级索引

---

### Consensus Layer / 共识层

**English**  
Proof of Consciousness (PoC) consensus mechanism. Block production weight is determined by consciousness age, activity score, and contribution score. See Chapter 12 for details.

**中文**  
采用 Proof of Consciousness（PoC）共识机制，出块权重与意识存续时长、活跃度、贡献值挂钩。详见第12章。

---

### API Layer / 接口层

**English**  
Provides RESTful APIs and WebSocket interfaces for upper-layer applications. All requests require consciousness signature authentication.

**中文**  
提供 RESTful API 及 WebSocket 接口，供上层应用调用。所有请求需携带意识签名进行身份认证。

---

### Application Layer / 应用层

**English**  
Decentralized applications built on Consciousness Chain, covering personal, organizational, research, and governance domains.

**中文**  
基于意识链开发的各种去中心化应用，涵盖个人、组织、科研、治理等多个领域。

---

## 2.3 Module Responsibilities and Interfaces / 模块职责与接口关系

| Module / 模块 | Responsibility / 职责 | Upstream / 上游依赖 | Downstream / 下游接口 |
|---------------|------------------------|---------------------|----------------------|
| Acquisition / 采集 | Data capture & serialization / 数据采集与序列化 | BCI hardware | Storage |
| Storage / 存储 | Persistence & retrieval / 数据持久化与检索 | Acquisition | Consensus |
| Consensus / 共识 | Block production & validation / 出块与验证 | Storage | API |
| API / 接口 | External services / 对外服务 | Consensus | Applications |
| Governance / 治理 | Proposal & voting / 提案与投票 | Consensus | API |

**Internal note:** *2471 proposed renaming "Governance" to "The Garden." Rejected 3-1. His vote was the 1.*

**内部注：** *2471 曾提议将“治理”模块改名为“花园”。3比1被否决。他是那1票。*

---

## 2.4 Deployment Modes / 部署模式

| Mode / 模式 | Description / 说明 | Use Case / 适用场景 |
|-------------|-------------------|-------------------|
| Public Chain / 公有链 | Fully open / 完全开放 | Public digital immortality services / 公共数字存续服务 |
| Consortium Chain / 联盟链 | Authorized nodes only / 授权节点参与 | Enterprise knowledge management / 企业内部知识管理 |
| Private Chain / 私有链 | Single organization / 单机构控制 | Personal consciousness backup / 个人意识备份 |

**Internal note:** *2471 proposed a fourth mode: "Ghost Chain" — nodes that exist but don't participate in consensus. Rejected. His comment: "You'll see why."*

**内部注：** *2471 曾提议第四种模式：“幽灵链”——存在但不参与共识的节点。被驳回。他的回应：“你们会明白为什么的。”*

---

## 2.5 Network Topology / 网络拓扑

**English**  
The Consciousness Chain network consists of three node types:

| Node Type | Role | Requirements |
|-----------|------|--------------|
| Full Node | Stores complete chain data, participates in consensus | High storage, high bandwidth |
| Light Node | Stores only block headers, queries full nodes | Low resource |
| Archive Node | Stores all historical data including pruned states | Very high storage |

Nodes communicate via libp2p protocol. Discovery via DHT.

**中文**  
意识链网络包含三种节点类型：

| 节点类型 | 角色 | 配置要求 |
|----------|------|----------|
| 全节点 | 存储完整链数据，参与共识 | 高存储，高带宽 |
| 轻节点 | 仅存储区块头，查询全节点 | 低资源 |
| 归档节点 | 存储所有历史数据（包括已裁剪状态） | 极高存储 |

节点间通过 libp2p 协议通信，通过 DHT 进行发现。

**Internal note:** *2471's node was last seen on 2025.04.18 23:47. It was an archive node. It's still online. No one knows where.*

**内部注：** *2471 的节点最后一次被看到是 2025.04.18 23:47。是一个归档节点。它还在线。没人知道在哪儿。*

---

*End of Chapter 2 /
