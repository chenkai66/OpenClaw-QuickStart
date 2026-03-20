# 第6章·第3节：进阶技巧与配置速查

> 老手们总结出来的实用技巧，一小时让你的 OpenClaw 脱胎换骨。

---

## 1. 解决"AI 失忆"：memoryFlush

### 问题

对话太长时，OpenClaw 会压缩旧消息来腾出上下文空间。压缩过程中，细节可能丢失——AI 就"忘了"你们之前聊的内容。

### 解决方案

开启 `memoryFlush`：在压缩触发之前，AI 自动把关键信息保存到文件。

在 `~/.openclaw/openclaw.json` 里加上：

```json
{
  "agents": {
    "defaults": {
      "compaction": {
        "reserveTokensFloor": 20000,
        "memoryFlush": {
          "enabled": true,
          "softThresholdTokens": 4000
        }
      }
    }
  }
}
```

| 参数 | 含义 | 建议值 |
|------|------|--------|
| `reserveTokensFloor` | 压缩后最少保留多少 token | 20000 |
| `memoryFlush.enabled` | 压缩前自动保存 | `true` |
| `softThresholdTokens` | 在到达限制前多少 token 开始刷新 | 4000 |

### 手动压缩

在长对话中，可以手动触发，同时指定保留什么：

```
/compact 保留所有技术决策和代码片段
```

---

## 2. 渐进式输出：blockStreaming

默认情况下，OpenClaw 会等 AI 生成完整回复后一次性发送。开启 blockStreaming 后，按段落逐步推送，体验好很多：

```json
{
  "agents": {
    "defaults": {
      "blockStreamingDefault": "on"
    }
  }
}
```

---

## 3. 静默时段：activeHours

不想半夜被 AI 吵醒：

```json
{
  "agents": {
    "defaults": {
      "heartbeat": {
        "activeHours": "08:00 - 23:00"
      }
    }
  }
}
```

Cron 任务和定时消息只在这个时间段内触发。

---

## 4. 消息确认反馈：ackReaction

发消息后，AI 立刻回一个 emoji 表示"收到了，正在处理"：

```json
{
  "channels": {
    "discord": { "ackReaction": "🫐" },
    "telegram": { "ackReaction": "👀" }
  }
}
```

---

## 5. 模型分级策略——省 60-70% 费用

不是所有任务都需要最贵的模型。在 AGENTS.md 里定义分级策略：

```markdown
## 模型使用策略

- **简单任务**（搜索、总结、格式化）：用 `qwen3.5-flash` 或 `MiniMax-M2.5`
- **一般任务**（写作、分析、聊天）：用 `qwen3.5-plus`
- **复杂任务**（架构设计、调试、推理）：用 `qwen3-max` 或 `kimi-k2.5`
- **写代码**：用 `qwen3-coder-next`
```

在 TUI 里随时切换：
```
/model qwen3.5-flash     # 快速简单查询
/model qwen3-max          # 需要深度推理
```

---

## 6. 子 Agent 并行处理

OpenClaw 可以派出子 Agent 并行处理任务，而不是主 Agent 一件件做：

### 工作方式

1. 你告诉主 Agent："帮我调研这 3 个方向，然后汇总"
2. 主 Agent 派出 3 个子 Agent，各研究一个方向
3. 子 Agent 汇报结果，主 Agent 综合分析

### 在 AGENTS.md 中启用

```markdown
## 子 Agent

当任务有 3 个以上独立子任务时，派出子 Agent：
- 每个子 Agent 获得一个聚焦的任务描述
- 子 Agent 并行工作
- 主 Agent 收集并综合结果
- 子 Agent 优先使用轻量模型（qwen3.5-flash），除非任务需要强推理能力
```

---

## 7. 记忆维护——防止记忆腐化

时间久了，记忆会越积越多，旧的信息可能已经过时。在 HEARTBEAT.md 里加上自动维护：

```markdown
## 记忆维护（每周）
- 每周日 03:00：
  1. MEMORY.md 去重
  2. 归档 30 天前的日志到 memory/archive/
  3. 检查 memory/projects.md 是否与实际项目状态一致
  4. 清理已完成或已放弃的项目
```

---

## 8. 多渠道格式适配

不同平台对 Markdown 的支持不一样：

| 格式 | Discord | Telegram | WhatsApp | 钉钉 |
|------|---------|----------|----------|------|
| 表格 | 支持 | 不支持 | 不支持 | 不支持 |
| 代码块 | 支持 | 支持 | 不支持 | 支持 |
| 加粗/斜体 | 支持 | 支持 | 支持 | 支持 |

在 AGENTS.md 里加上自动适配规则：

```markdown
## 平台格式适配
- **Discord / Telegram**：正常用 Markdown，代码用三个反引号
- **WhatsApp / 钉钉**：不用表格，改成列表
- **Discord**：多个链接用 <> 包起来，避免预览刷屏
```

---

## 9. openclaw.json 完整配置速查

| 配置路径 | 作用 | 推荐值 |
|---------|------|--------|
| `models.providers.bailian.baseUrl` | API 端点 | 见 Coding Plan 章节 |
| `agents.defaults.model.primary` | 默认模型 | `bailian/qwen3.5-plus` |
| `agents.defaults.blockStreamingDefault` | 渐进式输出 | `"on"` |
| `agents.defaults.compaction.memoryFlush.enabled` | 压缩前自动保存 | `true` |
| `agents.defaults.heartbeat.activeHours` | 静默时段 | `08:00 - 23:00` |
| `tools.exec.enabled` | 允许执行命令 | `true` |
| `tools.web.search.enabled` | 允许网页搜索 | `true` |
| `tools.web.search.apiKey` | Brave 搜索 API Key | 在 brave.com 免费申请 |
| `tools.media.image.enabled` | 图片识别 | `true`（需视觉模型） |

---

## 10. 优先级清单（1 小时改造你的配置）

| 优先级 | 任务 | 耗时 |
|-------|------|------|
| 1 | **AGENTS.md**：会话启动 + 记忆规则 + 安全边界 | 30 分钟 |
| 2 | **memoryFlush**：开启压缩前自动保存 | 5 分钟 |
| 3 | **ackReaction**：消息收到确认 | 1 分钟 |
| 4 | **blockStreaming**：渐进式输出 | 5 分钟 |
| 5 | **activeHours**：静默时段 | 5 分钟 |
| 6 | **模型分级**：简单任务用便宜模型 | 15 分钟 |
| 7 | **Cron 任务**：日报 / 周报 | 15 分钟 |
| 8 | **记忆维护**：每周自动清理 | 10 分钟 |
| 9 | **自定义 Skill**：写第一个 SKILL.md | 30 分钟 |
| 10 | **多渠道**：接入 Discord 或 Telegram | 30 分钟 |

---

*上一节：[定时任务](02-cron-jobs.md) | 下一节：[MCP 协议集成 →](05-mcp-integration.md)*
