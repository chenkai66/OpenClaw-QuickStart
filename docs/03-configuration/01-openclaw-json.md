# 第3章·第1节：openclaw.json 配置

> OpenClaw 的所有配置都在这一个 JSON 文件里。

---

## 配置文件位置

```
~/.openclaw/openclaw.json
```

## 使用阿里云 DashScope（推荐）

DashScope 提供通义千问系列模型，也接入了第三方模型（DeepSeek、Kimi、GLM、MiniMax），统一走 OpenAI 兼容 API。

```json
{
  "models": {
    "providers": {
      "dashscope": {
        "baseUrl": "https://dashscope.aliyuncs.com/compatible-mode/v1",
        "apiKey": "你的DashScope-API-Key",
        "api": "openai-chat",
        "models": [
          {
            "id": "qwen3-max",
            "name": "Qwen3 Max",
            "description": "最强，适合复杂任务（262K 上下文）"
          },
          {
            "id": "qwen3.5-plus",
            "name": "Qwen3.5 Plus",
            "description": "均衡，兼顾质量、速度和成本（1M 上下文）"
          },
          {
            "id": "qwen3.5-flash",
            "name": "Qwen3.5 Flash",
            "description": "最快最便宜，适合简单任务（1M 上下文）"
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "dashscope/qwen3.5-plus"
      }
    }
  }
}
```

### 获取 API Key

1. 打开 https://dashscope.console.aliyun.com/
2. 注册或登录
3. 在「API Key 管理」页面创建新的 Key

---

## 使用 Anthropic

```json
{
  "models": {
    "providers": {
      "anthropic": {
        "apiKey": "你的Anthropic-API-Key"
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-sonnet-4-5-20260514"
      }
    }
  }
}
```

---

## 多模型同时配置

可以同时配好几个模型供应商，在 TUI 里用 `/model` 随时切换：

```json
{
  "models": {
    "providers": {
      "dashscope": {
        "baseUrl": "https://dashscope.aliyuncs.com/compatible-mode/v1",
        "apiKey": "你的DashScope-Key",
        "api": "openai-chat",
        "models": [
          { "id": "qwen3-max", "name": "Qwen3 Max" },
          { "id": "qwen3.5-plus", "name": "Qwen3.5 Plus" },
          { "id": "qwen3.5-flash", "name": "Qwen3.5 Flash" }
        ]
      },
      "anthropic": {
        "apiKey": "你的Anthropic-Key"
      }
    }
  }
}
```

---

## 完整配置项速查

| 配置路径 | 作用 | 推荐值 |
|---------|------|--------|
| `models.providers.*.baseUrl` | API 端点 | 按供应商填 |
| `agents.defaults.model.primary` | 默认模型 | `dashscope/qwen3.5-plus` |
| `agents.defaults.blockStreamingDefault` | 渐进式输出 | `"on"` |
| `agents.defaults.compaction.memoryFlush.enabled` | 压缩前自动保存记忆 | `true` |
| `tools.exec.enabled` | 允许执行命令 | `true` |
| `tools.web.search.enabled` | 允许网页搜索 | `true` |
| `tools.media.image.enabled` | 图片识别 | `true`（需要视觉模型） |
| `gateway.mode` | 网关模式 | `"local"` |

---

*下一节：[模型提供商配置 →](02-model-providers.md)*
