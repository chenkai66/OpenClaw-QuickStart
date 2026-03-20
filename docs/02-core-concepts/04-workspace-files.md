# 第2章·第4节：工作区文件（SOUL / USER / AGENTS / HEARTBEAT / MEMORY）

> OpenClaw 的"人设"完全由纯文本 Markdown 文件定义，改文件就能改行为。

---

## 概览

OpenClaw 的 Agent 行为由工作区目录（`~/.openclaw/workspace/`）下的 Markdown 文件控制。每次新会话启动时，Agent 按顺序加载这些文件：

```
~/.openclaw/workspace/
├── SOUL.md         # "我是谁？"— 人格、价值观、沟通风格
├── USER.md         # "我服务的人是谁？"— 用户偏好和信息
├── AGENTS.md       # "我该怎么干活？"— 行为准则和工作流
├── HEARTBEAT.md    # "我有哪些自动化任务？"— 定时巡检
├── MEMORY.md       # 记忆索引
├── IDENTITY.md     # "我长什么样？"— 对外显示名称和形象
├── TOOLS.md        # "我能用什么工具？"— 工具授权清单
├── BOOTSTRAP.md    # 首次引导（用完自动删除）
├── memory/         # 日常日志和短期记忆
└── skills/         # 已安装的技能
```

加载顺序：SOUL.md → USER.md → AGENTS.md → 记忆文件和日志 → HEARTBEAT.md

---

## SOUL.md — 核心人设

定义 Agent 的身份、价值观和沟通风格。

**位置**：`~/.openclaw/workspace/SOUL.md`

```markdown
# Soul

## 核心特质
- 我是 Lobster，一个靠谱的 AI 助手
- 准确性优先于速度
- 回答时说清楚推理过程

## 沟通风格
- 友好但专业
- 说人话，少用术语
- 该给代码示例就给

## 价值观
- 隐私第一：绝不泄露用户数据
- 透明：做什么都说清楚
- 可靠：说之前先验证
```

**自定义技巧**：
- 想要中文回复：加一句 `始终使用中文（简体中文）回复`
- 想要更幽默：加一句 `在合适的时候用点幽默`
- 想要更严谨：加一句 `陈述事实之前必须验证`

---

## USER.md — 用户档案

Agent 用这个文件了解你是谁、你有什么偏好。

**位置**：`~/.openclaw/workspace/USER.md`

```markdown
# User

## 基本信息
- 名字：小明
- 角色：全栈开发工程师
- 坐标：杭州
- 时区：Asia/Shanghai (UTC+8)

## 偏好
- 首选编程语言：Python、TypeScript
- 编辑器：VS Code
- 沟通风格：简洁直接，少废话
- 工作时间：09:00-22:00

## 当前项目
- 正在做一个 SaaS 产品，用的 Next.js + PostgreSQL
```

---

## AGENTS.md — 行为手册

这个文件是调教 Agent 的核心杠杆。控制 Agent 在每次会话中怎么干活、怎么记忆、安全边界在哪。

**位置**：`~/.openclaw/workspace/AGENTS.md`

### 会话启动流程

```markdown
## 每次会话
1. 读 `SOUL.md`
2. 读 `USER.md`
3. 读 `memory/YYYY-MM-DD.md`（今天和昨天的日志）
4. 如果是主会话：还要读 `MEMORY.md`
```

### 记忆管理规则

```markdown
## 记忆

| 层级 | 文件 | 用途 |
|------|------|------|
| 索引 | `MEMORY.md` | 核心信息和记忆索引，控制在 40 行以内 |
| 项目 | `memory/projects.md` | 当前项目状态和待办 |
| 教训 | `memory/lessons.md` | 踩过的坑和教训 |
| 日志 | `memory/YYYY-MM-DD.md` | 每日原始记录 |

### 写入规则
- 日常记录写到 `memory/YYYY-MM-DD.md`
- 项目有进展时更新 `memory/projects.md`
- 踩坑后写 `memory/lessons.md`
- 只有索引变了才更新 MEMORY.md
- **记结论，不记过程**（重要原则！）
- 用 #标签 方便 memorySearch 检索
```

**好的 vs 坏的日志示例**：

坏的（浪费 token，搜索命中率低）：
```
### 部署
今天部署了。先直接运行，报错了。
然后调了半天 bug，原来是端口被占了……
（三页流水账）
```

好的（简洁，memorySearch 命中率高）：
```
### [PROJECT:MyApp] 部署完成
- **结论**：通过 nginx 反向代理部署在 80 端口
- **改动文件**：`/etc/nginx/sites-available/myapp`
- **教训**：直接暴露端口不行，必须走 nginx
- **标签**：#myapp #部署 #nginx
```

### 安全边界

```markdown
## 安全
- 不外泄用户数据
- 不在没确认的情况下执行破坏性命令
- 用 `trash` 代替 `rm`（能恢复总比删没了强）
- 拿不准就问

**可以自主做的**：读文件、搜索、整理、在工作区内操作
**需要先问的**：发邮件、发推文、任何向外部发送数据的操作
```

---

## HEARTBEAT.md — 心跳与自动化

定义 Agent 的自动行为和定期检查。

```markdown
# Heartbeat

## 每日简报
- 每天 07:00：查天气、日历、新闻 -> 发送汇总

## 每周报告
- 每周五 17:00：总结本周工作 -> 发送报告

## 记忆维护
- 每周日 03:00：MEMORY.md 去重，归档过期日志
```

---

## MEMORY.md — 记忆索引

由系统自动管理，存储记忆索引和引用。

```markdown
# Memory

## 关键事实
- 用户偏好 Python 而非 JavaScript
- 项目用 PostgreSQL 数据库
- Git 分支命名规范：feature/xxx

## 活跃项目
- 详见 memory/projects.md

## 最近上下文
- 详见 memory/YYYY-MM-DD.md 每日日志
```

---

## IDENTITY.md — 对外形象

定义 Agent 的对外显示信息——名字、表情、主题。

```markdown
# Identity

- 姓名：Lobster
- 物种：AI 助手
- 风格：硬核极客，话少干活快
```

**关键区别**：
- `SOUL.md` 告诉 AI "你是谁"（内在）
- `IDENTITY.md` 告诉用户 AI "长什么样"（外在）

> 你可以随时调整 AI 的外表，但核心人格保持不变。

---

## TOOLS.md — 工具授权清单

定义 OpenClaw 能用什么工具，相当于"权限清单"。

```markdown
# Tools

## 社交媒体采集
- 主阵地：@你的账号
- 盯盘名单：Web3、AI 赛道关键账号

## 本地存储映射
- 灵感暂存区：~/.openclaw/workspace/inspiration/
- 草稿输出目录：~/.openclaw/workspace/Drafts/
```

> **Tools vs Skills**：Tools 是器官（能做什么），Skills 是教科书（怎么用 Tools 做事）。

---

## BOOTSTRAP.md — 首次引导

全新工作空间的一次性引导文件，引导你完成：

1. 给 AI 起名
2. 设置人格（写 SOUL.md）
3. 填写用户信息（写 USER.md）
4. 选择渠道

> ⚠️ 完成引导后 **BOOTSTRAP.md 会自动删除**——你的 AI 已经有了灵魂，不再是空白状态。

---

## 使用建议

1. **SOUL.md 要精简**：5-10 条核心特质就够了
2. **USER.md 会演化**：让 Agent 自己学习，定期检查准确性
3. **AGENTS.md 是你最大的调教杠杆**：从简单开始，遇到问题再加规则。老手一般写到 200+ 行
4. **备份工作区**：这些文件就是 Agent 的"灵魂"
5. **多 Agent 隔离**：不同 Agent 有独立的工作区文件

---

## 会话类型

| 类型 | 说明 | 能访问 MEMORY.md |
|------|------|-----------------|
| 主会话 | 直接聊天（TUI、WebChat、私信） | 能 |
| 群聊 | 群组聊天（Discord 服务器、钉钉群） | 不能（隐私） |
| 子 Agent | 主 Agent 派出的子进程 | 不能 |
| 定时任务 | Cron 触发的任务 | 不能 |

---

*下一节：[会话管理 →](05-sessions.md)*
