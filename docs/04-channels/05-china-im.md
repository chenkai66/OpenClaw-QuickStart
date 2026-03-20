# 第4章·第5节：国内 IM 接入总览

> 钉钉、飞书、企微、QQ、微信 — 一网打尽。

---

## openclaw-china 项目

国内 IM 接入统一使用 `openclaw-china` 插件：

**仓库**：https://github.com/BytePioneer-AI/openclaw-china

---

## 安装 openclaw-china

```bash
# 方式一：直接安装（推荐）
openclaw plugins install https://github.com/BytePioneer-AI/openclaw-china.git

# 方式二：使用 setup 向导
npx @openclaw-china/setup
```

---

## 各平台功能对比

| 功能 | 钉钉 | QQ | 企微(智能机器人) | 企微(自建应用) | 微信客服 | 微信公众号 |
|------|------|-----|-------|------|------|------|
| 文本消息 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Markdown | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |
| 流式响应 | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| 图片/文件 | ✅ | ✅ | ✅ | ✅ | 开发中 | 开发中 |
| 语音消息 | ✅ | ✅ | ✅ | ✅ | 开发中 | 开发中 |
| 私聊 | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| 群聊 | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| 定时任务 | ✅ | ✅ | ✅ | ✅ | 开发中 | 开发中 |

---

## 选择建议

| 场景 | 推荐 |
|------|------|
| 企业内部使用 | 钉钉 或 企微智能机器人 |
| 需要接入普通微信 | 企微自建应用 或 **WorkBuddy** |
| 面向外部用户 | 微信客服 或 微信公众号 |
| 个人玩耍 | QQ 机器人 |
| 不想折腾，个人微信直连 | **WorkBuddy（腾讯官方）** |

---

## 微信接入专题

微信接入比较特殊，方案比较多，单独写了一篇详细教程：

👉 [第4章·第6节：微信接入完全指南](06-wechat-workbuddy.md) — 包含 WorkBuddy、企微官方插件、openclaw-china 自建应用三种方案。

---

## 钉钉详细教程

由于钉钉是国内使用最广的企业平台，单独准备了完整的保姆级教程：

👉 [第8章：钉钉对接完全教程](../08-dingtalk-complete/01-prerequisites.md)
