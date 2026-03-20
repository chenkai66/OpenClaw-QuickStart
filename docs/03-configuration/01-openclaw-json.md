# 第3章·第1节：openclaw.json 完全指南

> 一个文件掌控一切 — OpenClaw 的所有配置都在这里。

---

## 文件位置

```bash
~/.openclaw/openclaw.json
```

查找方法：
```bash
find / -name "openclaw.json" -not -path "*/node_modules/*" 2>/dev/null
```

---

## 完整配置结构

```json
{
  // 模型提供商
  "ai": {
    "provider": "anthropic",
    "model": "claude-sonnet-4-20250514",
    "apiKey": "sk-xxx",
    "baseUrl": "https://api.anthropic.com"
  },

  // 渠道配置
  "channels": {
    "telegram": {
      "enabled": true,
      "botToken": "xxx"
    },
    "dingtalk": {
      "enabled": true,
      "clientId": "dingxxx",
      "clientSecret": "xxx"
    }
  },

  // Tools 开关
  "tools": {
    "read": { "enabled": true },
    "write": { "enabled": true },
    "exec": { "enabled": true },
    "web_search": { "enabled": true },
    "web_fetch": { "enabled": true },
    "browser": { "enabled": false },
    "memory_search": { "enabled": true },
    "cron": { "enabled": true }
  },

  // 安全审批
  "approvals": {
    "exec": { "enabled": true }
  },

  // Skills 白名单
  "skills": {
    "allowBundled": ["obsidian", "github", "gog"]
  },

  // 消息配置
  "messages": {
    "groupChat": {
      "mentionPatterns": ["@openclaw"]
    },
    "tts": {
      "auto": false
    }
  }
}
```

---

## 使用 CLI 修改配置

```bash
# 查看当前配置
openclaw config get

# 设置单个配置项
openclaw config set ai.provider "qwen"
openclaw config set ai.model "qwen-max"

# 开启/关闭工具
openclaw config set tools.browser.enabled true

# 修改后重启生效
openclaw gateway restart
```

---

## 下一节

👉 [02-模型提供商配置](02-model-providers.md)
