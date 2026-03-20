# 第4章·第1节：渠道概览

> 一个 Gateway 连接所有聊天平台。

---

## 支持的渠道

### 官方内置渠道

| 渠道 | 配置难度 | 特点 |
|------|----------|------|
| Web Dashboard | ⭐ 最简单 | 浏览器直接使用 |
| Telegram | ⭐ 简单 | 只需 Bot Token |
| Discord | ⭐⭐ 中等 | 需要创建 Discord App |
| WhatsApp | ⭐⭐ 中等 | 需要扫码配对 |
| iMessage | ⭐⭐⭐ 复杂 | 仅 macOS |

### 国内 IM（通过 openclaw-china 插件）

| 渠道 | 配置难度 | 支持功能 |
|------|----------|----------|
| 钉钉 | ⭐ 简单 | 文本/Markdown/流式/图片/文件/语音/群聊 |
| QQ 机器人 | ⭐ 简单 | 文本/Markdown/流式/图片/文件/语音/群聊 |
| 企业微信（智能机器人） | ⭐ 简单 | 文本/Markdown/流式/图片/文件/语音/群聊 |
| 企业微信（自建应用） | ⭐⭐ 中等 | 可接入普通微信 |
| 微信客服 | ⭐⭐ 中等 | 面向外部微信用户 |
| 微信公众号 | ⭐⭐ 中等 | 订阅号/服务号/测试号 |

---

## 快速对比：选哪个？

| 需求 | 推荐渠道 |
|------|----------|
| 最快上手 | Telegram |
| 中国企业 | 钉钉 / 企微 |
| 个人使用 | WeChat / QQ |
| 开发调试 | Web Dashboard |

---

## 详细教程

- 👉 [Telegram 接入](02-telegram.md)
- 👉 [Discord 接入](03-discord.md)
- 👉 [WhatsApp 接入](04-whatsapp.md)
- 👉 [国内 IM 总览](05-china-im.md)
- 👉 [钉钉完全教程](../08-dingtalk-complete/01-prerequisites.md)（单独章节）
