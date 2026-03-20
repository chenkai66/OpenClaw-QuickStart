# 第4章·第2节：Telegram 接入

> Telegram 是 OpenClaw 最成熟的渠道之一，15 分钟搞定。

---

## 为什么用 Telegram

- **跨平台**：手机、电脑、网页都能用
- **推送通知**：消息秒到
- **群聊支持**：团队共享 AI
- **不需要公网 IP**：OpenClaw 用长轮询模式主动拉消息
- **内置渠道**：不需要装插件，配置就能用

---

## Step 1：创建 Telegram Bot

1. 在 Telegram 中搜索 **@BotFather**（注意认准蓝色认证对勾）
2. 发送 `/newbot`
3. 输入 Bot 的**显示名称**（如"我的 AI 助手"）
4. 输入 Bot 的**用户名**（必须以 `_bot` 结尾，如 `my_ai_bot`）
5. BotFather 返回 **API Token**，格式类似：`123456789:ABCdefGHIjklmnoPQRstuvWXYZ`

> ⚠️ **Token 就是密码**，不要泄露到公开代码仓库或群聊中！

---

## Step 2：配置 OpenClaw

### 方式一：onboard 向导（推荐新手）

```bash
openclaw onboard
# 选择 Telegram (Bot API) → 粘贴 Token
```

### 方式二：手动编辑配置

```json
{
  "channels": {
    "telegram": {
      "enabled": true,
      "botToken": "123456789:ABCdefGHIjklmnoPQRstuvWXYZ",
      "dmPolicy": "pairing"
    }
  }
}
```

也可以通过环境变量设置 Token：

```bash
export TELEGRAM_BOT_TOKEN="123456789:ABCdefGHIjklmnoPQRstuvWXYZ"
```

---

## Step 3：配对激活

第一次给 Bot 发消息时，你会收到一个**配对码**（而不是 AI 回复）。

在服务器端执行：

```bash
openclaw pairing approve telegram <配对码>
```

配对成功后，再发消息就能收到 AI 回复了。

> 配对码 1 小时内有效，过期需要重新发起。

---

## 私聊策略（dmPolicy）

| 策略 | 行为 | 适合谁 |
|------|------|--------|
| `pairing`（默认） | 新用户需要服务器端批准 | 个人使用 |
| `allowlist` | 只有白名单用户能对话 | 团队使用 |
| `open` | 任何人都能直接对话 | 公开服务 |
| `disabled` | 关闭私聊 | 仅群组使用 |

---

## 群组配置

把 Bot 加入群组后，可以配置响应规则：

```json
{
  "channels": {
    "telegram": {
      "groupPolicy": "open",
      "groups": {
        "-1001234567890": {
          "requireMention": true,
          "agentId": "default"
        }
      }
    }
  }
}
```

- `requireMention: true`：必须 @Bot 名字才回复（防刷屏）
- `groupPolicy: "open"`：群里所有人都能触发 Bot

> 💡 获取群组 ID：向 `@userinfobot` 发消息，或在 Bot API `getUpdates` 中查看。

---

## 常见问题

### Bot 不回复？

1. 确认 Gateway 运行中：`openclaw status`
2. 确认 Token 正确：`openclaw config get channels.telegram.botToken`
3. 确认已完成配对
4. 查看日志：`openclaw gateway --verbose`

### 群聊中 Bot 不说话？

1. 检查 `groupPolicy` 是否为 `open` 或 `allowlist`
2. 确认 `requireMention` 设置
3. 在 BotFather 中关闭隐私模式：发送 `/setprivacy` → 选 Disable

---

## 进阶配置（官方文档）

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "your-token",
      dmPolicy: "pairing",
      // 流式输出（边生成边发送）
      streaming: "partial",       // off | partial | block | progress
      // 自定义命令
      customCommands: [
        { command: "backup", description: "Git 备份" },
        { command: "generate", description: "生成图片" },
      ],
      // Webhook 模式（替代长轮询，适合生产环境）
      webhookUrl: "https://example.com/telegram-webhook",
      webhookSecret: "secret",
      // 代理（国内访问 Telegram API）
      proxy: "socks5://localhost:9050",
      // 历史消息
      historyLimit: 50,
      // 媒体限制
      mediaMaxMb: 100,
      // 回复模式
      replyToMode: "first",       // off | first | all
      // 链接预览
      linkPreview: true,
      // 表情回复通知
      reactionNotifications: "own", // off | own | all
    },
  },
}
```

> **国内用户提示**：Telegram API 在国内需要代理。可以用 `proxy` 字段设置 SOCKS5 代理，或在服务器层面配置。

> 来源：[官方配置参考](https://docs.openclaw.ai/gateway/configuration-reference)

---

## 下一节

👉 [03-Discord 接入](03-discord.md) — 连接 Discord
