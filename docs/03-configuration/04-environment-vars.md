# 第3章·第4节：环境变量

> 高级配置：通过环境变量自定义 OpenClaw 的行为。

---

## 核心环境变量

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `OPENCLAW_HOME` | 主目录 | `~/.openclaw` |
| `OPENCLAW_STATE_DIR` | 状态目录 | `$OPENCLAW_HOME/state` |
| `OPENCLAW_CONFIG_PATH` | 配置文件路径 | `$OPENCLAW_HOME/openclaw.json` |
| `OPENCLAW_GATEWAY_TOKEN` | Gateway 认证 Token | 无 |
| `OPENCLAW_GATEWAY_PASSWORD` | Gateway 认证密码 | 无 |
| `OPENCLAW_LOAD_SHELL_ENV` | 从 Shell 导入环境变量（`1` 启用） | `0` |

---

## 模型 API Key 环境变量

| 变量 | 说明 |
|------|------|
| `OPENAI_API_KEY` | OpenAI API Key |
| `ANTHROPIC_API_KEY` | Anthropic (Claude) API Key |
| `DASHSCOPE_API_KEY` | 阿里云百炼 (通义千问) API Key |
| `OPENROUTER_API_KEY` | OpenRouter API Key |
| `DEEPSEEK_API_KEY` | DeepSeek API Key |
| `GROQ_API_KEY` | Groq API Key |

---

## .env 文件支持

OpenClaw 自动读取 `.env` 文件，优先级：

1. 当前工作目录的 `.env`
2. `~/.openclaw/.env`（全局）

> 两个文件都**不会覆盖**已有的环境变量。

```bash
# ~/.openclaw/.env
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxx
DASHSCOPE_API_KEY=sk-xxxxxxxxxxxxxxxx
OPENCLAW_GATEWAY_TOKEN=my-secret-token
```

---

## 在配置文件中引用环境变量

在 `openclaw.json` 里用 `${VAR_NAME}` 引用环境变量：

```json
{
  "gateway": {
    "auth": {
      "token": "${OPENCLAW_GATEWAY_TOKEN}"
    }
  },
  "models": {
    "providers": {
      "openai": {
        "apiKey": "${OPENAI_API_KEY}"
      },
      "dashscope": {
        "apiKey": "${DASHSCOPE_API_KEY}"
      }
    }
  }
}
```

**规则**：
- 仅匹配大写名称：`[A-Z_][A-Z0-9_]*`
- 缺失或空值会在加载时报错
- 用 `$${VAR}` 转义，输出字面值
- 支持内联拼接：`"${BASE_URL}/v1"` → `"https://api.example.com/v1"`

---

## 配置热重载

Gateway 会监视 `openclaw.json`，改配置**不需要重启**（大部分情况）：

| 重载模式 | 行为 |
|----------|------|
| `hybrid`（默认） | 安全更改立即生效，关键更改自动重启 |
| `hot` | 仅热应用安全更改，需重启时打日志提醒 |
| `restart` | 任何更改都重启 Gateway |
| `off` | 关闭监视，改完手动重启 |

```json
{
  "gateway": {
    "reload": { "mode": "hybrid", "debounceMs": 300 }
  }
}
```

**哪些能热重载、哪些不能？**

| 配置类别 | 需要重启？ |
|----------|-----------|
| 渠道、Agent、模型、路由 | 否 |
| Hooks、Cron、Heartbeat | 否 |
| 会话、消息、工具、技能 | 否 |
| UI、日志、身份 | 否 |
| **Gateway 端口/认证/TLS** | **是** |
| **插件、Discovery** | **是** |

---

## 设置方法

### 临时设置

```bash
export OPENCLAW_HOME=/opt/openclaw
openclaw gateway start
```

### 永久设置

```bash
echo 'export OPENCLAW_HOME=/opt/openclaw' >> ~/.bashrc
source ~/.bashrc
```

### systemd 服务中设置

```ini
[Service]
Environment="OPENCLAW_HOME=/opt/openclaw"
```

---

## 下一章

👉 [第4章：渠道接入](../04-channels/01-overview.md) — 连接你的聊天工具
