# 第2章·第5节：会话管理

> 每个对话都是一个独立的会话，互不干扰。

---

## 会话是什么？

会话（Session）是 Agent 与用户之间的一次完整对话上下文。

**特点**：
- 每个渠道的每个用户自动创建独立会话
- 会话内的上下文持续保留
- 不同会话之间完全隔离
- 会话数据以 JSONL 格式存储

---

## 会话存储

```
~/.openclaw/agents/main/sessions/
├── session-abc123.jsonl       # 会话1
├── session-def456.jsonl       # 会话2
└── session-ghi789.jsonl       # 会话3
```

每个 JSONL 文件中，每行是一条消息记录：

```json
{"role": "user", "content": "帮我查天气", "timestamp": "2026-03-20T10:00:00Z"}
{"role": "assistant", "content": "北京今天晴，25°C...", "timestamp": "2026-03-20T10:00:05Z"}
```

---

## 会话管理命令

```bash
# 查看所有会话
/sessions

# 查看当前会话状态
/status

# 创建新会话
/new

# 切换会话
/session <session-id>
```

---

## 多会话并行

OpenClaw 支持同时运行多个会话：
- 一个讨论产品方案
- 一个研究旅行计划
- 一个写代码

通过 `sessions_spawn` 工具，可以创建子任务会话。

---

## 下一章

👉 [第3章：配置详解](../03-configuration/01-openclaw-json.md) — openclaw.json 完全指南
