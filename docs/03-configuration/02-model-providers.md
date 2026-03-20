# 第3章·第2节：模型提供商配置

> 选择你的 AI 大脑 — 不同场景用不同模型。

---

## 支持的模型提供商

| 提供商 | 模型 | 特点 | 费用 |
|--------|------|------|------|
| Anthropic | Claude Sonnet 4.5 / Opus 4.6 | 质量最好 | 付费 |
| OpenAI | GPT-5.4 / GPT-4o | 综合最强 | 付费 |
| Qwen（通义千问） | qwen-max / qwen-plus | 国内首选，免费额度 | 免费起步 |
| Google | Gemini 3.1 Flash-Lite / Gemini 2.5 Pro | 免费额度 | 免费起步 |
| DeepSeek | deepseek-chat / deepseek-reasoner | 性价比高 | 付费（便宜） |
| 自定义 | 任何兼容 OpenAI API 的模型 | 灵活 | 自定 |

> v2026.3.7 新增了 GPT-5.4 和 Gemini 3.1 Flash-Lite 的原生支持，不用再配置自定义端点。

---

## 配置方法

### 方式一：使用 onboard 向导

```bash
openclaw onboard
# 选择模型提供商 → 按提示配置
```

### 方式二：手动配置

#### Anthropic Claude（质量最好）

```json
{
  "ai": {
    "provider": "anthropic",
    "model": "claude-sonnet-4-20250514",
    "apiKey": "sk-ant-xxx"
  }
}
```

#### 通义千问 Qwen（国内推荐）

```json
{
  "ai": {
    "provider": "qwen",
    "model": "qwen-max",
    "apiKey": "sk-xxx"
  }
}
```

#### OpenAI GPT-5.4（2026 最新）

```json
{
  "ai": {
    "provider": "openai",
    "model": "gpt-5.4",
    "apiKey": "sk-xxx"
  }
}
```

#### DeepSeek（性价比之王）

```json
{
  "ai": {
    "provider": "custom",
    "model": "deepseek-chat",
    "apiKey": "sk-xxx",
    "baseUrl": "https://api.deepseek.com/v1"
  }
}
```

#### 自定义 API 端点（代理、私有部署等）

```json
{
  "ai": {
    "provider": "custom",
    "model": "claude-sonnet-4-20250514",
    "apiKey": "sk-xxx",
    "baseUrl": "http://localhost:8080/v1"
  }
}
```

---

## 模型路由与自动降级

v2026.3.7 引入了更强的模型路由机制：

- 主模型被限流时，自动切换到备用模型
- 可以按任务复杂度选择不同模型（简单问题用便宜模型，复杂任务用强模型）
- 支持多提供商链式配置

```json
{
  "ai": {
    "provider": "anthropic",
    "model": "claude-sonnet-4-20250514",
    "apiKey": "sk-ant-xxx"
  },
  "fallbackModels": [
    {
      "provider": "qwen",
      "model": "qwen-plus",
      "apiKey": "sk-xxx"
    }
  ]
}
```

---

## 下一节

👉 [03-Tools 详解](03-tools-config.md)
