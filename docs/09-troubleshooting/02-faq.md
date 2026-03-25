# 第9章·第2节：FAQ 常见问题

---

## 基础问题

### Q1: OpenClaw 和 Clawdbot、Moltbot 是什么关系？
**A**: 同一个项目的不同阶段命名。Clawdbot → Moltbot → OpenClaw。

### Q2: OpenClaw 免费吗？
**A**: OpenClaw 本身是 MIT 开源免费的。但你需要为 AI 模型 API 付费（Qwen 有免费额度）。

### Q3: 支持哪些 AI 模型？
**A**: Anthropic Claude、OpenAI GPT-5.4、通义千问 Qwen、Google Gemini 3.1、DeepSeek、小米 MiMo 等。支持任何兼容 OpenAI API 的模型。

### Q4: 需要翻墙吗？
**A**: 不一定。如果用 Qwen 模型 + 钉钉渠道，完全不需要翻墙。

### Q5: 最新版本是多少？
**A**: v2026.3.13（2026年3月13日发布）。推荐使用这个版本或 v2026.3.11。

---

## 技术问题

### Q6: 数据安全吗？
**A**: 自托管模式下，数据完全在你的服务器上。AI 模型的 API 调用会将对话内容发送到模型服务商。

### Q7: 可以离线使用吗？
**A**: 不可以。需要网络连接来调用 AI 模型 API。

### Q8: 支持多人同时使用吗？
**A**: 支持。每个用户的每个渠道自动创建独立会话。

### Q9: 手机上能用吗？
**A**: 可以。通过 Telegram/钉钉/微信等聊天工具，手机上直接使用。

### Q10: ContextEngine 是什么？
**A**: v2026.3.7 引入的可插拔上下文引擎。以前记忆管理是硬编码的，现在可以像装插件一样切换不同的上下文策略（RAG、压缩、知识图谱等），不用改 Agent 配置。

---

## 微信 / WorkBuddy 相关

### Q11: 怎么用微信和 OpenClaw 对话？

**A**: 最简单的方式是装腾讯官方的 [WorkBuddy](https://www.codebuddy.cn/work/)，扫码绑定微信客服号，5 分钟搞定。详见第4章第6节。

### Q12: WorkBuddy 是什么？
**A**: 腾讯出品的桌面智能体，兼容 OpenClaw 技能。通过企业微信的微信客服号通道，让你在个人微信里直接和 AI 对话。

### Q13: WorkBuddy 关掉电脑还能用吗？
**A**: 不行，WorkBuddy 需要电脑保持运行。如果需要 7x24 服务，用企业微信官方插件 + 云服务器部署 OpenClaw。

### Q14: 企业微信有官方 OpenClaw 支持吗？
**A**: 有。2026年3月企业微信官方发布了 OpenClaw 适配插件（wecom-openclaw-plugin），支持长连接智能机器人，可用消息、文档、日程、会议等接口。

---

## 钉钉相关

### Q15: 需要公网 IP 吗？
**A**: 使用 Stream 模式不需要。

### Q16: 可以添加到群里吗？
**A**: 可以。在群设置中添加机器人即可。

### Q17: 消息有延迟怎么办？
**A**: 通常是 AI 模型处理时间。可以启用流式输出减少等待感。

---

## 安装与运行

### Q18: `npm install -g openclaw` 报 EACCES 权限不足

```bash
# 方法一：使用 nvm 管理 Node.js（推荐）
nvm install 24
nvm use 24
npm install -g openclaw

# 方法二：修改 npm 全局目录
mkdir ~/.npm-global
npm config set prefix ~/.npm-global
echo export PATH=~/.npm-global/bin:\$PATH >> ~/.bashrc
source ~/.bashrc
npm install -g openclaw
```

> 不推荐使用 sudo npm install -g，会导致后续权限问题。

### Q19: Gateway 启动报 EADDRINUSE

端口 18789 被占用：

```bash
# 查看占用进程
lsof -i :18789

# 终止占用进程
kill -9 <PID>

# 或者换一个端口
openclaw config set gateway.port 18790
```

### Q20: OpenClaw 支持哪些 AI 模型？

OpenClaw 支持任何 OpenAI 兼容 API，包括：

| 提供商 | 模型 | 说明 |
|--------|------|------|
| Anthropic | Claude Sonnet/Opus | 对话质量最佳 |
| OpenAI | GPT-4o/GPT-5.4 | 多模态能力强 |
| 通义千问 | qwen-max/qwen-plus | 国内延迟低 |
| DeepSeek | deepseek-chat | 性价比高 |
| Google | Gemini 2.0 Flash | 免费额度 |
| Ollama | 各种开源模型 | 完全本地运行 |
| OpenRouter | 聚合多模型 | 一个 Key 用多个模型 |

### Q21: 配置文件用什么格式？

OpenClaw 配置格式是 **JSON5**（不是纯 JSON）：
- 支持注释：// 这是注释
- 支持末尾逗号
- 支持不带引号的键名

### Q22: SOUL.md、USER.md 这些文件有什么区别？

| 文件 | 作用 | 类比 |
|------|------|------|
| SOUL.md | AI 人格和价值观 | 性格 |
| AGENTS.md | 工作流程和行为规范 | 工作手册 |
| USER.md | 用户信息和偏好 | 用户档案 |
| HEARTBEAT.md | 自动化巡检任务 | 自主意识 |
| MEMORY.md | 记忆索引 | 记忆 |
| IDENTITY.md | 对外显示名称和形象 | 外表 |
| BOOTSTRAP.md | 首次引导（用完删除） | 出生引导 |

---

👉 [返回目录](../../README.md)
