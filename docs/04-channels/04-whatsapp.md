# 第4章·第4节：WhatsApp 接入

> WhatsApp 是 OpenClaw 最早支持的渠道，全球超 20 亿用户。

---

## 特点

- **端到端加密**：消息安全
- **QR 码配对**：手机扫码即可连接
- **全球覆盖**：海外用户首选
- **内置渠道**：OpenClaw 原生支持

> ⚠️ **重要限制**：WhatsApp 协议限制只能在**一台设备**上运行 Web 客户端。运行 OpenClaw 的 WhatsApp 渠道后，网页版 WhatsApp 会断开。

---

## Step 1：配置 OpenClaw

```json
{
  "channels": {
    "whatsapp": {
      "enabled": true,
      "allowFrom": ["+8613800138000"],
      "dmPolicy": "pairing"
    }
  }
}
```

`allowFrom` 是手机号白名单，只有这些号码发来的消息会被处理。

---

## Step 2：扫码配对

```bash
# 启动配对流程
openclaw channels login whatsapp
```

终端会显示一个 QR 码，用手机 WhatsApp 扫描：

1. 打开手机 WhatsApp
2. 设置 → 已关联设备 → 关联设备
3. 扫描终端中的 QR 码

配对成功后，OpenClaw 会自动保存凭证，后续重启不需要重新扫码。

---

## Step 3：验证

给你自己的 WhatsApp 发一条消息（或让白名单里的号码发），等待 AI 回复。

```bash
# 检查连接状态
openclaw status
# 应该看到 WhatsApp channel: connected
```

---

## 注意事项

- **保持 OpenClaw 运行**：关掉后 WhatsApp 连接会断开
- **单设备限制**：一个 WhatsApp 账号只能连一个 OpenClaw 实例
- **凭证文件**：保存在 `~/.openclaw/` 下，迁移服务器时记得备份
- **多媒体支持**：支持收发图片、语音、视频、文件

---

## 常见问题

### 扫码后连接不上？

1. 确保手机和服务器网络通畅
2. WhatsApp 需要最新版本
3. 如果之前连过其他设备，先在手机上断开已关联设备

### 连接经常断？

1. 服务器网络不稳定，考虑换个网络环境
2. 检查是否有其他设备也在连同一个 WhatsApp 账号

---

## 下一节

👉 [05-国内 IM 总览](05-china-im.md) — 钉钉、飞书、微信、QQ
