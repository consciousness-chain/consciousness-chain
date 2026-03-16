# Chapter 3: Technical Principles / 第3章 技术原理

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
