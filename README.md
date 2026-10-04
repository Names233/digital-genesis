# Digital Genesis

> **AstrBot × MaiBot × AIRI × Hermes × MemPalace — 五合一融合架构**
>
> MaiBot 是灵魂，AIRI 是化身，AstrBot 是大脑，Hermes 是双手，MemPalace 是记忆
> —— 创世，让数字生命降临。

## 核心理念

| 维度 | 负责者 | 说明 |
|------|--------|------|
| **怎么说** (Personality) | MaiBot | 人格、语气、情感、氛围感知 |
| **怎么表现** (Embodiment) | AIRI | 视觉形象、语音、动画、游戏 |
| **怎么决策** (Decision) | AstrBot | 大脑中枢、平台路由、任务调度、插件生态 |
| **怎么做到** (Execution) | Hermes | 技术执行、代码、部署、运维、调研、文件操作 |
| **怎么记** (Memory) | MemPalace | 记忆宫殿、知识图谱、语义检索 |

## 五大项目

| 项目 | Star | 语言 | License | 核心能力 |
|------|------|------|---------|----------|
| [Hermes](https://github.com/NousResearch/hermes-agent) | 251.1k | Python | MIT | 技术执行者，"The agent that grows with you" |
| [MemPalace](https://github.com/MemPalace/mempalace) | 59.4k | Python | MIT | 记忆系统标杆，96.6% R@5，45 MCP 工具（含 3 工具轻量版） |
| [AIRI](https://github.com/moeru-ai/airi) | 50.0k | TypeScript | MIT | 数字化身，Live2D/VRM，本地 TTS，游戏，多端 |
| [AstrBot](https://github.com/AstrBotDevs/AstrBot) | 41.4k | Python | AGPL-3.0 | 大脑中枢，18+ 平台，Computer Use，插件生态 |
| [MaiBot](https://github.com/Mai-with-u/MaiBot) | 6.1k | Python | GPL-3.0 | 拟人化数字生命，"最像而不是好"，动态触发聊天 |

> Star 数据为 2026-10-04 实时核查值。

## 架构总览

采用 **MCP + A2A 双协议**分层：**MCP 管"Agent ↔ 工具/记忆"，A2A 管"Agent ↔ Agent"**。

```
用户 (QQ/微信/TG/Discord/直播/Web)
  │
  ▼
AstrBot (大脑) ── 决策中枢 / 编排器 / A2A 编排端点
  ├── MaiBot (灵魂)  ── A2A ── 人格化回复
  ├── AIRI (化身)    ── A2A ── 视觉/语音/游戏
  ├── Hermes (双手)  ── A2A ── 技术执行（对等 Agent，非工具集合）
  └── MemPalace (记忆) ── 各 Agent 以轻量 MCP 直连共享
```

## 详细设计

👉 [ARCHITECTURE.md](./ARCHITECTURE.md)
