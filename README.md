# OpenClaw 完整教程——从零到精通

> **OpenClaw** 是 2026 年最火的开源 AI Agent 平台，GitHub 星标超过 24.8 万，登顶历史榜首。
> 这套教程带你从零开始，一步步掌握 OpenClaw。

---

## 这是什么？

一套 **从 0 到 100** 的 OpenClaw 中文教程，包括：

- **10 个章节**的完整文档：从入门到进阶
- **8 个实战示例**：可直接运行的代码
- **配置模板**：拿来就用的配置文件
- **钉钉对接**：手把手完整教程
- **百炼 Coding Plan**：阿里云官方 8 模型订阅方案指南

---

## 项目结构

```
openclaw-tutorial/
+-- README.md
+-- docs/
|   +-- 01-getting-started/            # 第1章：快速入门
|   |   +-- 01-what-is-openclaw.md     #   OpenClaw 是什么
|   |   +-- 02-installation.md         #   安装（全平台）
|   |   +-- 03-first-chat.md           #   第一次对话
|   |   +-- 04-web-dashboard.md        #   Web 控制面板
|   |   +-- 05-tui-setup.md            #   TUI 终端界面
|   +-- 02-core-concepts/             # 第2章：核心概念
|   |   +-- 01-architecture.md         #   6 层架构
|   |   +-- 02-gateway.md              #   Gateway 网关
|   |   +-- 03-agent-loop.md           #   Agent 循环
|   |   +-- 04-workspace-files.md      #   SOUL/USER/AGENTS/HEARTBEAT/MEMORY
|   |   +-- 05-sessions.md             #   会话管理
|   +-- 03-configuration/             # 第3章：配置详解
|   |   +-- 01-openclaw-json.md        #   openclaw.json 指南
|   |   +-- 02-model-providers.md      #   模型提供商配置
|   |   +-- 03-tools-config.md         #   Tools 工具配置
|   |   +-- 04-environment-vars.md     #   环境变量
|   +-- 04-channels/                  # 第4章：渠道接入
|   |   +-- 01-overview.md             #   渠道概览
|   |   +-- 05-china-im.md             #   国内 IM（钉钉/微信/飞书）
|   +-- 05-skills/                    # 第5章：Skills 系统
|   |   +-- 01-understanding-skills.md #   理解 Skills
|   |   +-- 04-custom-skills.md        #   自定义 Skill 编写
|   +-- 06-advanced/                  # 第6章：进阶功能
|   |   +-- 01-memory-system.md        #   记忆系统
|   |   +-- 02-cron-jobs.md            #   定时任务
|   |   +-- 03-advanced-tips.md        #   进阶技巧与配置速查
|   |   +-- 05-mcp-integration.md      #   MCP 协议集成
|   +-- 07-use-cases/                 # 第7章：实战案例
|   |   +-- 01-second-brain.md         #   第二大脑知识系统
|   +-- 08-dingtalk-complete/         # 第8章：钉钉对接完全教程
|   |   +-- 01-prerequisites.md        #   前置准备
|   |   +-- 02-dingtalk-app-setup.md   #   钉钉应用创建
|   |   +-- 03-openclaw-dingtalk.md    #   OpenClaw 钉钉配置
|   |   +-- 04-advanced-features.md    #   高级功能
|   |   +-- 05-production-deploy.md    #   生产环境部署
|   +-- 09-troubleshooting/           # 第9章：故障排查
|   |   +-- 01-common-issues.md        #   常见问题
|   |   +-- 02-faq.md                  #   FAQ
|   +-- 10-coding-plan/              # 第10章：百炼 Coding Plan
|       +-- 01-coding-plan-guide.md    #   Coding Plan 接入指南
+-- examples/                         # 实战代码示例
+-- config/                           # 配置模板
|   +-- templates/
|       +-- basic-config.json          #   DashScope 按量付费
|       +-- advanced-config.json       #   DashScope + 第三方模型
|       +-- coding-plan-config.json    #   Coding Plan 配置
|       +-- dingtalk-config.json       #   钉钉对接配置
+-- scripts/
```

---

## 快速开始

### 1. 安装 OpenClaw（5 分钟）

```bash
# Linux / macOS
curl -fsSL https://openclaw.ai/install.sh | bash

# Windows PowerShell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### 2. 初始化

```bash
openclaw onboard --install-daemon
```

### 3. 开始对话

```bash
openclaw tui         # 终端界面
# 或
openclaw dashboard   # Web 控制面板
```

---

## 学习路线

| 阶段 | 章节 | 时间 | 目标 |
|------|------|------|------|
| 入门 | 第 1-2 章 | 2 小时 | 安装，理解核心概念 |
| 进阶 | 第 3-5 章 | 4 小时 | 配置、渠道接入、Skills |
| 高手 | 第 6、10 章 | 4 小时 | 记忆、Cron、Coding Plan、进阶技巧 |
| 实战 | 第 7-8 章 | 3 小时 | 实际项目、钉钉对接 |

---

## API 方案对比

| 方案 | 费用 | 模型 | 适合 |
|------|------|------|------|
| **DashScope 按量付费** | 按 token 计 | Qwen 系列 | 轻度使用 |
| **百炼 Coding Plan**（推荐） | 200 元/月 | 8 个模型 | 日常开发 |
| **Anthropic** | 按 token 计 | Claude 系列 | 海外用户 |

详见[第 10 章：百炼 Coding Plan](docs/10-coding-plan/01-coding-plan-guide.md)。

---

## 相关资源

| 资源 | 链接 |
|------|------|
| 官方 GitHub | https://github.com/openclaw/openclaw |
| 官方文档 | https://docs.openclaw.ai |
| ClawHub 技能市场 | https://clawhub.ai |
| Awesome OpenClaw | https://github.com/rohitg00/awesome-openclaw |
| OpenClaw 中国插件 | https://github.com/BytePioneer-AI/openclaw-china |

---

## 许可证

MIT 协议——随便用、随便改、随便分享。
