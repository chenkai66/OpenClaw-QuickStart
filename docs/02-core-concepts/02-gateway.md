# 第2章·第2节：Gateway 网关系统

> Gateway 是 OpenClaw 的心脏 — 所有消息的进出都经过它。

---

## 什么是 Gateway

Gateway 是一个常驻进程，负责：
1. **接收**来自各渠道（钉钉、Telegram 等）的消息
2. **路由**消息到正确的 Agent 和会话
3. **管理**会话状态和身份验证
4. **返回** Agent 的回复到对应渠道

---

## Gateway 管理命令

```bash
# 查看状态
openclaw gateway status

# 启动
openclaw gateway start

# 停止
openclaw gateway stop

# 重启（修改配置后必须重启）
openclaw gateway restart

# 查看日志
openclaw gateway logs
```

---

## 会话路由规则

Gateway 根据以下维度路由消息：

| 维度 | 说明 |
|------|------|
| 渠道来源 | 区分 Telegram、钉钉、Web UI |
| 发送者 ID | 区分不同用户 |
| 群/私聊 | 区分群聊和私聊 |
| 工作空间 | 隔离不同工作上下文 |

---

## 安全控制

### 白名单模式

```json
{
  "channels": {
    "whatsapp": {
      "allowFrom": ["+15555550123"]
    }
  }
}
```

### 群聊 @ 触发

```json
{
  "channels": {
    "whatsapp": {
      "groups": {
        "*": { "requireMention": true }
      }
    }
  },
  "messages": {
    "groupChat": {
      "mentionPatterns": ["@openclaw"]
    }
  }
}
```

---

## 下一节

👉 [03-Agent 循环](03-agent-loop.md) — 了解 AI 如何思考和执行
