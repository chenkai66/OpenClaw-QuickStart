# 案例5：钉钉 AI 助手

## 目标
搭建一个通过钉钉使用的 AI 助手。

## 配置文件

### openclaw.json 钉钉配置

```json
{
  "ai": {
    "provider": "qwen",
    "model": "qwen-max"
  },
  "channels": {
    "dingtalk": {
      "enabled": true,
      "clientId": "你的ClientID",
      "clientSecret": "你的Secret",
      "robotCode": "你的RobotCode",
      "corpId": "你的CorpID",
      "agentId": "你的AgentID",
      "dmPolicy": "open",
      "groupPolicy": "open",
      "messageType": "markdown",
      "debug": false
    }
  },
  "tools": {
    "read": { "enabled": true },
    "write": { "enabled": true },
    "exec": { "enabled": true },
    "web_search": { "enabled": true },
    "web_fetch": { "enabled": true },
    "memory_search": { "enabled": true },
    "memory_get": { "enabled": true },
    "cron": { "enabled": true },
    "message": { "enabled": true }
  },
  "approvals": {
    "exec": { "enabled": true }
  }
}
```

## 测试步骤

1. 安装钉钉插件：
```bash
openclaw plugins install https://github.com/BytePioneer-AI/openclaw-china.git
```

2. 配置凭证（使用 CLI）：
```bash
openclaw config set channels.dingtalk.enabled true
openclaw config set channels.dingtalk.clientId "dingxxx"
# ... 其余配置
```

3. 重启：
```bash
openclaw gateway restart
```

4. 在钉钉中搜索机器人并发送消息测试。

## 试试这些对话

```
@OpenClaw 你好，介绍一下你自己
@OpenClaw 帮我查一下北京今天的天气
@OpenClaw 生成一个随机密码，16位，包含大小写和特殊字符
@OpenClaw 帮我写一个 Python 快排算法
```
