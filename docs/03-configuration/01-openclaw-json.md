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


---

## 配置结构总览

`openclaw.json` 的完整顶层结构：

```json
{
  "meta": {},          // 元数据（系统自动维护）
  "wizard": {},        // 向导记录
  "auth": {},          // 认证配置（API Key 等）
  "models": {},        // 模型提供商和可用模型
  "agents": {},        // Agent 配置（模型/工作区/压缩）
  "channels": {},      // 渠道配置（Telegram/钉钉/微信等）
  "gateway": {},       // 网关配置（端口/认证/TLS）
  "memory": {},        // 记忆系统配置
  "plugins": {},       // 插件配置
  "tools": {},         // 工具配置
  "env": {},           // 内联环境变量
  "bindings": [],      // Agent 绑定（渠道→Agent 映射）
  "cron": {},          // 定时任务
  "hooks": {}          // 生命周期钩子
}
```

> 💡 **JSON5 语法**：OpenClaw 支持 JSON5，可以写注释、尾逗号、单引号。

---

## 配置编辑方式

| 方式 | 命令 / 入口 | 适合谁 |
|------|------------|--------|
| **交互向导** | `openclaw onboard` / `openclaw configure` | 新手 |
| **CLI 单条** | `openclaw config set agents.defaults.model.primary "openai/gpt-4o"` | 老手 |
| **Web Dashboard** | 浏览器打开 `http://127.0.0.1:18789` → Config 标签 | 可视化 |
| **直接编辑** | `vim ~/.openclaw/openclaw.json` | 极客 |

> 配置文件改动后 Gateway **自动热重载**，大部分情况不需要手动重启。

