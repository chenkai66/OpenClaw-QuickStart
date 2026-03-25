# 第7章·第1节：实战项目——第二大脑知识系统

> 一个完整的实战项目，把 OpenClaw + Claude Code 打造成你的个人知识管理系统。

## 项目概览

**openclaw-second-brain** 是一个真实的生产项目，展示了 OpenClaw 作为 AI Agent 基础设施的完整能力。它能自动：

1. 捕获 OpenClaw TUI 和 Claude Code 的对话记录
2. 通过 LLM 驱动的流水线做摘要和知识提取
3. 生成结构化的知识笔记和日志
4. 维护一套统一的记忆系统
5. 通过 Cron 自动运行研究和知识同步任务

```
Claude Code 对话             OpenClaw TUI 对话
    |                              |
    +-- ~/.claude/projects/       +-- ~/.openclaw/agents/main/sessions/
    |                              |
    +-------------+----------------+
                  |
          知识同步 Agent（每小时 Cron）
                  |
          +-- 自动发现 Claude Code 会话
          +-- 格式统一转换
          +-- 去重（避免重复处理）
          +-- LLM 知识提取
                  |
          第二大脑知识库
          （笔记 + 日志 + 摘要）
                  |
          记忆系统更新（每晚）
                  |
          下次对话自动带上记忆
```

## 技术栈

| 组件 | 技术 |
|------|------|
| 运行时 | Node.js 22+ |
| 前端 | Next.js（Web 界面） |
| 知识图谱 | D3.js |
| AI 模型 | Claude Sonnet 4.5（主力）、Qwen 系列（备用） |
| LLM 流水线 | TypeScript 自定义摘要提取 |
| API | DashScope API（OpenAI 兼容）或 Anthropic API |
| 进程管理 | systemd |

---

*下一节：[知识 Agent 详解 →](02-knowledge-agents.md)*
