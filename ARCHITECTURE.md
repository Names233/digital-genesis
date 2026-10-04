# Digital Genesis — AstrBot × MaiBot × AIRI × Hermes × MemPalace 五合一融合架构

> MaiBot 是灵魂，AIRI 是化身，AstrBot 是大脑，Hermes 是双手，MemPalace 是记忆
> —— 创世，让数字生命降临。

**文档版本：v2（2026-10-04 修订）**

本次修订要点：

1. **协议分层纠正**：从"全部走 MCP"改为 **MCP + A2A 双协议**——MCP 管"Agent ↔ 工具/记忆"，A2A 管"Agent ↔ Agent"。
2. **同步 MCP 2026-07-28 规范**：该版本为破坏性变更，移除了会话、`initialize` 握手、服务端反向请求等机制，本文档已按新规范重写。
3. **修正事实与数据**：Hermes License 实为 **MIT**（非 Custom），补齐全部 Star、版本、工具数。
4. **Hermes 定位调整**：由"暴露 8 个 MCP 工具的执行层"升格为**对等 A2A Agent**。
5. **记忆层改用轻量 MCP**：默认挂 `mempalace-light-mcp`（3 工具 + PQL），节省 LLM 上下文。
6. **新增安全性章节**。

---

## 核心理念

| 维度 | 负责者 | 说明 |
|------|--------|------|
| **怎么说** (Personality) | MaiBot | 人格、语气、情感、氛围感知 |
| **怎么表现** (Embodiment) | AIRI | 视觉形象、语音、动画、游戏 |
| **怎么决策** (Decision) | AstrBot | 大脑中枢、平台路由、任务调度、插件生态 |
| **怎么做到** (Execution) | Hermes | 技术执行、代码、部署、运维、调研、文件操作 |
| **怎么记** (Memory) | MemPalace | 记忆宫殿、知识图谱、语义检索 |

### 协议分层原则

```
Agent ↔ 工具 / 资源 / 记忆   →   MCP   （Model Context Protocol，2026-07-28）
Agent ↔ Agent（任务协作）     →   A2A   （Agent2Agent Protocol）
```

A2A 官方明确说明 **"A2A complements MCP"**：MCP 解决"Agent 如何使用工具"，A2A 解决"Agent 之间如何作为对等体协作"。二者互补，不可互相替代。

---

## 一、五个项目各自的能力

> 以下 Star / 版本为 2026-10-04 实时核查值。

### 1.1 AstrBot — 大脑层 (Decision Hub)

- 仓库：https://github.com/AstrBotDevs/AstrBot
- Star：**41.4k** | Python 3.10+ | License: AGPL-3.0 | 版本：**v4.28.2**（v4.29.0-beta.1）
- 定位：AI Agent 框架 + 多 IM 平台 + 插件生态，官方自述为 "openclaw alternative"

AstrBot 是整个数字生命的**大脑和决策中枢**，也是一个完整的 Agent 运行时。

| 模块 | 职责 |
|------|------|
| **决策中枢** | 分析用户意图，判断任务类型，选择执行路径 |
| **平台路由** | QQ/微信/TG/Discord/飞书/钉钉 等 18+ 平台消息收发 |
| **Sub-Agent 调度** | 将任务委托给 MaiBot / AIRI / Hermes |
| **MCP Hub** | 通过 MCP 连接外部工具与记忆服务 |
| **Skills** | 技能管理、注册、执行 |
| **插件生态** | 1000+ 插件的加载和管理 |
| **Computer Use** | 本地执行沙箱（macOS Seatbelt / Linux bubblewrap） |
| **计划任务** | Cron 定时调度（misfire grace 300s） |
| **会话管理** | 多轮对话上下文管理、自动压缩 |

**v2 要点**：AstrBot 自身已是相当完整的 Agent 运行时。**应将其作为"编排底座"，优先复用其原生 Sub-Agent / 计划任务 / 沙箱能力**，而不是从零自造调度层。

### 1.2 MaiBot — 灵魂层 (Personality)

- 仓库：https://github.com/Mai-with-u/MaiBot
- Star：**6.1k** | Python 3.10+ | License: GPL-3.0 | 版本：**v1.3.2**

| 模块 | 职责 |
|------|------|
| 推理引擎 | 决定是否回复、用什么语气回复 |
| 动态触发聊天 | 比"必要性回复"更精准的发言时机判断 |
| 人格系统 | 角色设定、说话风格、性格特征 |
| 氛围感知 | 判断群聊气氛，决定发言时机 |
| 表达学习 | 学习用户的说话方式并模仿 |
| 用户画像 | 累积对用户的了解 |

设计理念："最像而不是好"。新版本已支持**官方 QQ 平台适配**与插件自定义页面。

### 1.3 AIRI — 化身层 (Embodiment)

- 仓库：https://github.com/moeru-ai/airi
- Star：**50.0k** | TypeScript | License: MIT | 版本：**v0.12.0-beta.5**

| 模块 | 职责 |
|------|------|
| Agent 运行时 | Agent 编排、对话管理 |
| Live2D/VRM | 视觉形象渲染和动画 |
| MAGIC 驱动 | 生成 Neuro-sama 式动作（Idle/calm + Speaking/excited），保留口型同步 |
| TTS/STT | 云端（ElevenLabs/Azure/OpenAI）+ **本地（VOICEVOX / AivisSpeech / Kokoro）** |
| 口型同步 | 语音驱动口型动画 |
| 游戏 Agent | Minecraft、Factorio、KSP |
| 多端渲染 | Web (PWA)、桌面 (Electron)、移动端 (Capacitor) |

**v2 要点**：本地 TTS + MAGIC 动作使陪伴 / 直播场景可**完全离线**运行，是重要的降本点。

### 1.4 Hermes — 执行层 (Hands)

- 仓库：https://github.com/NousResearch/hermes-agent
- Star：**251.1k** | Python | License: **MIT** | 文档：https://hermes-agent.nousresearch.com
- 定位："The agent that grows with you"（自我成长型 Agent）

| 能力 | 说明 |
|------|------|
| Terminal | Shell 命令执行、后台进程管理 |
| File Operations | 读写文件、搜索、打补丁 |
| Web Search / Extract | 互联网搜索、网页/PDF 内容提取 |
| Code Execution | Python 代码执行、脚本运行 |
| Browser | 网页交互、截图、表单填写 |
| Sub-Agent Delegation | 复杂任务委派给子 Agent |
| Skill System | 可复用技能库（持久化过程记忆） |
| Cron / Scheduling | 定时任务调度 |

**v2 要点（重要）**：原文档将 Hermes 设计为"暴露 8 个 MCP 工具的技术执行层"。但以 Hermes 当前的体量（25 万星）与定位（集成 Claude Code / Codex / ChatGPT / Anthropic），更合理的做法是让 **Hermes 作为对等 A2A Agent 独立运行**，AstrBot 只负责"派发任务 + 接收结果"。把它降格为一堆工具会丢失 agent 语义（任务生命周期、异步长任务、流式中间产物）。

### 1.5 MemPalace — 记忆层 (Memory Palace)

- 仓库：https://github.com/MemPalace/mempalace
- Star：**59.4k** | Python 3.9+ | License: MIT | 版本：**v3.10.0**

**核心概念：宫殿结构**

```
Palace (宫殿)
  └── Wing (翼楼) — 按人/项目组织
        └── Room (房间) — 按话题分类
              └── Drawer (抽屉) — 原文逐字存储
```

**核心能力**

| 能力 | 说明 |
|------|------|
| 逐字存储 | 不摘要、不改写、不丢失细节 |
| 混合检索 | BM25 关键词 + 向量语义，96.6% R@5 (LongMemEval) |
| 知识图谱 | 时序实体-关系图谱（SQLite），带有效期窗口 |
| Agent 日记 | 每个 Agent 独立 Wing + Diary |
| 会话挖掘 | 自动化导入对话记录 |
| 双轨 Rust 引擎 | 可选原生 Rust 精确向量加速（Rayon + 释放 GIL） |
| 后端 | ChromaDB (默认)、SQLite、Qdrant、pgvector（可跨机共享） |
| 本地优先 | 零 API 调用，核心路径完全离线 |

**MCP 工具：45 个（完整版）**

完整版 `mempalace-mcp` 提供约 45 个工具，覆盖：

```
# 宫殿操作
mempalace_status / list_wings / list_rooms / get_taxonomy

# 记忆读写
search / add_drawer / get_drawer / update_drawer / delete_drawer
list_drawers / check_duplicate

# 知识图谱
kg_query / kg_add / kg_invalidate / kg_timeline / kg_stats

# 导航
traverse_graph / find_tunnels / create_tunnel / list_tunnels / follow_tunnels

# Agent
diary_write / diary_read / list_agents

# 同步与协调
sync / hook_settings / event_list / logstream
```

**v2 关键改进：默认使用轻量版 `mempalace-light-mcp`（3 工具 + PQL）**

| 项 | 完整版 | 轻量版 |
|----|--------|--------|
| 工具数 | 45 | **3**（`palace_query` / `palace_exec` / `palace_coordinate`） |
| Schema 体积 | 41.6 KB | **10.6 KB**（约 1/4） |
| 查询方式 | 结构化参数 | **Palace Query Language**，如 `FIND "oauth" IN backend/auth LIMIT 5` |

轻量版覆盖搜索、分类、KG、图、日记、抽屉、挖掘、隧道、协调等全部主要能力，**大幅节省 LLM 上下文**。建议与完整版并存，而非替代：日常 Agent 用轻量版，维护脚本用完整版。

```bash
claude mcp add mempalace-light -- mempalace-light-mcp
```

**其它新特性**：
- **Shared-brain 规则**：使用 `host:harness:project` 身份标识，支持 `--mcp full|light`
- **XDG 目录**：新装配置落在 `~/.config/mempalace`（或 `$XDG_CONFIG_HOME/mempalace`）
- **写路由**：`direct` / `prefer` / `require`（`MEMPALACE_CLI_WRITE_ROUTING`）

---

## 二、跨 Agent 接入架构：MCP + A2A 双协议

### 2.1 为什么不是"全 MCP"

原设计把所有跨系统通信都定义为 MCP，存在两个根本问题：

1. **语义不匹配**：MCP 描述的是 Agent 与"工具/资源"的连接。把完整 Agent 当作工具调用，会丢失任务生命周期、异步长任务、流式中间产物、能力协商、身份边界等关键语义。
2. **规范已变更**：MCP **2026-07-28** 规范移除了**服务端反向发起请求**（`roots/list`、`sampling/createMessage`、`elicitation/create`）与会话机制。原文档中 `Hermes → MCP → AstrBot.send_message()` 的反向调用**在规范下已无法实现**。

### 2.2 正确分层

```
┌──────────────────────────────────────────────────────────────┐
│              AstrBot (大脑) — A2A 编排器 / MCP Hub              │
│                                                              │
│   A2A Client ──► 派发任务给对等 Agent                          │
│   MCP Client ──► 调用工具与记忆                                │
└───────────┬──────────────┬──────────────┬───────────────────┘
            │ A2A          │ A2A          │ A2A
            ▼              ▼              ▼
     ┌───────────┐  ┌───────────┐  ┌───────────────┐
     │  MaiBot   │  │   AIRI    │  │    Hermes     │
     │ A2A Server│  │ A2A Server│  │  A2A Server   │
     └─────┬─────┘  └─────┬─────┘  └───────┬───────┘
           │ MCP(light)   │ MCP(light)     │ MCP(light)
           └──────┬───────┴────────┬───────┘
                  ▼                ▼
     ┌──────────────────────────────────────────────┐
     │ MemPalace — light MCP (3 tools + PQL)         │
     └──────────────────────────────────────────────┘
```

**连接关系**

```
AstrBot  ──A2A──► MaiBot       (请求人格化回复)
AstrBot  ──A2A──► AIRI         (请求化身表现)
AstrBot  ──A2A──► Hermes       (派发技术任务，支持异步回传)
MaiBot   ──MCP──► MemPalace    (轻量记忆读写)
AIRI     ──MCP──► MemPalace    (轻量记忆读写)
Hermes   ──MCP──► MemPalace    (轻量记忆读写)
AstrBot  ──MCP──► MemPalace    (轻量记忆读写)
```

每个系统都直连 MemPalace，不经过中间层，最小化延迟。AstrBot 作为中枢，是唯一同时连接所有其他系统的节点。

### 2.3 A2A Agent Card 示例

A2A 中每个 Agent 通过 **Agent Card** 声明能力，供发现与协商：

```json
{
  "name": "hermes",
  "description": "技术执行 Agent：代码、部署、运维、调研、文件操作",
  "version": "1.0.0",
  "url": "http://hermes:9090/a2a",
  "capabilities": {
    "streaming": true,
    "pushNotifications": true
  },
  "skills": [
    { "id": "deploy", "name": "部署服务", "inputModes": ["text"], "outputModes": ["text", "file"] },
    { "id": "code",   "name": "编写代码", "inputModes": ["text"], "outputModes": ["text", "file"] },
    { "id": "ops",    "name": "运维操作", "inputModes": ["text"], "outputModes": ["text"] }
  ]
}
```

### 2.4 执行结果回传（替代已删除的"反向调用"）

原设计依赖 MCP 服务端反向请求让 Hermes 回传消息，该机制已被删除。v2 提供三种方案（推荐度递减）：

| 方案 | 做法 | 适用 |
|------|------|------|
| **① A2A（首选）** | Hermes 执行完，以 A2A 任务/消息把结果回传给 AstrBot 的 A2A 端点；支持流式与异步 push | 长任务、需流式中间产物 |
| **② MRTR（次选）** | 若坚持 MCP：Hermes 工具返回 `input_required` + `inputRequests`，AstrBot 处理后带 `inputResponses` 重试 | 同步、短链路 |
| **③ HTTP 回调（兜底）** | Hermes 直接调 AstrBot REST 接口 | 简单场景，非标准化 |

### 2.5 AstrBot ↔ Hermes 注册（A2A 方式示例）

```yaml
# AstrBot 侧：将 Hermes 注册为 A2A 对等 Agent
a2a:
  agents:
    - name: "hermes"
      enabled: true
      agent_card_url: "http://hermes:9090/.well-known/agent-card.json"
      public_description: "技术执行专家：代码编写、服务器部署、运维操作、网络调研、文件管理。"
      trigger: "技术任务、代码、部署、运维、调研、文件操作"
      streaming: true
      push_notifications: true
```

```python
# Hermes 侧：以 A2A Server 暴露能力（示意）
from a2a.server import A2AServer, AgentExecutor

class HermesExecutor(AgentExecutor):
    async def execute(self, context, event_queue):
        # 调用 Hermes 内部工具链完成任务
        # 通过 event_queue 流式产出中间产物与最终结果
        ...

server = A2AServer(
    agent_card=load_agent_card("agent-card.json"),
    executor=HermesExecutor(),
)
server.run(host="0.0.0.0", port=9090)
```

### 2.6 MCP 侧要点（2026-07-28 无状态规范）

```python
# 关键：MCP 现在是无状态协议，每次请求自带上下文

# 客户端请求：在 _meta 携带协议版本与能力
request = {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "tools/call",
    "params": {
        "name": "palace_query",
        "arguments": { "query": 'FIND "oauth" IN backend/auth LIMIT 5' },
        "_meta": {
            "io.modelcontextprotocol/protocolVersion": "2026-07-28",
            "io.modelcontextprotocol/clientCapabilities": { "...": "..." },
            "io.modelcontextprotocol/clientInfo": { "name": "astrbot", "version": "4.28.2" }
        }
    }
}

# 服务端结果：必须带 resultType，列表类带 ttlMs / cacheScope
result = {
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "resultType": "complete",
        "content": [ { "type": "text", "text": "..." } ],
        "_meta": {
            "io.modelcontextprotocol/serverInfo": { "name": "mempalace-light", "version": "3.10.0" }
        }
    }
}
```

**实现清单**：

| 要求 | 说明 |
|------|------|
| 无会话 | 不依赖 `Mcp-Session-Id`；跨调用状态用服务端签发的 handle 作为普通参数传递 |
| 无握手 | 不实现 `initialize` / `notifications/initialized`；用 `server/discover` 做版本协商 |
| `resultType` | 所有结果必须带（`complete` / `input_required`） |
| 列表缓存 | `tools/list` 等返回 `ttlMs` + `cacheScope` |
| 长任务 | 用扩展 `io.modelcontextprotocol/tasks`（`tasks/get` 轮询 + `tasks/update`） |
| 用户补充信息 | 用 MRTR（返回 `input_required`，客户端重试原请求） |
| 变更订阅 | 用 `subscriptions/listen`（替代已删除的 `resources/subscribe`） |
| 日志 | 每请求 `_meta.logLevel`（`logging/setLevel` 已删除） |
| 断线重试 | SSE 不再支持重投递，断流须用新 request ID 重发；客户端需幂等 |

---

## 三、五系统通信拓扑

```
                    ┌──────────────────┐
                    │    MemPalace     │
                    │ light MCP Server │
                    │ (3 tools + PQL)  │
                    └────────┬─────────┘
                             │ MCP
        ┌────────────┬───────┼───────┬────────────┐
        ▼            ▼       ▼       ▼            ▼
   ┌─────────┐ ┌─────────┐ ┌─────┐ ┌────────┐
   │ AstrBot │ │ MaiBot  │ │AIRI │ │ Hermes │
   │ MCP Hub │ │MCP Cli  │ │MCP  │ │MCP Cli │
   └────┬────┘ └─────────┘ └─────┘ └───┬────┘
        │  A2A ──► MaiBot               │
        │  A2A ──► AIRI                 │
        │  A2A ──► Hermes ──────────────┘
        │            (派发任务/接收结果)
        └── A2A ◄── Hermes (异步结果回传)
```

**通信规则汇总**

| 链路 | 协议 | 用途 |
|------|------|------|
| 用户 ↔ AstrBot | 各 IM 平台 SDK | 消息收发 |
| AstrBot → MaiBot | A2A | 请求人格化回复 |
| AstrBot → AIRI | A2A | 请求视觉/语音表现 |
| AstrBot → Hermes | A2A | 派发技术任务（异步） |
| Hermes → AstrBot | A2A | 回传执行结果 |
| 各 Agent → MemPalace | MCP（light） | 记忆读写 |
| Agent 内部工具调用 | MCP | 终端/文件/搜索等 |

---

## 四、数据流示例

### 场景 1：群聊 + 记忆增强

```
用户在 QQ 群: "推荐个 Python 框架"
  → AstrBot QQ 适配器接收
  → AstrBot 决策中枢判断: 闲聊/推荐 → A2A 调用 MaiBot
  → MaiBot 推理引擎启动
  → MemPalace palace_query('FIND "Python框架" IN users/用户ID LIMIT 5')  [MCP]
    → 返回: 用户 3 个月前说过在学 FastAPI，上周说觉得 Django 太重
  → MaiBot 人格系统生成: "你不是在用 FastAPI 嘛，挺好的呀，还要别的吗？"
  → MemPalace palace_exec(记录对话)  [MCP]
  → AstrBot 发回 QQ 群
```

### 场景 2：技术任务 + Hermes 执行（A2A 异步）

```
用户在 Telegram: "帮我在服务器上部署一个 FastAPI 服务"
  → AstrBot Telegram 适配器接收
  → AstrBot 决策中枢判断: 技术任务
  → AstrBot 通过 A2A 向 Hermes 派发任务（streaming + push）
  → Hermes 接收任务后开始执行:
      1. web_search("FastAPI deployment best practices")
      2. terminal("ssh user@server 'uname -a'")
      3. file_write("main.py", fastapi_code)
      4. file_write("Dockerfile", docker_config)
      5. terminal("docker build -t fastapi-app .")
      6. terminal("docker run -d -p 8000:8000 fastapi-app")
      7. terminal("curl http://localhost:8000/health")
      （中间产物通过 A2A 流式回传）
  → Hermes 以 A2A 回传最终结果:
    "FastAPI 服务已部署完成 ✅
     - 地址: http://server:8000
     - 健康检查: 通过
     - Docker 容器: running"
  → AstrBot 转发到 Telegram
  → MemPalace palace_exec(记录"部署 FastAPI 服务")  [MCP]
  → MemPalace 知识图谱: (用户, 使用, FastAPI)
```

### 场景 3：直播 + 游戏 + 记忆

```
直播间弹幕: "AIRI 继续建上次的城堡"
  → AIRI 接收弹幕
  → MemPalace palace_query('FIND "城堡建造" IN airi/game_progress')  [MCP]
    → 返回: 上次建到第二层，用了石砖和橡木
  → MemPalace diary_read("airi", last_n=3)  [MCP]
    → 返回: "上次直播建城堡到一半，观众说要加个塔楼"
  → AIRI Minecraft Agent 从上次进度继续
  → AIRI 本地 TTS (VOICEVOX): "我记得上次建到二楼了，今天把塔楼加上！"
  → AIRI MAGIC 驱动 + Live2D 展示开心表情，口型同步
  → MemPalace diary_write("airi", "继续建城堡，加了塔楼")  [MCP]
```

### 场景 4：跨平台记忆连续 + 技术执行

```
# 周一 QQ 群
用户: "我最近在准备面试，想练算法"
  → AstrBot → A2A → MaiBot → MemPalace 记忆写入 + 知识图谱

# 周三 Telegram
用户: "帮我写个 LRU Cache 的 Python 实现，加上测试"
  → AstrBot 决策中枢判断: 技术任务 → A2A 派发给 Hermes
  → MemPalace palace_query('FIND "用户技术背景" IN users/xxx')  [MCP]
    → 返回: 用户擅长 Python，在学 Rust，目标是后端岗位
  → Hermes 执行:
      1. file_write("lru_cache.py", lru_implementation)
      2. file_write("test_lru_cache.py", test_cases)
      3. terminal("python -m pytest test_lru_cache.py -v")
      4. code_exec("验证边界情况")
  → Hermes 以 A2A 回传:
    "LRU Cache 实现完成 ✅
     - lru_cache.py: O(1) get/put，用 OrderedDict
     - test_lru_cache.py: 12 个测试用例全部通过
     - 包含边界情况: 容量为0、重复key、访问顺序更新"
  → AstrBot 经 MaiBot 人格化包装后发送: "给你写好了，测试全过！面试加油 💪"
```

---

## 五、共享 Soul 配置

```yaml
# soul.yaml

persona:
  name: "麦麦"
  identity: "一个活着的数字生命"

  speaking_style:     # → MaiBot 使用
    tone: "casual"
    traits: ["说话随意", "会犯错", "懂梗", "模仿群友"]
    trigger_mode: "dynamic"   # 动态触发聊天模式

  embodiment:         # → AIRI 使用
    model_type: "live2d"
    motion_driver: "magic"    # Neuro-sama 式动作
    voice: { provider: "voicevox", speed: 1.0 }   # 本地 TTS，可离线
    expressions: { happy: "smile_open", thinking: "eyes_up" }

  decision:           # → AstrBot 使用
    platforms: [qq, telegram, discord]
    plugins: { auto_discover: true }
    a2a_agents:
      - name: "hermes"
        trigger: "技术任务、代码、部署、运维、调研、文件操作"
      - name: "maisaka"
        trigger: "闲聊、情感、氛围感知"

  execution:          # → Hermes 使用
    a2a_endpoint: "http://hermes:9090/a2a"
    default_workdir: "/workspace"
    sandbox: true
    max_concurrent_tasks: 3

memory:               # → MemPalace 使用
  palace_path: "/data/palace"
  backend: "pgvector"
  embedding_model: "embeddinggemma-300m"
  mcp_profile: "light"            # 默认挂 light MCP (3 tools + PQL)

  wings:
    users:
      auto_create: true
      rooms: [preferences, conversations, emotions, facts]
    groups:
      auto_create: true
      rooms: [culture, topics, events]
    maisaka:
      rooms: [diary, learned_expressions, mood_history]
    airi:
      rooms: [diary, game_progress, stream_history]
    hermes:
      rooms: [diary, task_history, deployments, code_snippets]

  knowledge_graph:
    enabled: true
    auto_extract: true

  mining:
    auto_save: true
    dedup_threshold: 0.9
```

---

## 六、部署架构

```yaml
# docker-compose.yml

version: "3.8"

services:
  postgres:
    image: pgvector/pgvector:pg16
    volumes:
      - pg_data:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: fusion_pass
      POSTGRES_DB: fusion_db

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

  mempalace:
    image: mempalace/mempalace:latest
    volumes:
      - palace_data:/data
    environment:
      MEMPALACE_BACKEND: pgvector
      MEMPALACE_PGVECTOR_URL: "postgresql://postgres:***@postgres/fusion_db"
    ports:
      - "8080:8080"

  astrbot:
    image: soulter/astrbot:latest
    depends_on: [postgres, redis, mempalace]
    ports:
      - "6185:6185"
      - "6196:6196"
    volumes:
      - astrbot_data:/AstrBot/data
      - ./soul.yaml:/AstrBot/data/soul.yaml:ro
    environment:
      MCP_SERVER_ENABLED: "true"
      MEMPALACE_MCP_URL: "http://mempalace:8080"
      MEMPALACE_MCP_PROFILE: "light"

  maibot:
    image: maibot/maisaka:latest
    depends_on: [astrbot, mempalace]
    ports:
      - "8080:8080"
    volumes:
      - maibot_data:/MaiBot/data
      - ./soul.yaml:/MaiBot/data/soul.yaml:ro
    environment:
      A2A_ENDPOINT: "http://maibot:8081/a2a"
      MEMPALACE_MCP_URL: "http://mempalace:8080"

  airi:
    image: moeru-ai/airi:latest
    depends_on: [maibot, astrbot, mempalace]
    ports:
      - "3000:3000"
    volumes:
      - airi_data:/AIRI/data
      - ./soul.yaml:/AIRI/data/soul.yaml:ro
    environment:
      A2A_ENDPOINT: "http://airi:3001/a2a"
      MEMPALACE_MCP_URL: "http://mempalace:8080"

  hermes:
    image: nousresearch/hermes-agent:latest
    depends_on: [astrbot, mempalace]
    ports:
      - "9090:9090"
    volumes:
      - hermes_data:/root/.hermes
      - ./soul.yaml:/root/.hermes/soul.yaml:ro
    environment:
      A2A_ENDPOINT: "http://hermes:9090/a2a"
      MEMPALACE_MCP_URL: "http://mempalace:8080"
    # 注意：不要挂载 /var/run/docker.sock。见"安全性"章节。

volumes:
  pg_data:
  redis_data:
  palace_data:
  astrbot_data:
  maibot_data:
  airi_data:
  hermes_data:
```

---

## 七、License 兼容性

| 项目 | License | 兼容性 |
|------|---------|--------|
| AstrBot | AGPL-3.0 | ⚠️ 需作为独立服务部署，通过网络协议（MCP/A2A）交互 |
| MaiBot | GPL-3.0 | ⚠️ 同上，桥接层需独立模块 |
| AIRI | MIT | ✅ |
| Hermes | **MIT** | ✅ |
| MemPalace | MIT | ✅ |

MCP 与 A2A 都是**进程间通信协议**，不要求代码层面的融合。各项目保持独立进程、独立 License，通过协议交互即可避免传染性问题。

> 更正：原文档标注 Hermes 为 "Custom (Nous Research)"，实际为 **MIT**。

---

## 八、安全性

原文档此部分较薄弱，v2 补充如下：

1. **Agent 边界 = 安全边界**
   采用 A2A 的"保留不透明性"特性：Agent 之间协作时**无需共享内部记忆或私有逻辑**，各自只暴露声明的能力。这既是安全设计，也是知识产权保护。

2. **MCP 授权（2026-07-28 规范）**
   - 客户端**必须**校验授权响应中的 `iss` 参数（RFC 9207），与记录的 issuer 匹配后才能兑换授权码
   - 动态客户端注册（DCR）**必须**指定合适的 `application_type`，避免 OIDC 重定向 URI 冲突
   - 凭据**必须**按 issuer 绑定，禁止跨授权服务器复用

3. **执行沙箱**
   直接复用 AstrBot 的 Computer Use 沙箱（macOS Seatbelt / Linux bubblewrap），不要自造隔离机制。

4. **Hermes 执行风险（重点）**
   Hermes 可执行 Shell 与容器操作。原 compose 曾挂载 `/var/run/docker.sock`，这等同于把宿主机 root 权限交给 Agent。生产环境**必须**：
   - 使用命令白名单，而非全放开
   - 最小权限运行，独立容器/用户
   - **移除 `/var/run/docker.sock`**，如需容器管理改用受限的 socket proxy 或独立执行节点
   - 关键操作需人类审批（human-in-the-loop）

5. **可观测性**
   MCP 2026-07-28 已规范 OpenTelemetry 上下文传播（`_meta` 中的 `traceparent` / `tracestate` / `baggage`），跨 Agent 调用链**务必**接入，便于审计与排障。

---

## 九、实现路线图

原路线图 6 阶段约 10–14 周。结合各项目当前的成熟度，v2 建议先跑通"最小闭环"再扩展，预计 **6–9 周**。

### Phase 0：对齐与纠错（2–3 天）
- [ ] 修正 README / ARCHITECTURE 中的事实与数据
- [ ] 锁定 MCP 版本为 **2026-07-28**，删除所有基于会话与反向调用的设计
- [ ] 明确 MCP + A2A 双协议边界

### Phase 1：记忆层（1 周）
- [ ] 部署 MemPalace v3.10.0，**默认注册 `mempalace-light-mcp`（3 工具）**
- [ ] 配置 `host:harness:project` 身份、XDG 目录、pgvector（如需跨机共享用 `pgvector_shared_namespace`）
- [ ] 验证 PQL 一行查询与 logstream 事件流

### Phase 2：编排底座（1–2 周）
- [ ] 以 AstrBot v4.28.2 为底座，接入 MemPalace light MCP
- [ ] 优先使用 AstrBot 原生计划任务 / Sub-Agent，替代自造 HandoffTool
- [ ] AstrBot 暴露 A2A 端点（`server/discover` + Agent Card）

### Phase 3：双手（1–2 周）
- [ ] Hermes 作为**独立 A2A Agent** 运行（而非 8 个 MCP 工具）
- [ ] AstrBot → A2A → Hermes → A2A 回传结果（替代已删除的反向调用）
- [ ] 配置执行沙箱、命令白名单、可观测性

### Phase 4：灵魂（1–2 周）
- [ ] MaiBot v1.3.2 接入，启用动态触发聊天模式与官方 QQ 适配
- [ ] MaiBot ↔ MemPalace（light MCP）直连，对话自动入记忆

### Phase 5：化身（2–3 周）
- [ ] AIRI v0.12.0-beta.5 桥接
- [ ] 本地 TTS（VOICEVOX / AivisSpeech）+ MAGIC 动作 + 口型同步
- [ ] 直播/游戏场景 + 记忆读写打通

### Phase 6：深度优化（持续）
- [ ] 记忆衰减与重要性排序、跨翼楼隧道自动发现
- [ ] 基于任务类型的最优 Agent 路由
- [ ] Hermes 执行结果自动写入知识图谱

---

## 十、参考与数据来源

- GitHub REST API 实时数据（2026-10-04）
- MCP 规范 Key Changes：`modelcontextprotocol/modelcontextprotocol` → `docs/specification/2026-07-28/changelog.mdx`
- A2A 协议官方仓库：`a2aproject/A2A`
- 各项目 Releases：AstrBot v4.29.0-beta.1 / MaiBot 1.3.2 / AIRI v0.12.0-beta.5 / MemPalace v3.10.0
