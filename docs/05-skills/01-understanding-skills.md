# 第5章·第1节：理解 Skills

> Skills 把 AI 从"能说"变成"能做" — 没有 Skills 的 OpenClaw 只是聊天机器人。

---

## 核心比喻

把 AI 想象成一个**聪明但没经验的实习生**：

| 概念 | 比喻 | 作用 |
|------|------|------|
| 普通 Prompt | 每次从头教 | 今天教一遍，明天还得重新教 |
| Rule / 记忆 | 工位上的行为守则 | 一直生效，但只管态度和格式 |
| MCP / Tools | 电脑里的软件 | 能用外部工具，但不知道什么时候该用 |
| **Skills** | **岗位培训大礼包** | 完整 SOP + 模板 + 脚本，按固定流程执行 |

---

## Skills vs Tools vs MCP

| 特性 | Skills | MCP | Tools |
|------|--------|-----|-------|
| 核心作用 | 知识复用 | 能力扩展 | 功能调用 |
| 实现方式 | Markdown 文件 | 服务器端配置 | API 接口 |
| 使用难度 | ⭐ 简单 | ⭐⭐⭐ 复杂 | ⭐⭐ 中等 |
| Token 消耗 | 低（渐进式加载） | 高（启动时全量加载） | 中等 |
| 创建门槛 | 任何人 | 需要编码 | 需要 API 开发 |
| 部署要求 | 无需服务器 | 需要服务器 | 需要后端 |

---

## 渐进式加载机制

Skills 不会一次性全部塞进 AI 的上下文，而是**按需加载**：

```
阶段1：发现
  └─ 启动时只加载每个 Skill 的名称和描述（~50字/个）

阶段2：激活
  └─ 当任务匹配某个 Skill 时，才加载完整 SKILL.md（~500字）

阶段3：执行
  └─ 按照 SKILL.md 的指令执行，按需加载参考文件
```

**Token 节省对比**：
- 传统全量加载：10个 Skills × 500字 = **5000字（~1250 tokens）**
- Skills 渐进加载：125 + 125 = **250 tokens**（节省 80%）

---

## Skill 的文件结构

### 最小结构

```
my-skill/
└── SKILL.md     # 唯一必需文件
```

### 完整结构

```
my-skill/
├── SKILL.md         # 必需：指令 + 元数据
├── scripts/         # 可选：可执行代码
├── references/      # 可选：参考文档
└── assets/          # 可选：模板、资源
```

### SKILL.md 模板

```markdown
---
name: my-awesome-skill
description: 一句话描述这个 Skill 做什么
---

# My Awesome Skill

## 使用场景
当需要 xxx 时使用本 Skill。

## 执行步骤
1. 第一步做什么
2. 第二步做什么
3. 第三步做什么

## 注意事项
- 注意点1
- 注意点2
```

---

## Skill 加载位置

Skills 从三个位置加载（优先级从高到低）：

| 位置 | 路径 | 说明 |
|------|------|------|
| 工作区 | `<workspace>/skills/` | 项目特定 Skills |
| 用户全局 | `~/.openclaw/skills/` | 所有项目共享 |
| 内置 | 随安装包 | 官方 Skills |


---

## ClawHub 技能市场

ClawHub 是 OpenClaw 官方的技能市场，截至 2026 年 3 月已收录 **17000+** 技能。

### 基本命令

```bash
# 安装 ClawHub CLI
npm i -g clawhub --registry https://registry.npmmirror.com

# 搜索技能
clawhub search "日历"
clawhub search "邮件处理"

# 安装技能
clawhub install <skill-name>

# 批量更新所有技能
clawhub update --all
```

> 💡 **懒人操作**：直接在对话中说"帮我安装 Gmail 技能"，AI 会自动执行安装。

---

## Top 20 必装技能清单

### 第一梯队：地基能力（必装）

| 技能 | 功能 | 安装命令 |
|------|------|---------|
| **Agent Browser** | 网页自动化（打开、填表、截图） | `clawhub install browser` |
| **Brave Search** | 联网搜索最新信息 | `clawhub install brave-search` |
| **Shell** | 终端命令执行 | `clawhub install shell` |
| **Cron** | 定时任务和提醒 | `clawhub install cron` |

### 第二梯队：聊天入口（选 1 个）

| 技能 | 适合谁 | 安装命令 |
|------|--------|---------|
| **Telegram** | 个人用户/海外 | `clawhub install telegram` |
| **飞书** | 国内团队 | `clawhub install feishu` |
| **钉钉** | 国内企业 | `clawhub install dingtalk` |
| **Discord** | 开源社区 | `clawhub install discord` |

### 第三梯队：生产力（按需装）

| 技能 | 功能 | 安装命令 |
|------|------|---------|
| **Gmail** | 邮件自动化 | `clawhub install gmail` |
| **Google Calendar** | 日程管理 | `clawhub install google-calendar` |
| **GitHub** | 代码仓库管理 | `clawhub install github` |
| **Notion** | 知识库管理 | `clawhub install notion` |
| **Obsidian** | 本地笔记管理 | `clawhub install obsidian` |

### 第四梯队：进阶玩法

| 技能 | 功能 | 安装命令 |
|------|------|---------|
| **Nodes** | 手机设备联动 | `clawhub install nodes` |
| **Skill Creator** | 自定义技能封装 | `clawhub install skill-creator` |
| **Home Assistant** | 智能家居控制 | `clawhub install home-assistant` |
| **Spotify** | 音乐播放控制 | `clawhub install spotify` |
| **Twitter/X** | 社媒运营 | `clawhub install twitter` |

### 安装安全提醒

1. **来源验证**：优先选 ClawHub 官方认证、下载量 1000+ 的技能
2. **权限管控**：对 Browser、Shell 等高危技能开启"确认模式"
3. **授权谨慎**：涉及 Gmail、GitHub 账号的技能，只勾选必要权限
4. **定期更新**：每周跑一次 `clawhub update --all`


---

## 下一节

👉 [02-内置 Skills](02-builtin-skills.md) — 53 个内置 Skills 全解析
