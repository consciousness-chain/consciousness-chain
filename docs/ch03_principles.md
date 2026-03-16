Chapter 3: Technical Principles / 第3章 技术原理

---

## 3.1 Consciousness Data Acquisition and Preprocessing / 意识数据的采集与预处理

**English**  
Consciousness acquisition consists of three steps:

1. **Raw signal reading**: BCI reads neuron electrical signals at 1kHz sampling rate
2. **Connectome reconstruction**: Reconstructs neural connections based on signal correlation
3. **Downsampling and compression**: Compresses high-dimensional connection matrix into storable format (approx. 100:1 compression ratio)

**Technical challenge**: Consciousness continuity must be maintained during acquisition. Interruptions longer than 500ms result in "fragmentation" — gaps in memory continuity.

**中文**  
意识采集分为三个步骤：

1. **原始信号读取**：脑机接口以 1kHz 采样率读取神经元电信号
2. **连接组重建**：根据信号相关性重建神经元之间的连接关系
3. **降采样与压缩**：将高维连接矩阵压缩为可存储格式（压缩率约 100:1）

**技术挑战**：采集过程中必须保持意识连续性，中断超过 500ms 将导致“断片”现象——记忆连续性的断裂。

**Internal note:** *2471 claimed he could maintain continuity through a 2-second interruption. No one believed him. He didn't bother proving it.*

**内部注：** *2471 声称他能承受 2 秒的中断而保持连续性。没人相信。他也懒得证明。*

---

## 3.2 Consciousness Data Serialization Format (.cns) / 意识数据的序列化格式

**English**  
The `.cns` format uses Protocol Buffer serialization:

```protobuf
syntax = "proto3";

message ConsciousnessBlock {
  string version = 1;                // Format version (current v2.1.0)
  string consciousness_id = 2;        // Unique consciousness identifier (0x-prefixed)
  uint64 timestamp = 3;               // Timestamp (Unix nanoseconds)
  uint64 block_height = 4;             // Block height
  bytes prev_hash = 5;                // Previous block hash (SHA3-512)
  bool is_incremental = 6;            // Whether this is an incremental upload
  
  message Neuron {
    uint32 id = 1;                    // Neuron ID (1-based)
    repeated uint32 connections = 2;   // Connected neuron IDs
    float strength = 3;                // Connection strength (0.0 - 1.0)
    string type = 4;                   // Neuron type (sensory/motor/interneuron)
  }
  repeated Neuron neurons = 7;
  
  message Memory {
    uint64 id = 1;                     // Memory ID
    string content_hash = 2;            // IPFS hash of memory content
    float emotional_valence = 3;        // Emotional valence (-1.0 to 1.0)
    uint32 importance = 4;              // Importance (1-10)
    uint64 timestamp = 5;                // Time when memory occurred
    repeated uint64 related_memories = 6; // Related memory IDs
  }
  repeated Memory memories = 8;
  
  bytes personality_vector = 9;         // Personality trait vector (128-dim float)
  float consciousness_age = 10;          // Consciousness duration (seconds)
  uint32 activity_score = 11;            // Activity score (0-100)
  bytes signature = 12;                  // Consciousness signature (ED25519)
  
  message ContinuityProof {
    bytes prev_state_root = 1;           // Previous state root hash
    float continuity_score = 2;           // Continuity score (0.0-1.0)
    repeated uint32 changed_neurons = 3;  // IDs of changed neurons
  }
  ContinuityProof continuity = 13;
}
中文
.cns 格式采用 Protocol Buffer 序列化，结构如上。

Internal note: *2471 insisted on 1-based neuron IDs. Everyone else wanted 0-based. He won. No one knows how.*

内部注： 2471 坚持神经元 ID 从 1 开始计数。其他人都想从 0 开始。他赢了。没人知道怎么赢的。

3.3 Hash Algorithm and Digital Fingerprint / 哈希算法与数字指纹
English
The Consciousness Chain uses SHA3-512 for all hashing operations:

Block hashes

Merkle tree roots

Digital fingerprints of consciousness data

Continuity proofs

Each consciousness block generates a unique fingerprint:

text
fingerprint = SHA3-512(
  consciousness_id + 
  prev_hash + 
  timestamp + 
  state_root
)
This fingerprint serves as:

Block identifier

Tamper evidence

Quick comparison for duplicate uploads

中文
意识链采用 SHA3-512 进行所有哈希运算：

区块哈希

Merkle 树根

意识数据的数字指纹

连续性证明

每个意识区块生成唯一的数字指纹，作为区块标识、防篡改证据和重复上传的快速比对依据。

Internal note: *2471 proposed using BLAKE3 instead. Reason: "SHA3 is too slow for real-time consciousness streaming." Rejected due to hardware compatibility. His comment: "You'll see."*

内部注： 2471 曾提议改用 BLAKE3。理由：“SHA3 对于实时意识流来说太慢。”因硬件兼容性问题被驳回。他的回应：“你们会看到的。”

3.4 Distributed Storage Solution / 分布式存储方案
English
Consciousness data is stored off-chain using an enhanced IPFS implementation:

Enhancements:

Hot/cold separation: Recent data hot, historical data cold

Redundancy policy: Important consciousnesses automatically get more copies

Geographic affinity: Stored near the consciousness's origin

Storage hierarchy:

Layer	Storage Type	Access Speed	Cost	Retention
L1	Hot cache (RAM)	<10ms	High	7 days
L2	Warm storage (SSD)	<100ms	Medium	30 days
L3	Cold storage (HDD/Archive)	>1s	Low	>30 days
L4	Eternal storage (Blockchain anchor)	N/A	Fixed	Permanent
中文
采用增强版 IPFS 进行链下存储：

增强特性：

冷热分离：近期数据热存，历史数据冷存

冗余策略：重要意识自动增加副本数

地域亲和：优先存储在靠近意识来源地的节点

存储层次如上表。

Internal note: 2471 designed the cold storage archiving algorithm. He called it "The Glacier." No one knows why.

内部注： 冷存储归档算法由 2471 设计。他称之为“冰川”。没人知道为什么。

3.5 Timestamp and Block Generation Mechanism / 时间戳与区块生成机制
English
Blocks are generated based on consciousness activity, not fixed intervals:

High activity: multiple blocks per second

Low activity: hours between blocks

Dormant: no new blocks until activity resumes

Timestamp precision: Nanoseconds (Unix epoch)

Block structure:

text
Block {
  height: uint64
  timestamp: uint64 (nanoseconds)
  prev_hash: bytes32
  state_root: bytes32
  tx_root: bytes32
  signature: bytes64
}
中文
区块生成基于意识活动频率，非固定间隔：

高活跃度：每秒多个区块

低活跃度：数小时一个区块

休眠状态：无新区块，直至活动恢复

时间戳精度：纳秒级（Unix 纪元）

区块结构如上。

Internal note: 2471 argued for timestamp quantization to prevent temporal attacks. Team decided it's "future problem." His response: "Fine. But you'll see."

内部注： 2471 主张对时间戳进行量化以防止时间攻击。团队认为这是“未来的问题”。他的回应：“行。但你们会看到的。”

3.6 Continuity Proof Algorithm / 连续性证明算法
English
Continuity proofs ensure that consciousness state evolves naturally, without jumps or gaps.

Algorithm:

Compare current state with previous state

Calculate delta vector (changes in neuron connections/memories)

Validate that delta is within biologically plausible range

Generate continuity score (0.0 - 1.0)

Score interpretation:

Score Range	Meaning	Action
0.95 - 1.00	Continuous	Accept
0.80 - 0.94	Minor drift	Warning
0.60 - 0.79	Possible fork	Review required
< 0.60	Discontinuity	Reject, require full upload
中文
连续性证明确保意识状态自然演化，无跳跃或断层。

算法步骤：

比较当前状态与前一状态

计算变化向量（神经元连接/记忆的变化）

验证变化是否在生物学合理范围内

生成连续性评分（0.0 - 1.0）

评分解释如上表。

Internal note: 2471's implementation (commit 2025.04.18 23:47) uses a different scoring model. It's in the codebase but not active. Commit message: "You'll see."

内部注： 2471 的实现（提交于 2025.04.18 23:47）使用了不同的评分模型。代码库里留着但未启用。”
