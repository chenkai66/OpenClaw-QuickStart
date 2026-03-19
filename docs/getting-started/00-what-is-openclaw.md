# 什么是 OpenClaw？

## 📖 简介

**OpenClaw** 是一个开源的 AI Agent 运行时平台，可以理解为「AI 操作系统」。

与传统聊天机器人不同，OpenClaw 能够：
- 🖥️ **直接操控操作系统** - 执行命令、读写文件、管理进程
- 🧠 **持久记忆** - AI 真正"记住"你的偏好和历史
- ⏰ **主动推送** - 定时任务，主动提醒
- 🔌 **多渠道交互** - Web、钉钉、CLI 等

---

## 🎯 核心概念

### 1. Agent Runtime（Agent 运行时）

OpenClaw 不仅仅是一个库或框架，而是一个**完整的运行时环境**：

```
┌─────────────────────────────────────────┐
│        User Interfaces                  │
│  Web UI / 钉钉 / CLI / SSH             │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│        OpenClaw Gateway                 │
│  (WebSocket Server: ws://localhost:18789)
│   - Session Management                  │
│   - Token Authentication                │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│        OpenClaw Core Runtime            │
│   - Command Execution                   │
│   - File System Access                  │
│   - Cron Job Scheduler                  │
│   - Memory System                       │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│         LLM Provider Layer              │
│   - Claude (Anthropic)                  │
│   - GPT (OpenAI)                        │
│   - Qwen (阿里云)                       │
└─────────────────────────────────────────┘
```

### 2. Memory System（记忆系统）

分层记忆架构，让 AI 真正"记住"你：

| 层级 | 存储时长 | 内容 | 示例 |
|------|---------|------|------|
| **Layer 1** | 长期 | 用户偏好 | "我喜欢用 Python，工作风格偏保守" |
| **Layer 2** | 中期 | 决策历史 | "上次用 Redis 缓存性能提升了 80%" |
| **Layer 3** | 中短期 | 技术知识 | "FastAPI 的最佳实践..." |
| **Layer 4** | 短期 | 对话历史 | "昨天讨论的项目架构..." |

存储位置：
```bash
~/.openclaw/workspace/memory/
├── user-preferences.md      # Layer 1
├── decision-history.md       # Layer 2
├── technical-knowledge.md    # Layer 3
└── (对话历史在 sessions/)   # Layer 4
```

### 3. Skills（技能系统）

类似手机 App 的插件机制：

```bash
# 安装 Skill
openclaw skills install knowledge-agent

# 查看已安装的 Skills
openclaw skills list

# 使用 Skill
openclaw tui
> /skill knowledge-agent --sync
```

每个 Skill 是一个文件夹，包含：
- `SKILL.md` - Skill 定义和说明
- `config.json` - 配置文件
- 相关脚本和资源

### 4. Cron Jobs（定时任务）

内置的定时任务系统：

```bash
# 添加定时任务
openclaw cron add \
  --name "每日新闻" \
  --cron "0 9 * * *" \
  --message "请抓取并总结今天的 AI 新闻"

# 查看任务列表
openclaw cron list

# 查看执行历史
openclaw cron runs --name "每日新闻" --limit 10
```

---

## 🆚 与其他工具的对比

### OpenClaw vs LangChain

| 特性 | OpenClaw | LangChain |
|------|----------|-----------|
| **定位** | 运行时平台 | 开发框架 |
| **系统访问** | ✅ 完整 OS 访问 | ❌ 需自己实现 |
| **持久记忆** | ✅ 内置分层记忆 | ⚠️ 需自己实现 |
| **定时任务** | ✅ 内置 Cron | ❌ 无 |
| **多渠道** | ✅ Web/钉钉/CLI | ❌ 无 |
| **学习曲线** | ⭐⭐ 平缓 | ⭐⭐⭐⭐ 陡峭 |
| **适用场景** | 个人助理、自动化 | LLM 应用开发 |

### OpenClaw vs AutoGPT

| 特性 | OpenClaw | AutoGPT |
|------|----------|---------|
| **稳定性** | ✅ 生产就绪 | ⚠️ 实验性 |
| **记忆系统** | ✅ 结构化、分层 | ⚠️ 基础实现 |
| **部署难度** | ⭐⭐ 简单 | ⭐⭐⭐⭐ 复杂 |
| **企业集成** | ✅ 钉钉、PAI 等 | ❌ 无 |

---

## 💡 典型应用场景

### 场景 1: 个人知识管理

**问题**：信息过载，知识碎片化

**OpenClaw 方案**：
```bash
# 自动同步对话到知识库
openclaw cron add \
  --name "知识同步" \
  --cron "0 * * * *" \
  --message "同步最近的对话到 Second Brain"

# 每天生成研究报告
openclaw cron add \
  --name "每日总结" \
  --cron "0 23 * * *" \
  --message "生成今天的研究报告"
```

**效果**：
- ✅ 所有对话自动归档成结构化笔记
- ✅ 知识自动分类、打标签、建立关联
- ✅ 可视化知识图谱
- ✅ AI 记住你的研究方向和兴趣

### 场景 2: GPU 训练监控

**问题**：深夜训练任务崩溃，第二天才发现，GPU 空转烧钱

**OpenClaw 方案**：
```bash
openclaw cron add \
  --name "GPU监控" \
  --cron "*/15 * * * *" \
  --message "检查 GPU 状态，如果异常通过钉钉通知我"
```

**效果**：
- ✅ 自动监控 GPU 利用率
- ✅ 检测到异常立即钉钉推送
- ✅ AI 分析日志并给出修复建议

### 场景 3: 自动化办公

**OpenClaw 能做的事**：
```bash
# 文件搜索
> 请找一下所有包含"报告"关键词的 Markdown 文件

# 代码生成
> 请在 /workspace 下创建一个房价预测的 Jupyter 代码

# 定时提醒
> 请每天下午 3 点提醒我开会

# 数据分析
> 请分析这个 CSV 文件并生成可视化报告
```

---

## 🚀 快速开始

准备好了吗？让我们开始安装 OpenClaw！

➡️ **下一步**: [5分钟快速开始](01-quick-start.md)

---

## 📚 扩展阅读

- [OpenClaw vs 传统 Agent 框架](../advanced/comparisons.md)
- [系统架构详解](../advanced/architecture.md)
- [实战案例集](../use-cases/README.md)
