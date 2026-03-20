# 第3章·第2节：模型提供商配置

> 选择你的 AI 大脑 — 不同场景用不同模型。

---

## 支持的模型提供商

| 提供商 | 模型 | 特点 | 费用 |
|--------|------|------|------|
| Anthropic | Claude Sonnet 4.5 / Opus 4.6 | 质量最好 | 付费 |
| OpenAI | GPT-4o / GPT-4.5 | 稳定可靠 | 付费 |
| Qwen（通义千问） | qwen-max / qwen-plus | 国内首选，免费额度 | 免费起步 |
| Google | Gemini 2.5 Pro | 免费额度 | 免费起步 |
| 自定义 | 任何兼容 OpenAI API 的模型 | 灵活 | 自定 |

---

## 配置方法

### 方式一：使用 onboard 向导

```bash
openclaw onboard
# 选择模型提供商 → 按提示配置
```

### 方式二：手动配置

#### Anthropic Claude（推荐质量最好）

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

#### 自定义 API 端点（Dashscope Proxy 等）

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

> 💡 这种方式适合使用代理或私有 API 的场景

---

## 下一节

👉 [03-Tools 详解](03-tools-config.md)
