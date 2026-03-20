# 第10章：百炼 Coding Plan 接入指南

> Coding Plan 是阿里云百炼推出的编程工具订阅方案，200 元/月包含 8 个模型，性价比很高。

---

## Coding Plan 是什么

百炼 Coding Plan 是阿里云为编程工具（OpenClaw、Claude Code、Cursor 等）推出的专属订阅方案。跟按量付费的 DashScope API 不同，Coding Plan 月付固定费用，包含多个模型的使用额度。

| 方案 | 月费 | 包含模型 | 适合 |
|------|------|---------|------|
| 基础版 | 免费 | qwen3.5-plus（有额度限制） | 体验尝鲜 |
| 专业版 | 200 元/月 | 8 个模型 | 日常开发 |

专业版包含的 8 个模型：
- **qwen3.5-plus**：均衡，日常首选
- **qwen3-max-2026-01-23**：Qwen 最强，复杂推理
- **qwen3-coder-next**：代码生成专用
- **qwen3-coder-plus**：1M 上下文，长文档处理
- **MiniMax-M2.5**：快速响应
- **glm-5**：智谱 GLM 最新款
- **glm-4.7**：智谱 GLM
- **kimi-k2.5**：Moonshot 出品，支持图片

---

## 第一步：购买 Coding Plan

1. 打开[百炼 Coding Plan 页面](https://bailian.console.aliyun.com/#/coding-plan)
2. 选择「专业版」，完成支付
3. 在「API Key 管理」页面创建新的 API Key

> 注意：Coding Plan 的 Key 以 `sk-sp-` 开头，跟普通 DashScope Key（`sk-` 开头）不一样。

---

## 第二步：配置环境变量

```bash
export OPENAI_API_KEY="sk-sp-你的CodingPlan密钥"
export OPENAI_BASE_URL="https://coding.dashscope.aliyuncs.com/v1"
```

写入 `~/.zshrc` 或 `~/.bashrc` 让它永久生效。

> **再强调一遍**：Base URL 是 `coding.dashscope.aliyuncs.com`，不是 `dashscope.aliyuncs.com`。搞混了会一直按量扣费。

---

## 第三步：配置 openclaw.json

把完整配置写到 `~/.openclaw/openclaw.json`：

```json
{
  "models": {
    "providers": {
      "bailian": {
        "baseUrl": "https://coding.dashscope.aliyuncs.com/v1",
        "apiKey": "sk-sp-你的CodingPlan密钥",
        "api": "openai-chat",
        "models": [
          {
            "id": "qwen3.5-plus",
            "name": "qwen3.5-plus",
            "input": ["text", "image"],
            "contextWindow": 1000000,
            "maxTokens": 65536
          },
          {
            "id": "qwen3-max-2026-01-23",
            "name": "qwen3-max-2026-01-23",
            "input": ["text"],
            "contextWindow": 262144,
            "maxTokens": 65536
          },
          {
            "id": "qwen3-coder-next",
            "name": "qwen3-coder-next",
            "input": ["text"],
            "contextWindow": 262144,
            "maxTokens": 65536
          },
          {
            "id": "qwen3-coder-plus",
            "name": "qwen3-coder-plus",
            "input": ["text"],
            "contextWindow": 1000000,
            "maxTokens": 65536
          },
          {
            "id": "MiniMax-M2.5",
            "name": "MiniMax-M2.5",
            "input": ["text"],
            "contextWindow": 196608,
            "maxTokens": 32768
          },
          {
            "id": "glm-5",
            "name": "glm-5",
            "input": ["text"],
            "contextWindow": 202752,
            "maxTokens": 16384
          },
          {
            "id": "glm-4.7",
            "name": "glm-4.7",
            "input": ["text"],
            "contextWindow": 202752,
            "maxTokens": 16384
          },
          {
            "id": "kimi-k2.5",
            "name": "kimi-k2.5",
            "input": ["text", "image"],
            "contextWindow": 262144,
            "maxTokens": 32768
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": { "primary": "bailian/qwen3.5-plus" },
      "models": {
        "bailian/qwen3.5-plus": {},
        "bailian/qwen3-max-2026-01-23": {},
        "bailian/qwen3-coder-next": {},
        "bailian/qwen3-coder-plus": {},
        "bailian/MiniMax-M2.5": {},
        "bailian/glm-5": {},
        "bailian/glm-4.7": {},
        "bailian/kimi-k2.5": {}
      }
    }
  },
  "gateway": { "mode": "local" }
}
```

> 这份配置参考了[阿里云官方文档](https://help.aliyun.com/zh/model-studio/openclaw-coding-plan)。

---

## 第四步：生效配置

```bash
openclaw gateway restart
```

验证：
```bash
openclaw status
```

## 第五步：在 TUI 中切换模型

```bash
openclaw tui
```

用 `/model` 切换：
```
/model qwen3.5-plus       # 默认，均衡
/model kimi-k2.5           # 推理强
/model qwen3-coder-next    # 写代码首选
/model glm-5               # 备选
```

---

## 模型选择建议

| 任务类型 | 推荐模型 | 原因 |
|---------|---------|------|
| 日常聊天 | qwen3.5-plus | 均衡，质量/速度/成本都不错 |
| 复杂推理 | qwen3-max-2026-01-23 | Qwen 最强款 |
| 写代码 | qwen3-coder-next | 专门为编程优化 |
| 长文档处理 | qwen3-coder-plus | 1M 上下文窗口 |
| 图片理解 | qwen3.5-plus 或 kimi-k2.5 | 支持多模态 |
| 快速简单任务 | MiniMax-M2.5 | 响应快 |

---

## 注意事项

1. **仅限交互式使用**：Coding Plan 是给编程工具（OpenClaw、Claude Code 等）用的，不能用于 API 自动化脚本或批量处理
2. **不能共享账号**：订阅是个人的
3. **数据使用**：输入输出数据可能用于服务改进（参见阿里云服务条款 5.2）

---

## 常见问题

### "买了 Coding Plan 还在按量扣费"

你用错 Key 了。确认：
- Key 以 `sk-sp-` 开头（不是 `sk-`）
- Base URL 是 `coding.dashscope.aliyuncs.com`（不是 `dashscope.aliyuncs.com`）

### "找不到模型"

检查 `models` 数组里的 8 个模型 ID 是否写对了，要和上面配置中的完全一致。

### "请求频率超限"

专业版限制：5 小时内 6000 次请求。如果用得很猛，注意控制节奏。

---

*下一节：[进阶技巧 →](../06-advanced/03-advanced-tips.md)*
