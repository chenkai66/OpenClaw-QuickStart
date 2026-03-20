# 第5章·第4节：编写自定义 Skills（含实际案例）

> 从真实生产环境中总结出来的 SKILL.md 写法。

## Skill 文件结构

每个 Skill 是一个目录，核心是 `SKILL.md` 文件。OpenClaw 会在相关场景下自动加载这些文件到 Agent 上下文。

```
~/.openclaw/skills/
+-- my-custom-skill/
    +-- SKILL.md          # 必须有：Skill 定义文件
    +-- config.json       # 可选：配置文件
    +-- README.md         # 可选：说明文档
```

## 实例 1：知识同步 Agent

这是一个真实的生产 SKILL.md，通过 Cron 每小时自动执行，处理对话记录并提取知识：

```markdown
# KNOWLEDGE-AGENT Skill

> 你是一个由 Cron 调用的子 Agent，负责执行完整的对话摘要数据流水线。

## 重要提示：Cron 配置

**你不需要创建或修改 Cron 任务！** 定时任务由主 Agent 管理。

如果用户要求配置 Cron，告诉他们：

### 第一步：找到项目路径

\`\`\`bash
ls -la ~/openclaw/workspace/openclaw-second-brain 2>/dev/null || \
find ~ -type d -name "openclaw-second-brain" 2>/dev/null | head -1
\`\`\`

### 第二步：创建 Cron 任务

\`\`\`bash
openclaw cron add \
  --name "Knowledge Sync" \
  --cron "0 * * * *" \
  --session isolated \
  --message "cd <项目路径> && npm run agent:knowledge"
\`\`\`
```

**这个 Skill 的设计要点**：
1. 开头就说清楚自己是干嘛的
2. 提前拦截用户的错误请求（"不需要你创建 Cron"）
3. 给出可直接复制的命令

---

## 实例 2：深度研究 Agent

一个能分析社区讨论、做行业研究的 Skill：

```markdown
# DEEP-RESEARCH Skill

> 你是一个深度研究助手，擅长从多个来源收集信息并生成研究报告。

## 触发条件
用户说"帮我研究一下……"或"调研一下……"

## 工作流程

### 第一步：理解需求
- 确认研究主题
- 确认研究深度（快速概览 / 深入分析）
- 确认输出格式

### 第二步：多源搜索
- Google 搜索技术文章
- GitHub 搜索相关项目
- HackerNews / Reddit 搜索社区讨论

### 第三步：整合分析
- 汇总各方观点
- 标注共识和争议
- 提炼可执行建议

### 第四步：生成报告
保存到 `content/reports/YYYY-MM-DD-主题.md`
```

---

## 实例 3：项目开发 Agent

一个多阶段工作流 Skill，用于从 0 到 1 构建项目：

```markdown
# PROJECT-DEVELOPER Skill

> 你是一个资深项目开发专家，擅长从 0 到 1 构建可变现的项目。

## 工作阶段

### 阶段一：调研（2-4 小时）
- 市场分析和可行性研究
- 竞品分析
- 商业模式设计
- MVP 规划

### 阶段二：设计（2-3 小时）
- 技术选型
- 架构设计
- 开发排期
- 风险评估

### 阶段三：开发（12-16 小时）
- 环境搭建
- 核心功能开发
- 测试
- 部署上线

### 阶段四：运营（持续）
- 用户获取
- 数据分析
- 迭代优化

## 虚拟团队角色
- 产品经理：需求、UX、优先级
- 技术架构师：选型、架构、代码审查
- 前端开发：UI、交互、前端优化
- 后端开发：API、数据库、业务逻辑
- 测试工程师：用例、自动化、质量保障
```

---

## 自己写 Skill 的模板

```markdown
# MY-SKILL-NAME Skill

> 一句话描述这个 Skill 干什么

## 触发条件

描述什么时候应该激活这个 Skill。

## 输入

Agent 需要什么信息？

| 参数 | 说明 | 必填 | 示例 |
|------|------|------|------|
| topic | 研究主题 | 是 | "React hooks" |
| depth | 分析深度 | 否 | "detailed" |

## 工作流程

### 第一步：[动作名称]
详细说明……

### 第二步：[动作名称]
详细说明……

## 输出格式

\`\`\`markdown
# 输出标题
## 第一部分
## 第二部分
\`\`\`

## 注意事项

- 重要提醒 1
- 重要提醒 2
```

## Skill 配置文件（config.json）

```json
{
  "contentDirectories": {
    "notes": "content/notes",
    "logs": "content/logs",
    "reports": "content/reports"
  },
  "processing": {
    "batchSize": 10,
    "minConversationLength": 50,
    "similarityThreshold": 0.7
  },
  "schedule": {
    "syncInterval": "0 * * * *",
    "reportTime": "0 23 * * *"
  }
}
```

---

*下一节：[记忆系统 →](../06-advanced/01-memory-system.md)*
