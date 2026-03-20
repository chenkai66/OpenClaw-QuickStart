# 第6章·第4节：多 Agent 路由

> 让不同渠道、不同群组使用完全不同的 AI 助手。

---

## 为什么需要多 Agent

一个 Agent 不够用的场景：

- **工作和生活分开**：工作 Agent 用 Claude，家用 Agent 用通义千问
- **团队共享**：不同群组绑定不同的 AI 角色
- **专业分工**：代码审查用一个 Agent，日常聊天用另一个

---

## 配置多 Agent

在 `openclaw.json` 中定义多个 Agent：

```json
{
  "agents": {
    "defaults": {
      "model": { "primary": "anthropic/claude-sonnet-4-20250514" }
    },
    "list": [
      {
        "id": "work",
        "name": "工作助手",
        "model": "anthropic/claude-sonnet-4-20250514",
        "workspace": "~/.openclaw/workspace-work"
      },
      {
        "id": "home",
        "name": "生活助手",
        "model": "dashscope/qwen-max",
        "workspace": "~/.openclaw/workspace-home"
      },
      {
        "id": "code",
        "name": "代码专家",
        "model": "openai/gpt-5.4",
        "workspace": "~/.openclaw/workspace-code"
      }
    ]
  }
}
```

每个 Agent 有**独立的工作空间**——独立的 SOUL.md、MEMORY.md、会话记录。

---

## 绑定 Agent 到渠道

通过 `bindings` 把特定渠道/群组映射到特定 Agent：

```json
{
  "bindings": [
    {
      "channel": "telegram",
      "group": "-1001234567890",
      "agentId": "work"
    },
    {
      "channel": "dingtalk",
      "agentId": "work"
    },
    {
      "channel": "whatsapp",
      "agentId": "home"
    }
  ]
}
```

---

## 命令行指定 Agent

```bash
# 指定 Agent 发消息
openclaw agent --agent work --message "帮我审查这个 PR"
openclaw agent --agent home --message "今天晚上吃什么"
openclaw agent --agent code --message "写一个 Python 爬虫"
```

---

## 子 Agent（Sub-Agent）

主 Agent 可以派生子 Agent 来处理子任务：

```json
{
  "agents": {
    "defaults": {
      "subagents": {
        "maxConcurrent": 8,
        "allowAgents": ["prompt-engineer", "media-generator"]
      }
    }
  }
}
```

子 Agent 运行在独立的沙箱中，完成任务后将结果返回给主 Agent。

---

## Telegram 论坛话题绑定

Telegram 超级群组的论坛模式下，不同话题可以绑定不同 Agent：

- "技术支持" → `code` Agent
- "日常闲聊" → `home` Agent
- "周报汇总" → `work` Agent

---

## 下一节

👉 [05-MCP 协议集成](05-mcp-integration.md) — 让 AI 连接更多工具
