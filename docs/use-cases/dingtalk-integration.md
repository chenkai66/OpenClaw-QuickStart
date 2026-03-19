# 钉钉集成 OpenClaw 完整教程

将 OpenClaw 接入钉钉，打造企业级智能助手！

---

## 📖 目录

1. [前置准备](#前置准备)
2. [钉钉机器人配置](#钉钉机器人配置)
3. [OpenClaw 插件安装](#openclaw-插件安装)
4. [完整集成流程](#完整集成流程)
5. [实际使用案例](#实际使用案例)
6. [故障排查](#故障排查)

---

## 🎯 前置准备

### 系统要求

- ✅ 已安装 OpenClaw (v1.0+)
- ✅ 钉钉企业账号（具备开发者权限）
- ✅ 稳定的网络连接

### 检查 OpenClaw 安装

```bash
# 检查版本
openclaw --version  # 应显示 1.0+

# 检查状态
openclaw health

# 启动 Gateway
openclaw gateway
```

---

## 🤖 钉钉机器人配置

### 方式1: 群聊自定义机器人（快速）

适合快速测试和小团队使用。

#### 步骤 1: 创建自定义机器人

1. 打开钉钉群聊
2. 点击右上角 `...` → `智能群助手`
3. 点击 `添加机器人` → `自定义`
4. 设置机器人信息：
   - **名称**: OpenClaw 助手
   - **头像**: 上传图片
   - **描述**: AI 智能助手

#### 步骤 2: 配置安全设置

**推荐：加签验证（最安全）**

```
✅ 加签验证
Secret: SECxxxxxxxxxxxxxxxxxxxx (复制保存)
```

**或者：关键词验证**

```
✅ 自定义关键词
关键词: openclaw
```

#### 步骤 3: 获取 Webhook 地址

```
Webhook 地址:
https://oapi.dingtalk.com/robot/send?access_token=xxxxxxxxxxxxxxxxxxxxxxxx
```

**复制保存以下信息**：
- ✅ Webhook URL
- ✅ Secret（如果使用加签）

---

### 方式2: 企业内部应用（推荐）

适合企业正式使用，功能更强大。

#### 步骤 1: 登录钉钉开放平台

访问 [https://open-dev.dingtalk.com](https://open-dev.dingtalk.com)

#### 步骤 2: 创建应用

1. 进入 `应用开发` → `企业内部开发`
2. 点击 `创建应用`
3. 填写应用信息：
   - **应用名称**: OpenClaw 智能助手
   - **应用描述**: AI Agent 运行时平台
   - **应用图标**: 上传图标

#### 步骤 3: 获取凭证

在应用详情页获取：

```
AppKey (Client ID): dingxxxxxxxxxxxxxxx
AppSecret (Client Secret): xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
AgentId: xxxxxxxxx
```

#### 步骤 4: 配置权限

进入 `权限管理`，勾选：

```
✅ 通讯录只读权限 (Contact read access)
✅ 消息发送权限 (Message sending permission)
✅ 群会话信息权限（可选）
✅ 审批流程权限（可选，如需集成审批）
```

点击 `提交`，等待管理员审核。

#### 步骤 5: 配置回调

在 `开发管理` → `事件订阅` 中配置：

```
请求网址: http://your-server-ip:18789/dingtalk/callback
加签密钥: 自动生成（复制保存）
```

---

## 🔌 OpenClaw 插件安装

### 安装钉钉插件

```bash
# 安装官方插件
npm install openclaw-channel-dingtalk

# 或使用 openclaw 命令
openclaw plugins install @soimy/dingtalk

# 验证安装
openclaw plugins list
```

---

## 🚀 完整集成流程

### 集成方案 A: 群聊机器人（Webhook）

适合快速开始，零代码配置。

#### 1. 创建配置文件

```bash
mkdir -p ~/.openclaw/dingtalk
cat > ~/.openclaw/dingtalk/webhook-config.json << 'EOFCONFIG'
{
  "webhook": "https://oapi.dingtalk.com/robot/send?access_token=YOUR_TOKEN",
  "secret": "YOUR_SECRET_KEY",
  "messageType": "text",
  "at": {
    "atAll": false
  }
}
EOFCONFIG
```

#### 2. 配置 OpenClaw 插件

```bash
openclaw plugins config @soimy/dingtalk
```

按提示输入：
- Webhook URL: (粘贴步骤1获取的 URL)
- Secret: (粘贴加签密钥)
- Message Type: 选择 `text`

#### 3. 测试连接

```bash
# 测试发送消息
openclaw plugins test @soimy/dingtalk

# 在钉钉群中应该收到测试消息
```

#### 4. 配置消息处理

创建处理脚本 `~/.openclaw/dingtalk/handler.sh`：

```bash
#!/bin/bash

# 接收钉钉消息并转发给 OpenClaw

MESSAGE="$1"

# 调用 OpenClaw 处理
openclaw tui <<< "$MESSAGE"
```

---

### 集成方案 B: 企业内部应用（完整功能）

功能更强大，支持双向通信。

#### 1. 配置应用凭证

```bash
cat > ~/.openclaw/openclaw.json << 'EOFCONFIG'
{
  "dingtalk": {
    "appKey": "dingxxxxxxxxxxxxxx",
    "appSecret": "xxxxxxxxxxxxxxxxxxxxxxxxxx",
    "agentId": "xxxxxxxxx",
    "webhook": "https://oapi.dingtalk.com/robot/send?access_token=xxx",
    "secret": "SECxxxxxxxxxxxx"
  },
  "models": {
    ...
  }
}
EOFCONFIG

# 设置权限
chmod 600 ~/.openclaw/openclaw.json
```

#### 2. 配置环境变量

```bash
# 添加到 ~/.zshrc 或 ~/.bashrc
cat >> ~/.zshrc << 'EOFENV'

# 钉钉配置
export DINGTALK_APP_KEY="dingxxxxxxxxxxxxxx"
export DINGTALK_APP_SECRET="xxxxxxxxxxxxxxxxxx"
export DINGTALK_AGENT_ID="xxxxxxxxx"
export DINGTALK_WEBHOOK="https://oapi.dingtalk.com/robot/send?access_token=xxx"
export DINGTALK_SECRET="SECxxxxxxxxxxxx"

EOFENV

source ~/.zshrc
```

#### 3. 启动 Gateway 并配置路由

编辑 `~/.openclaw/gateway.json`：

```json
{
  "port": 18789,
  "routes": {
    "/dingtalk/webhook": {
      "handler": "dingtalk.handleWebhook",
      "methods": ["POST"]
    },
    "/dingtalk/callback": {
      "handler": "dingtalk.handleCallback",
      "methods": ["POST"]
    }
  },
  "plugins": {
    "dingtalk": {
      "enabled": true,
      "appKey": "${DINGTALK_APP_KEY}",
      "appSecret": "${DINGTALK_APP_SECRET}",
      "agentId": "${DINGTALK_AGENT_ID}"
    }
  }
}
```

#### 4. 创建消息处理器

创建 `~/.openclaw/plugins/dingtalk-handler.js`：

```javascript
// 钉钉消息处理器

module.exports = {
  async handleWebhook(req, res) {
    const { text, msgtype, senderNick } = req.body;
    
    // 提取用户消息
    const userMessage = text?.content || '';
    
    // 调用 OpenClaw 处理
    const response = await openclaw.process(userMessage);
    
    // 发送回复
    await dingtalk.sendMessage({
      msgtype: 'text',
      text: {
        content: response
      }
    });
    
    res.json({ success: true });
  },
  
  async handleCallback(req, res) {
    // 处理钉钉事件回调
    const { EventType } = req.body;
    
    switch (EventType) {
      case 'check_url':
        // URL 验证
        res.json({ msg_signature: 'xxx' });
        break;
      
      case 'check_create_suite_url':
        // 创建应用验证
        res.json({ success: true });
        break;
      
      default:
        res.json({ success: true });
    }
  }
};
```

#### 5. 启动服务

```bash
# 启动 Gateway
openclaw gateway

# 查看日志
tail -f ~/.openclaw/logs/gateway.log
```

#### 6. 配置钉钉回调

在钉钉开放平台配置：

```
事件订阅 URL: http://your-server-ip:18789/dingtalk/callback
```

---

## 🔐 安全配置

### 1. 签名验证

所有 Webhook 回调必须验证签名：

```javascript
const crypto = require('crypto');

function verifySignature(timestamp, secret, body) {
  const stringToSign = `${timestamp}\n${secret}`;
  const sign = crypto
    .createHmac('sha256', secret)
    .update(stringToSign)
    .digest('base64');
  return sign;
}

// 使用示例
const timestamp = req.headers['timestamp'];
const sign = req.headers['sign'];
const calculatedSign = verifySignature(timestamp, DINGTALK_SECRET, req.body);

if (sign !== calculatedSign) {
  return res.status(401).json({ error: 'Invalid signature' });
}
```

### 2. IP 白名单

在钉钉开放平台配置服务器出口 IP：

```
开发管理 → 安全设置 → 服务器出口IP → 添加 IP
```

### 3. Token 管理

Access Token 有效期为 2 小时，需自动刷新：

```bash
# 获取 Access Token
curl -X GET "https://oapi.dingtalk.com/gettoken?appkey=${DINGTALK_APP_KEY}&appsecret=${DINGTALK_APP_SECRET}"

# 返回
{
  "errcode": 0,
  "errmsg": "ok",
  "access_token": "xxxxxxxxxxxxxxxxxx",
  "expires_in": 7200
}
```

---

## 💡 实际使用案例

### 案例 1: 群聊问答

**在钉钉群中**：

```
@OpenClaw助手 请列出当前项目的文件结构

@OpenClaw助手 帮我生成一份项目周报

@OpenClaw助手 查询GPU使用率
```

### 案例 2: 审批流程自动化

```javascript
// OpenClaw 处理审批回调
openclaw.on('dingtalk.approval.pending', async (approval) => {
  // 分析审批内容
  const analysis = await openclaw.analyze(approval.content);
  
  // 自动审批或提醒
  if (analysis.risk === 'low') {
    await dingtalk.approveProcess(approval.id);
  } else {
    await dingtalk.sendNotification({
      userId: approval.approverId,
      message: `需要您审批: ${approval.title}`
    });
  }
});
```

### 案例 3: 定时任务推送

```bash
# 配置定时任务
openclaw cron add \
  --name "每日报告" \
  --cron "0 17 * * *" \
  --message "生成今日工作报告并发送到钉钉群"

# OpenClaw 会在每天 17:00 自动：
# 1. 生成工作报告
# 2. 通过钉钉 Webhook 发送到群聊
```

### 案例 4: 智能客服

```javascript
// 自动回复常见问题
openclaw.on('dingtalk.message', async (msg) => {
  const { content, sender } = msg;
  
  // 使用 OpenClaw 的记忆系统
  const context = await openclaw.memory.getUserContext(sender);
  
  // 生成回复
  const reply = await openclaw.generateReply(content, context);
  
  // 发送回复
  await dingtalk.sendMessage({
    msgtype: 'text',
    text: { content: reply },
    at: { atUserIds: [sender] }
  });
});
```

---

## 🐛 故障排查

### 常见问题

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| Webhook 无响应 | URL 配置错误 | 检查 Webhook URL 是否正确 |
| 签名验证失败 | Secret 不正确 | 重新生成并配置 Secret |
| 消息发送失败 | Token 过期 | 实现自动刷新机制 |
| 权限不足 | 权限未开通 | 在开放平台申请权限 |
| 回调验证失败 | 签名算法错误 | 检查 HMAC-SHA256 实现 |

### 调试步骤

#### 1. 测试 Webhook

```bash
curl -X POST "https://oapi.dingtalk.com/robot/send?access_token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "msgtype": "text",
    "text": {
      "content": "OpenClaw 测试消息"
    }
  }'
```

#### 2. 查看 OpenClaw 日志

```bash
tail -f ~/.openclaw/logs/gateway.log
tail -f ~/.openclaw/logs/dingtalk.log
```

#### 3. 测试 API 连接

```bash
# 获取 Access Token
curl "https://oapi.dingtalk.com/gettoken?appkey=${DINGTALK_APP_KEY}&appsecret=${DINGTALK_APP_SECRET}"

# 发送测试消息
curl -X POST "https://oapi.dingtalk.com/topapi/message/corpconversation/asyncsend_v2" \
  -H "Content-Type: application/json" \
  -d '{
    "agent_id": "YOUR_AGENT_ID",
    "userid_list": "user_id",
    "msg": {
      "msgtype": "text",
      "text": {"content": "测试"}
    }
  }'
```

#### 4. 检查插件状态

```bash
openclaw plugins list
openclaw plugins status @soimy/dingtalk
```

---

## 📊 监控和维护

### 监控指标

```bash
# Gateway 状态
openclaw health

# 钉钉插件状态
openclaw plugins status @soimy/dingtalk

# 消息统计
openclaw stats dingtalk
```

### 日志管理

```bash
# 查看实时日志
tail -f ~/.openclaw/logs/dingtalk.log

# 归档旧日志
mv ~/.openclaw/logs/dingtalk.log ~/.openclaw/logs/dingtalk-$(date +%Y%m%d).log
```

---

## 🎓 最佳实践

### 1. 消息格式

使用 Markdown 格式增强可读性：

```javascript
await dingtalk.sendMessage({
  msgtype: 'markdown',
  markdown: {
    title: '项目周报',
    text: '# 本周工作\n\n## 完成事项\n- 功能A开发\n- Bug修复'
  }
});
```

### 2. 群聊管理

避免刷屏：

```javascript
// 使用 @ 功能
await dingtalk.sendMessage({
  text: { content: '报告已生成' },
  at: { atUserIds: ['userId1'] }
});

// 或发送卡片消息
await dingtalk.sendActionCard({...});
```

### 3. 错误处理

```javascript
try {
  await dingtalk.sendMessage(msg);
} catch (error) {
  console.error('发送失败:', error);
  // 重试或记录
  await openclaw.memory.logError(error);
}
```

---

## 🚀 进阶功能

### 集成审批流程

```bash
# OpenClaw 监听审批事件
openclaw cron add \
  --name "审批监控" \
  --cron "*/5 * * * *" \
  --message "检查待审批事项并智能处理"
```

### 自定义命令

```bash
# 在钉钉中使用命令
@OpenClaw /help            # 查看帮助
@OpenClaw /query 项目状态   # 查询信息
@OpenClaw /approve 12345   # 审批操作
```

---

## 📚 参考资源

- [钉钉开放平台文档](https://open-dev.dingtalk.com)
- [OpenClaw 官方文档](../README.md)
- [Webhook 机器人指南](https://open.dingtalk.com/document/robots/custom-robot-access)
- [企业内部应用开发](https://open.dingtalk.com/document/orgapp/overview)

---

## ✅ 完成清单

集成完成后，检查：

- [ ] 钉钉机器人创建成功
- [ ] Webhook 配置正确
- [ ] OpenClaw 插件已安装
- [ ] 签名验证通过
- [ ] 能发送测试消息
- [ ] 能接收钉钉消息
- [ ] Gateway 正常运行
- [ ] 定时任务工作正常

---

**恭喜！你已经成功将 OpenClaw 接入钉钉！🎉**

**下一步**：
- [GPU 训练监控](gpu-monitoring.md)
- [知识管理系统](knowledge-management.md)
- [自定义 Skill 开发](../advanced/custom-skills.md)
