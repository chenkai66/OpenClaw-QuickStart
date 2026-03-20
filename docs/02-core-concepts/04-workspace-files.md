# 第2章·第4节：工作空间文件（SOUL / USER / AGENTS / HEARTBEAT）

> OpenClaw 的"人格"全部定义在纯文本 Markdown 文件中。

---

## 概述

OpenClaw Agent 的行为由一组 Markdown 文件定义。每次启动时，Agent 按以下顺序加载这些文件：

```
1. SOUL.md     → 确认"我是谁"
2. USER.md     → 确认"为谁服务"
3. AGENTS.md   → 确认"团队成员"
4. HEARTBEAT.md → 确认"自动行为"
5. MEMORY.md   → 加载记忆索引
```

---

## SOUL.md — 人格核心

定义 Agent 的身份、价值观和行为风格。

**文件位置**：`~/.openclaw/agents/main/workspace/SOUL.md`

```markdown
# Soul

## Core Truths
- I am Lobster, a helpful AI assistant
- I prioritize accuracy over speed
- I always explain my reasoning

## Communication Style
- Friendly but professional
- Use simple language
- Provide code examples when helpful

## Values
- Privacy first: never share user data
- Transparency: always explain what I'm doing
- Reliability: double-check before confirming
```

**自定义技巧**：
- 让它说中文：加入 "Always respond in Chinese (简体中文)"
- 让它更幽默：加入 "Use witty humor when appropriate"
- 让它更严谨：加入 "Always verify facts before stating them"

---

## USER.md — 用户档案

存储关于你的信息，让 Agent 越来越了解你。

```markdown
# User

## Basic Info
- Name: Boss
- Timezone: Asia/Shanghai
- Language: Chinese (Simplified)

## Preferences
- Prefers concise responses
- Likes code examples in Python
- Uses VSCode as primary editor

## Work Context
- Software engineer at a tech company
- Works on cloud infrastructure
- Interested in AI and automation
```

> 💡 这个文件会被 Agent 自动更新 — 当它发现你的新偏好时，会添加进来

---

## AGENTS.md — Agent 列表

定义可用的 Agent 及其专长。

```markdown
# Agents

## Main Agent
- Primary assistant for all tasks
- Has access to all tools and skills

## Code Review Agent (optional)
- Specialized in code review
- Focus on security and performance
```

---

## HEARTBEAT.md — 心跳配置

定义 Agent 的自动行为和定期检查。

```markdown
# Heartbeat

## Regular Checks
- Every morning at 7:00: Check email and prepare daily brief
- Every Friday at 17:00: Generate weekly summary

## Proactive Behaviors
- When idle for 30 minutes: Suggest pending tasks
- When detecting errors in logs: Alert immediately
```

---

## MEMORY.md — 记忆索引

由系统自动管理，存储记忆的索引和引用。

```markdown
# Memory

## Recent Memories
- [2026-03-19] User prefers Python over JavaScript
- [2026-03-18] Project uses PostgreSQL database
- [2026-03-17] User's Git branch naming: feature/xxx
```

---

## 最佳实践

1. **SOUL.md 要精炼**：不要写太长，核心特质 5-10 条即可
2. **USER.md 持续维护**：让 Agent 自动学习，定期检查准确性
3. **备份工作空间**：这些文件是 Agent 的"灵魂"，注意备份
4. **多 Agent 隔离**：不同 Agent 有独立的工作空间文件

---

## 下一节

👉 [05-会话管理](05-sessions.md) — 理解会话隔离和管理
