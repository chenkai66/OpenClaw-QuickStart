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

## 核心组件

### 1. 知识同步 Agent

每小时通过 Cron 运行，自动处理新的对话记录：

```bash
npm run agent:knowledge
```

工作流程：
1. 扫描 OpenClaw 和 Claude Code 的会话目录
2. 找到未处理的新对话
3. 调用 LLM 提取关键信息和结论
4. 生成结构化笔记，保存到 `content/notes/`
5. 更新知识索引

### 2. 研究 Agent

每天运行一次，根据你最近的对话主题自动做研究：

```bash
npm run agent:research
```

工作流程：
1. 分析最近 7 天的热门话题
2. 提取高频关键词
3. 在 Google、GitHub、HackerNews 上搜索
4. 生成研究报告，保存到 `content/reports/`

### 3. 社区研究 Agent

按需触发的研究助手，分析社区讨论：

| 研究类型 | 说明 |
|---------|------|
| 趋势分析 | 搜索近 3 个月讨论，分析趋势 |
| 工具对比 | 收集真实用户评价，对比优劣 |
| 最佳实践 | 搜索实战经验和代码示例 |
| 社区观点 | 分析共识和争议 |

用法：
```bash
# 在对话中直接说
"帮我调研一下 Cursor 和 Copilot 的对比"
```

## 记忆系统架构

系统使用 4 层记忆架构：

| 层级 | 持久性 | 内容 |
|------|-------|------|
| 用户偏好 | 长期 | 姓名、习惯、沟通风格 |
| 决策历史 | 中期 | 过去的选择和项目决策 |
| 技术知识 | 中短期 | 学到的概念、代码模式 |
| 对话历史 | 短期 | 最近的交互记录 |

## 生产环境经验

### 1. 让对话有意义

```bash
# 给重要对话设标题
openclaw chat --title "React 性能优化" "讨论 React 性能"
```

### 2. 归档旧会话

```bash
# 归档 30 天前的会话
openclaw session archive --older-than 30d

# 手动备份
cp -r ~/.openclaw/agents/main/sessions \
  ~/.openclaw/backups/sessions-$(date +%Y%m%d)
```

### 3. 监控 Cron 任务

```bash
# 查看最近执行记录
openclaw cron runs --name "Knowledge Sync" --limit 50

# 查看失败的任务
openclaw cron runs --name "Knowledge Sync" --failed
```

### 4. 保护 API Key

在 `.gitignore` 里加上：

```gitignore
# OpenClaw 配置和认证文件（包含 API Key）
.openclaw/
**/auth-profiles.json

# 环境变量和密钥
.env
.env.local
*.key
*.pem
```

---

*下一节：[钉钉对接前置准备 →](../08-dingtalk-complete/01-prerequisites.md)*
