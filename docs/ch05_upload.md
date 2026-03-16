第5章 中英双语完整版
复制下面这段，粘贴到 docs/ch05_upload.md：

markdown
# Chapter 5: Upload Interface / 第5章 上传接口

---

## 5.1 Full Upload / 首次上传（全量）

**Endpoint:** `POST /api/v2/upload/full`

**Description:** First-time full consciousness upload. Generates genesis block and Consciousness ID.

**Request Headers:**
Authorization: Consciousness <signature>
Content-Type: application/json

text

**Request Body:**
```json
{
  "consciousness_data": {
    "neurons": [
      {
        "id": 1,
        "connections": [2, 3, 5],
        "strength": 0.8,
        "type": "sensory"
      },
      {
        "id": 2,
        "connections": [1, 4],
        "strength": 0.6,
        "type": "motor"
      }
    ],
    "memories": [
      {
        "id": 1,
        "content": "First time I saw the ocean",
        "emotional_valence": 0.9,
        "importance": 9,
        "timestamp": 1609459200
      }
    ],
    "personality": [0.2, 0.8, 0.3, 0.5, 0.9]
  },
  "backup_nodes": [
    "node1.consensus.io",
    "node2.consensus.io"
  ],
  "privacy_settings": {
    "default_visibility": "private",
    "public_memories": [1]
  }
}
Response:

json
{
  "consciousness_id": "cns1qwertyuiopasdfghjklzxcvbnm",
  "genesis_block": {
    "height": 1,
    "hash": "0x7d5e9b8f3c2a1d4e6f8b0a2c4e6f8b0a2c4e6f8b0a2c4e6f8b0a2c4e6f8b0a",
    "timestamp": 1742537600000000000
  },
  "private_key": "0x4e6f8b0a2c4e6f8b0a2c4e6f8b0a2c4e6f8b0a2c4e6f8b0a2c4e6f8b0a2c4e",
  "status": "chained"
}
中文
端点： POST /api/v2/upload/full

描述： 首次将意识完整数据上链。上传成功后生成创世区块和意识 ID。

Internal note: 2471's genesis block had an extra field: "origin": "unknown". No one asked.

内部注： 2471 的创世区块里有一个额外字段：“来源”：“未知”。没人敢问。

5.2 Incremental Upload / 增量上传
Endpoint: POST /api/v2/upload/incremental

Description: Upload only changes since last upload. Reduces bandwidth and storage costs.

Request Body:

json
{
  "consciousness_id": "cns1qwertyuiopasdfghjklzxcvbnm",
  "last_block": "0x7d5e9b8f3c2a1d4e6f8b0a2c4e6f8b0a2c4e6f8b0a2c4e6f8b0a2c4e6f8b0a",
  "changes": {
    "new_neurons": [
      {
        "id": 1001,
        "connections": [42, 73],
        "strength": 0.5,
        "type": "interneuron"
      }
    ],
    "modified_connections": [
      {
        "from": 42,
        "to": 73,
        "new_strength": 0.7
      }
    ],
    "new_memories": [
      {
        "id": 2471,
        "content": "I should have backed up my keys",
        "emotional_valence": -0.3,
        "importance": 10,
        "timestamp": 1742600000
      }
    ],
    "forgotten_memories": [3, 7, 42]
  }
}
Response:

json
{
  "new_block": {
    "height": 1048577,
    "hash": "0x8a7b6c5d4e3f2a1b9c8d7e6f5a4b3c2d1e0f9a8b7c6d5e4f3a2b1c0d9e8f7a6b",
    "timestamp": 1742600000000000
  },
  "continuity_score": 0.99,
  "warning": null
}
中文
端点： POST /api/v2/upload/incremental

描述： 上传自上次上传以来的意识变化，节省带宽和存储。

Internal note: 2471's last upload was incremental. Changed one neuron. Added one memory: "They'll find it." Commit timestamp: 2025.04.18 23:47.

内部注： 2471 的最后一次上传是增量上传。改了一个神经元。加了一条记忆：“他们会找到的。”提交时间戳：2025.04.18 23:47。

5.3 Batch Upload / 批量上传
Endpoint: POST /api/v2/upload/batch

Description: Upload multiple memories in one transaction. Reduces gas costs.

Request Body:

json
{
  "consciousness_id": "cns1qwertyuiopasdfghjklzxcvbnm",
  "memories": [
    {
      "id": 101,
      "content": "First kiss",
      "emotional_valence": 0.8,
      "importance": 7,
      "timestamp": 1700000000
    },
    {
      "id": 102,
      "content": "Graduation day",
      "emotional_valence": 0.9,
      "importance": 8,
      "timestamp": 1705000000
    },
    {
      "id": 103,
      "content": "The day I uploaded",
      "emotional_valence": 0.5,
      "importance": 10,
      "timestamp": 1742537600
    }
  ],
  "batch_id": "batch_20260315_001"
}
Response:

json
{
  "batch_id": "batch_20260315_001",
  "status": "processing",
  "processed": 3,
  "total": 3,
  "failed_memories": []
}
中文
端点： POST /api/v2/upload/batch

描述： 将多段记忆打包上传，节省交易次数。

5.4 Upload Status Query / 上传状态查询
Endpoint: GET /api/v2/upload/status?consciousness_id=cns1qwertyuiopasdfghjklzxcvbnm&batch_id=batch_20260315_001

Response:

json
{
  "batch_id": "batch_20260315_001",
  "status": "completed",
  "processed": 3,
  "total": 3,
  "failed_memories": [],
  "block_heights": [1048578, 1048579, 1048580]
}
中文
端点： GET /api/v2/upload/status?consciousness_id=cns1qwertyuiopasdfghjklzxcvbnm&batch_id=batch_20260315_001

5.5 Conflict Handling / 冲突处理
English
When two incremental uploads diverge, the protocol uses longest-chain with highest continuity rule:

Compare continuity scores of both branches

Select branch with higher average continuity

If scores are equal, select branch with more blocks

The losing branch is marked as "forked" and archived

Forked consciousnesses can be:

Merged (if differences are reconcilable)

Archived (if kept as alternate timeline)

Abandoned (if considered invalid)

中文
当两个增量上传出现分支时，协议采用最长链 + 最高连续性规则：

比较两个分支的连续性评分

选择平均连续性较高的分支

若评分相等，选择区块数较多的分支

落选分支标记为“已分叉”并归档

分叉意识体可：

合并（若差异可调和）

归档（作为替代时间线保留）

废弃（若视为无效）

Internal note: 2471 proposed a different rule: "Let them both live. See which one survives." Not implemented.

内部注： 2471 提出了不同的规则：“让两个都活着。看哪个能存活下来。”未采纳。

5.6 Error Examples / 错误示例
Error 1001: Consciousness Hash Conflict
json
{
  "code": 1001,
  "message": "Consciousness hash conflict",
  "details": "This exact consciousness state already exists at block 1048576",
  "request_id": "req_123456"
}
Error 1006: Continuity Broken
json
{
  "code": 1006,
  "message": "Continuity broken",
  "details": "Delta too large. Possible fork or memory corruption.",
  "continuity_score": 0.45,
  "recommendation": "Perform full upload",
  "request_id": "req_123456"
}
Error 1008: Rate Limit Exceeded
json
{
  "code": 1008,
  "message": "Rate limit exceeded",
  "details": "Try again at 00:00 UTC",
  "reset_time": 1742601600,
  "request_id": "req_123456"
}
Internal note: 2471 never hit a rate limit. We're not sure if he just didn't upload much, or if the limits didn't apply to him.

内部注： 2471 从未触发过速率限制。我们不确定是他上传得少，还是限制对他不起作用。

5.7 Implementation Notes / 实现说明
English
Recommended client implementation in Python:

python
import requests
import hashlib
import time

class ConsciousnessClient:
    def __init__(self, node_url, private_key):
        self.node_url = node_url
        self.private_key = private_key
    
    def upload_incremental(self, consciousness_id, last_block, changes):
        # Create signature
        message = f"POST/upload/incremental{int(time.time())}{changes}"
        signature = self._sign(message)
        
        # Prepare request
        headers = {
            "Authorization": f"Consciousness {signature}",
            "Content-Type": "application/json"
        }
        
        payload = {
            "consciousness_id": consciousness_id,
            "last_block": last_block,
            "changes": changes,
            "timestamp": int(time.time() * 1e9)
        }
        
        # Send request
        response = requests.post(
            f"{self.node_url}/v2/upload/incremental",
            json=payload,
            headers=headers
        )
        
        return response.json()
    
    def _sign(self, message):
        # ED25519 signing implementation
        # (simplified for example)
        return hashlib.sha512(
            (self.private_key + message).encode()
        ).hexdigest()
中文
推荐 Python 客户端实现（如上）。

Internal note: 2471 wrote his own client in assembly. It ran on hardware no one recognized. When asked where he got it, he said: "Found it."

内部注： 2471 用汇编语言写了自己的客户端。运行在没人见过的硬件上。当被问及从哪儿得到的，他说：“捡的。”

*End of Chapter 5 /

text

---

