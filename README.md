![OpenClaw](https://raw.githubusercontent.com/openclaw/openclaw/main/docs/assets/openclaw-logo-text.svg)

# OpenClaw 完整教程——从零到精通

> **OpenClaw** 是 2026 年最火的开源 AI Agent 平台，GitHub 星标超过 25.1 万，登顶历史榜首。
> 这套教程带你从零开始，一步步掌握 OpenClaw。最新版本：**v2026.3.13**。

---

## 这是什么？

一套 **从 0 到 100** 的 OpenClaw 中文教程，包括：

- **9 个章节**的完整文档：从入门到进阶
- **8 个实战示例**：可直接运行的代码
- **配置模板**：拿来就用的配置文件
- **钉钉对接**：手把手完整教程
- **微信接入**：WorkBuddy / 企业微信 / openclaw-china 三种方案
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
|   |   +-- 03-tools-config.md         #   26 个 Tools 详解
|   |   +-- 04-environment-vars.md     #   环境变量
|   |   +-- 05-coding-plan.md          #   百炼 Coding Plan 接入指南
|   +-- 04-channels/                   # 第4章：渠道接入
|   |   +-- 01-overview.md             #   渠道概览
|   |   +-- 02-telegram.md             #   Telegram 接入
|   |   +-- 03-discord.md              #   Discord 接入
|   |   +-- 04-whatsapp.md             #   WhatsApp 接入
|   |   +-- 05-china-im.md             #   国内 IM 总览
|   |   +-- 06-wechat-workbuddy.md     #   ★ 微信接入完全指南
|   +-- 05-skills/                     # 第5章：Skills 技能
|   |   +-- 01-understanding-skills.md #   理解 Skills
|   |   +-- 02-builtin-skills.md       #   53 个内置 Skills 详解
|   |   +-- 03-skill-management.md     #   技能管理（安装/更新/删除）
|   |   +-- 04-custom-skills.md        #   自定义 Skills
|   +-- 06-advanced/                   # 第6章：进阶
|   |   +-- 01-memory-system.md        #   记忆系统 + ContextEngine
|   |   +-- 02-cron-jobs.md            #   定时任务
|   |   +-- 03-advanced-tips.md        #   进阶技巧
|   |   +-- 04-multi-agent.md          #   多 Agent 路由
|   |   +-- 05-mcp-integration.md      #   MCP 协议
|   |   +-- 06-best-practices.md      #   Agent 设计哲学与最佳实践
|   +-- 07-use-cases/                  # 第7章：实战项目与应用案例
|   |   +-- 01-overview.md             #   章节总览与通用前置条件
|   |   +-- 02-second-brain.md         #   第二大脑知识系统
|   |   +-- 03-daily-briefing.md       #   每日简报与健康监控
|   |   +-- 04-devops-automation.md    #   开发运维自动化
|   |   +-- 05-content-pipeline.md     #   内容研究与写作流水线
|   |   +-- 06-smart-assistant.md      #   个人智能助理
|   +-- 08-dingtalk-complete/          # 第8章：钉钉对接
|   |   +-- 01~05                      #   5 节保姆级教程
|   +-- 09-troubleshooting/           # 第9章：故障排查与 FAQ
|       +-- 01-common-issues.md        #   常见问题
|       +-- 02-faq.md                  #   FAQ
|       +-- 03-production-pitfalls.md  #   生产环境踩坑指南
```

---

## 快速开始

```bash
# 1. 安装 Node.js v22.16+（推荐 v24）
node -v

# 2. 安装 OpenClaw
npm install -g @anthropic-ai/openclaw@latest

# 3. 初始化
openclaw onboard

# 4. 启动
openclaw gateway start
```

---

## 学习路线

| 阶段 | 内容 | 预计时间 |
|------|------|---------|
| 入门 | 第1章：安装 → 第一次对话 → Web 面板 | 30 分钟 |
| 理解 | 第2章：架构 → Gateway → Agent 循环 | 1 小时 |
| 配置 | 第3章：模型 → Tools → 环境变量 → Coding Plan | 1 小时 |
| 接入 | 第4章：渠道接入（含微信 WorkBuddy） | 30 分钟 |
| 技能 | 第5章：理解 Skills → 自定义 Skills | 1 小时 |
| 进阶 | 第6章：记忆 + ContextEngine → 定时任务 → MCP → 最佳实践 | 2 小时 |
| 实战 | 第7章：6 个实战案例（知识系统/简报/运维/写作/助理） | 3 小时 |
| 钉钉 | 第8章：钉钉完整对接 | 1 小时 |
| 排错 | 第9章：常见问题 + FAQ + 生产环境踩坑指南 | 按需 |

---

## API 方案对比

| 方案 | 模型数量 | 价格 | 适合谁 |
|------|---------|------|--------|
| DashScope 免费额度 | 1-2 个 Qwen 模型 | 免费 | 新手体验 |
| DashScope 按量付费 | 全部 Qwen 模型 | 按 Token 计费 | 个人开发者 |
| **Coding Plan** | **8 个模型**（含第三方） | **200 元/月** | 重度用户 |
| Anthropic 直连 | Claude 系列 | 按 Token 计费 | 追求质量 |
| OpenAI 直连 | GPT-5.4 / GPT-4o 等 | 按 Token 计费 | 综合需求 |

---

## 相关资源

- [OpenClaw GitHub](https://github.com/openclaw/openclaw) — 25.1 万+ Stars
- [ClawHub 技能市场](https://clawhub.com) — 社区技能
- [openclaw-china](https://github.com/BytePioneer-AI/openclaw-china) — 国内 IM 插件
- [WorkBuddy](https://www.codebuddy.cn/work/) — 腾讯官方桌面智能体，微信直连
- [企业微信 OpenClaw 插件](https://work.weixin.qq.com/nl/index/openclaw) — 企微官方适配
- [阿里云百炼](https://bailian.console.aliyun.com/) — Coding Plan 订阅

---

## 参考引用

本教程参考了以下资源：

- [Hello Claw](https://datawhalechina.github.io/hello-claw/cn/) — Datawhale 社区 OpenClaw 中文教程
