# 第4章·第6节：微信接入完全指南

> 微信直连 OpenClaw，在手机上随时随地和你的 AI 助手聊天。

---

## 微信接入方案对比

先搞清楚有哪些路子，再选适合自己的：

| 方案 | 难度 | 适合谁 | 原理 |
|------|------|--------|------|
| **WorkBuddy（推荐）** | ⭐ 最简单 | 个人用户、不想折腾 | 腾讯官方桌面智能体，通过微信客服号桥接 |
| **企业微信官方插件** | ⭐⭐ 中等 | 企业用户、小团队 | 企业微信长连接机器人 + 官方 OpenClaw 插件 |
| **openclaw-china 企微自建应用** | ⭐⭐⭐ 较复杂 | 开发者、需要深度定制 | 企业微信自建应用 API 对接 |

> 下面从最简单的 WorkBuddy 开始讲，一步一步来。

---

## 方案一：WorkBuddy（最简单，推荐个人用户）

### 什么是 WorkBuddy

WorkBuddy 是腾讯官方出品的桌面智能体，兼容 OpenClaw 技能。它能直接把你的**个人微信**变成 OpenClaw 的对话入口，不需要服务器、不需要公网 IP、不用搞企业微信认证。

核心原理：WorkBuddy 在你电脑上运行，通过企业微信的"微信客服号"通道，在你的个人微信和 AI 之间搭了一座桥。

### 前提条件

- 一台电脑（Windows 或 macOS）
- 电脑上保持 WorkBuddy 运行
- 手机上登录个人微信

### Step 1：下载安装 WorkBuddy

1. 打开 WorkBuddy 官网：https://www.codebuddy.cn/work/

![WorkBuddy 官网下载页](https://cdn.h3blog.com/ceae104a-1f64-11f1-a9ce-00163e06621a.png)

2. 选择你的操作系统版本下载
   - Windows：`WorkBuddySetup.exe`（约 150-200 MB）
   - macOS：`WorkBuddy.dmg`
3. 正常安装流程，一路下一步
4. 首次启动需要扫码登录（跳转官网页面，用微信扫码即可）

![首次扫码登录](https://cdn.h3blog.com/0de5b246-1f65-11f1-9e4c-00163e06621a.png)

### Step 2：开启 Claw 远程功能

1. 打开 WorkBuddy 客户端
2. 点击**右上角头像**
3. 进入「**Claw 设置**」

![WorkBuddy 主界面 - 点击右上角头像](https://cdn.h3blog.com/0f6f2d3c-1f65-11f1-8723-00163e06621a.png)

![点击 Claw 设置](https://cdn.h3blog.com/102d5350-1f65-11f1-a74f-00163e06621a.png)

这里你能看到多个集成选项：微信客服号、QQ、飞书、钉钉、企业微信。

### Step 3：配置微信客服号集成

1. 在集成列表中找到「**微信客服号集成**」
2. 点击右侧的「**配置**」按钮

![微信客服号集成 - 点击配置](https://cdn.h3blog.com/10e61aaf-1f65-11f1-8d88-00163e06621a.png)

3. WorkBuddy 会自动创建一个微信客服号，并显示一个**二维码**
4. 用手机**微信扫描**这个二维码（限时 300 秒，别磨蹭）

![用微信扫描二维码](https://cdn.h3blog.com/11a21d24-1f65-11f1-a8a6-00163e06621a.png)

5. 扫码成功后，页面提示「微信客服绑定成功」

![绑定成功](https://cdn.h3blog.com/b599de44-1f65-11f1-9d79-00163e06621a.png)

### Step 4：开始使用

绑定完成后，你的微信里会多出一个**客服会话入口**。打开这个会话，直接发消息就行了：

![手机微信中直接和 AI 对话](https://cdn.h3blog.com/b67d2c72-1f65-11f1-bdff-00163e06621a.jpg)

```
你：帮我总结今天桌面上最新的会议记录
AI：正在查看桌面文件...找到"会议纪要_0320.docx"，
    总结如下：1. 项目进度... 2. 下周计划...
```

WorkBuddy 会在电脑上自动执行任务，执行过程和结果都同步回微信聊天窗口。

### 注意事项

- **电脑需要保持开机且 WorkBuddy 运行**，手机才能远程操控
- WorkBuddy 除了微信，还支持 QQ、飞书、钉钉、企业微信，配置方法类似
- WorkBuddy 自带 OpenClaw 兼容的 Skill 引擎，装好就能用

![WorkBuddy 技能和插件](https://cdn.h3blog.com/b772a690-1f65-11f1-a1da-00163e06621a.png)
- 如果你已经自己部署了 OpenClaw，WorkBuddy 也可以连接你自己的实例

---

## 方案二：企业微信官方插件（推荐小团队）

2026 年 3 月，企业微信官方推出了 OpenClaw 适配插件，支持通过长连接智能机器人直接关联 OpenClaw。

### 前提条件

- 一个企业微信账号（10 人以下小团队也可以注册）
- OpenClaw 已部署并运行（本地或云服务器都行）
- OpenClaw 版本 v2026.3.7+

### Step 1：安装企业微信 OpenClaw 插件

```bash
# 安装官方插件
openclaw plugins install wecom-openclaw-plugin

# 如果已安装过，更新到最新版
openclaw plugins update wecom-openclaw-plugin
```

### Step 2：创建长连接智能机器人

有两种方式：

**方式 A：命令行扫码（最快）**

```bash
# 在终端执行，会显示二维码
openclaw wecom scan-login
```

用企业微信扫码，自动创建机器人并关联 OpenClaw。

**方式 B：企业微信管理后台**

1. 登录企业微信管理后台：https://work.weixin.qq.com/
2. 管理工具 → 智能机器人 → 创建机器人
3. 选择「API 模式创建」→ 通过**长连接**配置
4. 按照提示关联你的 OpenClaw 实例

### Step 3：验证

```bash
openclaw gateway restart
openclaw gateway status
# 应该能看到 WeChat Work channel: connected
```

在企业微信中给机器人发条消息试试。因为企业微信和个人微信互通，你的同事用个人微信也能和机器人对话。

### 支持的功能（v2026.3.19 插件）

| 功能 | 状态 |
|------|------|
| 文本消息 | ✅ |
| 图片/语音/视频/文件 | ✅ |
| Markdown 消息卡片 | ✅ |
| 创建文档和智能表格 | ✅（文档 MCP） |
| 日程、会议接口 | ✅（个人工具接口） |
| 群聊 | ✅ |
| 权限管理 | ✅（管理员可控） |

---

## 方案三：openclaw-china 企微自建应用（开发者向）

如果你需要更深度的定制，可以用 openclaw-china 社区插件对接企业微信自建应用。

### 安装

```bash
# 安装 openclaw-china 全套插件
openclaw plugins install https://github.com/BytePioneer-AI/openclaw-china.git

# 或者只装企微渠道
openclaw plugins install @openclaw-china/wecom-app
```

### 配置

需要在企业微信管理后台创建自建应用，获取 CorpID、AgentId、Secret、Token、EncodingAESKey，然后配置到 OpenClaw：

```bash
openclaw config set channels.wecom.corp_id "wwxxxxxxxxxxxxxx"
openclaw config set channels.wecom.agent_id "1000001"
openclaw config set channels.wecom.secret "xxxxxxxxxxxxxxxx"
openclaw config set channels.wecom.token "your_token"
openclaw config set channels.wecom.aes_key "your_aes_key"
```

重启网关：

```bash
openclaw gateway restart
```

这种方式的好处是可以接入**普通微信用户**（通过企业微信与个人微信互通），支持群聊、文件传输等全部功能。

---

## 方案对比总结

| | WorkBuddy | 企微官方插件 | openclaw-china 自建应用 |
|--|-----------|-------------|----------------------|
| 上手难度 | 5 分钟 | 15 分钟 | 30-60 分钟 |
| 需要服务器 | 不需要 | 需要 | 需要 |
| 需要公网 IP | 不需要 | 不需要（长连接） | 看配置 |
| 个人微信可用 | ✅ | ✅（互通） | ✅（互通） |
| 适合场景 | 个人使用 | 小团队办公 | 企业级定制 |
| 定制程度 | 低 | 中 | 高 |
| 维护成本 | 几乎为零 | 低 | 中等 |

---

## 常见问题

### WorkBuddy 扫码失败怎么办？

确保微信是最新版本，二维码 300 秒内有效，过期重新点击「配置」生成新的。

### 关掉电脑后还能用微信对话吗？

用 WorkBuddy 方案不行，它需要电脑保持运行。如果需要 7x24 服务，用企业微信插件 + 云服务器部署 OpenClaw。

### 企业微信没有 10 人以上的团队能注册吗？

可以。个人或小团队也能注册企业微信，选择"个体户"或者用个人身份证就行。

### 普通个人微信能直接当机器人吗？

可以但风险大。社区有 WeChatFerry、Gewechat 等协议模拟工具，能把个人微信变成机器人，但**违反微信用户协议，有封号风险**。除非你用小号，否则不推荐。

---

## 下一章

👉 [第5章：Skills 技能系统](../05-skills/01-understanding-skills.md) — 教 AI 怎么做事
