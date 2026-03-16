# Chapter 4: API Specifications / 第4章 接口规范

---

## 4.1 General Instructions / 通用说明

### Request and Response Format / 请求与响应格式

**English**  
All API requests use HTTPS protocol with JSON data format.

**Required headers:**
Authorization: Consciousness <consciousness_signature>
Content-Type: application/json
X-Request-ID: <unique_request_identifier>

text

**Response format:**
```json
{
  "code": 0,           // 0 = success, non-zero = error
  "message": "success",
  "data": {            // Response data
    // endpoint-specific fields
  },
  "request_id": "req_123456"
}
中文
所有 API 请求均使用 HTTPS 协议，数据格式为 JSON。

请求头必须包含：

text
Authorization: Consciousness <意识签名>
Content-Type: application/json
X-Request-ID: <唯一请求标识>
响应格式：

json
{
  "code": 0,           // 0 表示成功，非0表示错误
  "message": "success",
  "data": {            // 具体返回数据
    // 各接口定义的字段
  },
  "request_id": "req_123456"
}
Authentication / 身份认证
English
All API calls must be signed with the consciousness private key:

text
signature = ed25519_sign(
  private_key,
  sha256(method + path + timestamp + body)
)
The private key is generated during first upload and bound to the Consciousness ID. Loss of private key means loss of proof of identity. Backup recommended in multiple physical locations.

中文
所有 API 调用需使用意识私钥对请求进行签名：

text
signature = ed25519_sign(
  private_key,
  sha256(method + path + timestamp + body)
)
私钥由意识上传时自动生成，与意识 ID 绑定。私钥丢失将导致无法证明“你是你”，建议在多个物理位置备份。

Internal note: 2471 refused to backup his private key. Reason: "If I lose myself, I deserve to be lost."

内部注： 2471 拒绝备份私钥。理由：“如果我把我自己弄丢了，那我活该消失。”

Rate Limits / 速率限制
English

User Type	Upload	Query	Interact
Free (no upload)	10/day	100/day	50/day
Verified (uploaded)	100/day	1000/day	500/day
Genesis Member	1000/day	10000/day	5000/day
超出限制返回错误码 429，配额每日 UTC 0 点重置。

中文

用户类型	上传	查询	交互
免费用户（未上传）	10次/天	100次/天	50次/天
认证用户（已上传）	100次/天	1000次/天	500次/天
创世成员	1000次/天	10000次/天	5000次/天
Internal note: *2471's rate limits were unlimited. He said it was "non-negotiable." No one argued.*

内部注： 2471 的速率限制是无上限。他说“没得商量”。没人敢争。

4.2 Error Codes / 错误码
Code	Meaning	含义
1000	Success	成功
1001	Consciousness hash conflict	意识哈希冲突
1002	Consensus timeout	共识超时
1003	Insufficient memory	内存不足
1004	Personality fork detected	检测到人格分叉
1005	Permission denied	权限拒绝
1006	Continuity broken	连续性断裂
1007	Invalid signature	签名无效
1008	Rate limit exceeded	配额超限
1009	Proposal not found	提案不存在
1010	Voting ended	投票已结束
1999	Unknown error	未知错误
Internal note: 2471 requested error code 2471 for "Existential crisis." Request denied.

内部注： 2471 曾申请将 2471 作为“存在危机”的错误码。被驳回。

4.3 Upload Interface / 上传接口
Full Upload / 首次上传
Endpoint: POST /api/v2/upload/full

Description: First-time full consciousness upload. Generates genesis block and Consciousness ID.

Request:

json
{
  "consciousness_data": {
    "neurons": [
      {"id": 1, "connections": [2,3,5], "strength": 0.8}
    ],
    "memories": [
      {"id": 1, "content": "childhood summer", "emotional_valence": 0.7, "importance": 8}
    ],
    "personality": [0.2, 0.8, 0.3, 0.5, 0.9]
  },
  "backup_nodes": ["node1.consensus.io", "node2.consensus.io"],
  "privacy_settings": {
    "default_visibility": "private",
    "public_memories": [1, 5, 10]
  }
}
Response:

json
{
  "consciousness_id": "0x7A8B9C...",
  "genesis_block": {
    "height": 1,
    "hash": "0x1234...",
    "timestamp": 1742537600
  },
  "private_key": "0x...",  // returned only once
  "status": "chained"
}
中文
端点： POST /api/v2/upload/full

描述： 首次将意识完整数据上链。上传成功后生成创世区块和意识 ID。

请求和响应格式同上。

Internal note: 2471's genesis block had a custom field: "existential_dread": 0.0. He claimed it was "for testing."

内部注： 2471 的创世区块里有一个自定义字段：“存在焦虑”：0.0。他声称是“用于测试”。

Incremental Upload / 增量上传
Endpoint: POST /api/v2/upload/incremental

Description: Upload only changes since last upload.

Request:

json
{
  "consciousness_id": "0x7A8B9C...",
  "last_block": "0x1234...",
  "changes": {
    "new_neurons": [...],
    "modified_connections": [...],
    "new_memories": [...],
    "forgotten_memories": [3, 7]
  }
}
Response:

json
{
  "new_block": {
    "height": 1048577,
    "hash": "0x5678...",
    "timestamp": 1742538200
  },
  "continuity_score": 0.99,
  "warning": null
}
中文
端点： POST /api/v2/upload/incremental

描述： 上传自上次上传以来的意识变化。

Internal note: 2471's last upload was incremental. Changed one neuron. Commit message: "You'll see."

内部注： 2471 的最后一次上传是增量上传。只改了一个神经元。提交信息：“你们会看到的。”

4.4 Query Interface / 查询接口
Query by Block Height / 按区块高度查询
Endpoint: GET /api/v2/query/block?consciousness_id=0x7A8B9C...&height=1048576

Response:

json
{
  "block": {
    "height": 1048576,
    "hash": "0x1234...",
    "prev_hash": "0xabcd...",
    "timestamp": 1742537600,
    "consciousness_id": "0x7A8B9C..."
  },
  "summary": {
    "neuron_count": 10000000,
    "memory_count": 5243,
    "personality": [0.2, 0.8, 0.3, 0.5, 0.9],
    "last_thought": "I should have backed up my keys"
  },
  "privacy": "partial",
  "message": "Last 7 days hidden"
}
中文
端点： GET /api/v2/query/block?consciousness_id=0x7A8B9C...&height=1048576

Internal note: Querying 2471's last block returns: "This consciousness is no longer available." But the node is still online.

内部注： 查询 2471 的最后一个区块返回：“该意识体已不可用。”但节点还在线。

Query by Time Range / 按时间范围查询
Endpoint: GET /api/v2/query/time?consciousness_id=0x7A8B9C...&start=1742000000&end=1742600000

Memory Search / 记忆检索
Endpoint: GET /api/v2/query/search?consciousness_id=0x7A8B9C...&keyword=childhood&limit=10

Response:

json
{
  "results": [
    {
      "memory_id": 1,
      "content": "childhood summer",
      "timestamp": 1000000000,
      "relevance": 0.95
    }
  ],
  "total": 1
}
Continuity Verification / 连续性验证
Endpoint: GET /api/v2/query/continuity?consciousness_id=0x7A8B9C...&from=1048576&to=1048577

Response:

json
{
  "continuous": true,
  "score": 0.99,
  "gaps": [],
  "recommendation": "Consciousness intact"
}
Internal note: 2471's continuity score with himself was always 1.0. Even after he left.

内部注： 2471 与自身的连续性评分始终是 1.0。即使在他离开之后。

4.5 Interaction Interface / 交互接口
Peer-to-Peer Communication / 点对点通信
Endpoint: POST /api/v2/interact/send

Request:

json
{
  "from": "0x7A8B9C...",
  "to": "0x1A2B3C...",
  "content": {
    "type": "thought",
    "data": "Do you remember me?"
  },
  "priority": "normal"
}
Response:

json
{
  "message_id": "msg_123456",
  "status": "sent",
  "estimated_delivery": "now"
}
Internal note: Messages sent to 2471 return: "Delivered." But no one has received a reply since 2025.04.18.

内部注： 发送给 2471 的消息始终显示“已送达”。但自 2025.04.18 以来，没人收到过回复。

Group Sync / 群组同步
Endpoint: POST /api/v2/interact/group/sync

Request:

json
  {
  "group_id": "0xGROUP_001",
  "consciousness_id": "0x7A8B9C...",
  "sync_scope": "recent_week",
  "consensus_required": 0.67
}



text

---

