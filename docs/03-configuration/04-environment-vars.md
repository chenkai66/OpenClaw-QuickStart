# 第3章·第4节：环境变量

> 高级配置：通过环境变量自定义 OpenClaw 的行为。

---

## 核心环境变量

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `OPENCLAW_HOME` | 主目录 | `~/.openclaw` |
| `OPENCLAW_STATE_DIR` | 状态目录 | `$OPENCLAW_HOME/state` |
| `OPENCLAW_CONFIG_PATH` | 配置文件路径 | `$OPENCLAW_HOME/openclaw.json` |

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
