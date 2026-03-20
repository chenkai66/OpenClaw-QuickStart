# 第4章·第3节：Discord 接入

> 让 OpenClaw 在 Discord 服务器中为你的社区服务。

---

## 为什么用 Discord

- **社区场景**：开源项目、游戏社区、学习小组
- **频道分类**：不同频道可以绑定不同 Agent
- **不需要公网 IP**：使用 Bot Gateway 模式
- **内置渠道**：配置即可用

---

## Step 1：创建 Discord Bot

1. 登录 [Discord Developer Portal](https://discord.com/developers/applications)
2. 点击 **New Application** → 输入名称
3. 左侧菜单选择 **Bot** → 点击 **Add Bot**
4. 复制 **Bot Token**
5. 打开 **Message Content Intent**（在 Bot 页面下方的 Privileged Gateway Intents 中）
6. 在 **OAuth2 → URL Generator** 中，勾选 `bot` 权限和 `Send Messages`、`Read Message History`
7. 复制生成的邀请链接，在浏览器中打开，选择你的服务器

---

## Step 2：配置 OpenClaw

```json
{
  "channels": {
    "discord": {
      "enabled": true,
      "botToken": "你的 Discord Bot Token",
      "dmPolicy": "pairing"
    }
  }
}
```

或使用环境变量：

```bash
export DISCORD_BOT_TOKEN="你的 Token"
```

---

## Step 3：配对激活

和 Telegram 一样，第一次私聊 Bot 时会收到配对码：

```bash
openclaw pairing approve discord <配对码>
```

---

## 服务器频道配置

在 Discord 服务器中，可以为不同频道设置不同的 Agent：

```json
{
  "channels": {
    "discord": {
      "groupPolicy": "open",
      "groups": {
        "频道ID": {
          "requireMention": true,
          "agentId": "work"
        }
      }
    }
  }
}
```

---

## 常见问题

### Bot 加入服务器后不回复？

1. 确认打开了 **Message Content Intent**
2. 确认 Gateway 运行中
3. 确认 Bot 有读写消息的权限

---

## 下一节

👉 [04-WhatsApp 接入](04-whatsapp.md) — 连接 WhatsApp
