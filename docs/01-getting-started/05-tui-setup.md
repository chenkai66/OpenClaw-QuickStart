# 第1章·第5节：TUI 终端界面

> 在终端里直接和 OpenClaw 对话，开发者最趁手的用法。

---

## TUI 是什么

TUI（Terminal User Interface）就是在终端里跟 OpenClaw 交互的界面，支持完整的 Agent 能力：工具调用、文件操作、代码执行等。

启动后大概长这样：
```
🦞 OpenClaw 2026.x.x
session agent:main:main

> 在这里输入你的消息
```

## 启动方式

```bash
# 先确保环境变量配好了（DashScope 为例）
export OPENAI_API_KEY="你的DashScope-API-Key"
export OPENAI_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"

# 启动 TUI
openclaw tui
```

如果你已经把环境变量写进了 `~/.zshrc`，直接 `openclaw tui` 就行。

## 常用命令

| 命令 | 说明 |
|------|------|
| `/model <名称>` | 切换模型，比如 `/model qwen3-max` |
| `/help` | 查看帮助 |
| `Ctrl+C` 或 `/exit` | 退出 |

## DashScope 可用模型

| 模型 ID | 适合场景 | 上下文长度 | 价格（每百万 token） |
|---------|---------|-----------|-------------------|
| `qwen3-max` | 复杂任务，能力最强 | 262K | 输入 2.5 / 输出 10 |
| `qwen3.5-plus` | 均衡，兼顾速度和质量 | 1M | 输入 0.8 / 输出 4.8 |
| `qwen3.5-flash` | 简单任务，速度最快 | 1M | 输入 0.2 / 输出 2 |

DashScope 还接入了第三方模型：DeepSeek、Kimi、GLM、MiniMax 等，都可以直接用。

## 查看可用模型

```bash
openclaw models list
```

## 会话存储

所有对话会自动保存到 `~/.openclaw/agents/main/sessions/`，下次打开还能接着聊。

---

*下一节：[架构设计 →](../02-core-concepts/01-architecture.md)*


---

## TUI 进阶用法

### 指定思考级别

```bash
openclaw agent --message "分析这段代码的安全性" --thinking high
```

### 指定 Agent（多 Agent 路由）

```bash
openclaw agent --message "查看今天的日程" --agent home
openclaw agent --message "审查这个 PR" --agent work
```

### 管道输入

```bash
# 让 AI 解释一段代码
cat script.py | openclaw agent --message "解释这段代码"

# 让 AI 分析日志
openclaw logs --limit 50 --plain | openclaw agent --message "有什么异常？"
```

