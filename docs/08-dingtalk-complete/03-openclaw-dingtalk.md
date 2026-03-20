# 第8章·第3节：OpenClaw 钉钉配置

> 把钉钉凭证配置到 OpenClaw，让它们连起来。

---

## Step 1：安装钉钉插件

SSH 登录到你的 OpenClaw 服务器，执行：

```bash
# 方式一：安装 openclaw-china 全套插件（推荐）
openclaw plugins install https://github.com/BytePioneer-AI/openclaw-china.git

# 方式二：仅安装钉钉渠道
openclaw plugins install https://github.com/soimy/clawdbot-channel-dingtalk.git
```

> ⏳ 安装可能需要几分钟，耐心等待

---

## Step 2：配置钉钉凭证

### 方式一：CLI 命令配置（推荐）

```bash
# 启用钉钉渠道
openclaw config set channels.dingtalk.enabled true

# 设置 Client ID（AppKey）
openclaw config set channels.dingtalk.clientId "dingxxxxxxxxx"

# 设置 Client Secret（AppSecret）
openclaw config set channels.dingtalk.clientSecret "your-app-secret"

# 设置 Robot Code（通常与 Client ID 相同）
openclaw config set channels.dingtalk.robotCode "dingxxxxxxxxx"

# 设置 Corp ID（企业 ID）
openclaw config set channels.dingtalk.corpId "dingxxxxxxxxx"

# 设置 Agent ID（应用 ID）
openclaw config set channels.dingtalk.agentId "123456789"

# 设置消息策略
openclaw config set channels.dingtalk.dmPolicy "open"
openclaw config set channels.dingtalk.groupPolicy "open"
openclaw config set channels.dingtalk.messageType "markdown"
openclaw config set channels.dingtalk.debug false
```

### 方式二：直接编辑配置文件

```bash
# 找到配置文件
find / -name "openclaw.json" -not -path "*/node_modules/*" 2>/dev/null
# 通常在 /root/.openclaw/openclaw.json

# 编辑配置
vim ~/.openclaw/openclaw.json
```

在 `channels` 部分添加钉钉配置：

```json
{
  "channels": {
    "dingtalk": {
      "enabled": true,
      "clientId": "dingxxxxxxxxx",
      "clientSecret": "your-app-secret",
      "robotCode": "dingxxxxxxxxx",
      "corpId": "dingxxxxxxxxx",
      "agentId": "123456789",
      "dmPolicy": "open",
      "groupPolicy": "open",
      "messageType": "markdown",
      "debug": false
    }
  }
}
```

---

## Step 3：重启 Gateway

```bash
openclaw gateway restart
```

---

## Step 4：验证连接

```bash
# 查看 Gateway 状态
openclaw gateway status

# 查看日志确认钉钉连接
openclaw gateway logs | grep -i dingtalk
```

如果看到类似以下日志，说明连接成功：
```
[dingtalk] Connected to DingTalk Stream
[dingtalk] Bot is ready to receive messages
```

---

## Step 5：测试聊天

1. 打开钉钉客户端
2. 在顶部搜索栏搜索你的机器人名称
3. 点击机器人进入聊天
4. 发送 "Hello"
5. 如果收到 AI 回复，恭喜你！🎉 配置成功！

---

## 配置参数详解

| 参数 | 说明 | 可选值 |
|------|------|--------|
| `enabled` | 是否启用 | `true` / `false` |
| `clientId` | 应用 AppKey | 从钉钉开放平台获取 |
| `clientSecret` | 应用 AppSecret | 从钉钉开放平台获取 |
| `robotCode` | 机器人代码 | 通常与 clientId 相同 |
| `corpId` | 企业 ID | 从钉钉开放平台获取 |
| `agentId` | 应用 ID | 从钉钉开放平台获取 |
| `dmPolicy` | 私聊策略 | `open`（接受所有）/ `restricted` |
| `groupPolicy` | 群聊策略 | `open` / `restricted` |
| `messageType` | 消息格式 | `text` / `markdown` |
| `debug` | 调试模式 | `true` / `false` |

---

## 常见问题

### Q: 机器人收到消息但无回复

检查步骤：
1. `openclaw status` — 确认 Gateway 和 Agent 运行中
2. `openclaw gateway logs` — 查看是否有错误日志
3. 确认 AI 模型 API Key 有效

### Q: 钉钉显示"机器人不可用"

1. 确认已在钉钉开放平台发布版本
2. 确认已选择 Stream 模式
3. 重新安装钉钉插件

### Q: 连接经常断开

1. 检查服务器网络稳定性
2. 考虑使用 systemd 自动重启
3. 查看后续"生产环境部署"章节

---

## 下一节

👉 [04-高级功能](04-advanced-features.md) — 流式输出、Markdown 卡片、文件传输
