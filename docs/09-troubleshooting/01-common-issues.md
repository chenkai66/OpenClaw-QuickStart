# 第9章·第1节：常见问题与解决方案

> 踩过的坑，帮你少走弯路。

---

## 安装相关

### "command not found: openclaw"

OpenClaw 没装上或者 PATH 没配好。

```bash
# 检查是否安装
which openclaw
npm list -g openclaw

# 重新安装
npm install -g openclaw@latest

# 如果用 nvm，确认 Node 版本
nvm use 22
```

### "Node.js version too old"

OpenClaw 要求 Node.js v22.16 以上。

```bash
node -v
# 如果低于 v22.16，升级：
nvm install 22
nvm use 22
```

---

## 连接相关

### "API key invalid" 或 "401 Unauthorized"

API Key 配置有问题。

```bash
# 检查环境变量
echo $OPENAI_API_KEY
echo $OPENAI_BASE_URL

# DashScope 用户
# Key 应该以 sk- 开头
# URL 应该是 https://dashscope.aliyuncs.com/compatible-mode/v1

# Coding Plan 用户
# Key 应该以 sk-sp- 开头
# URL 应该是 https://coding.dashscope.aliyuncs.com/v1
```

> 常见错误：Coding Plan 的 Key 和普通 DashScope Key 搞混了。两者的 Key 前缀和 Base URL 都不一样。

### "Model not found"

模型 ID 写错了。

```bash
# 查看可用模型
openclaw models list
```

DashScope 常用模型 ID：`qwen3-max`、`qwen3.5-plus`、`qwen3.5-flash`

### "Connection refused" 或 Gateway 启动失败

```bash
# 检查端口是否被占用
lsof -i :18789

# 如果被占用，杀掉旧进程
kill $(lsof -t -i :18789)

# 重启 Gateway
openclaw gateway restart
```

---

## 渠道相关

### 钉钉收不到消息

1. 检查 Gateway 是否在运行：`openclaw gateway status`
2. 检查钉钉配置中的 `clientId`、`clientSecret` 是否正确
3. 确认机器人在钉钉后台已发布
4. 检查日志：`openclaw gateway --verbose`

### Telegram 机器人不回复

1. 确认 Bot Token 正确
2. 检查是否配置了 dmPolicy
3. 如果是新用户，需要先完成 pairing：`openclaw pairing approve telegram <code>`

---

## 记忆相关

### AI 总是忘记之前说过的话

1. 开启 memoryFlush（见[进阶技巧](../06-advanced/03-advanced-tips.md)）
2. 检查 MEMORY.md 是否太大（应该 < 40 行）
3. 确认是在主会话（Main session）中，群聊和子 Agent 默认不读取 MEMORY.md

### memorySearch 搜不到东西

1. 确认已配置 Embedding API
2. 检查记忆文件中是否有 #标签
3. 试试更具体的搜索词

---

## 性能相关

### 回复速度太慢

1. 换个快一点的模型：`/model qwen3.5-flash`
2. 检查网络到 DashScope API 的延迟
3. 如果上下文太长，用 `/compact` 压缩

### Token 消耗太快

1. 使用模型分级策略，简单任务用便宜模型
2. 开启 memoryFlush，避免上下文重复膨胀
3. 定期清理旧的会话文件

---

## 常用诊断命令

```bash
# 检查 OpenClaw 状态
openclaw status
openclaw doctor

# 查看 Gateway 日志
openclaw gateway --verbose

# 检查配置
cat ~/.openclaw/openclaw.json | python3 -m json.tool

# 检查会话
ls -lt ~/.openclaw/agents/main/sessions/ | head

# 检查记忆
wc -l ~/.openclaw/workspace/MEMORY.md
```


---

## Docker / NAS 相关

### 容器内无法访问网络

```bash
# 检查 DNS
docker exec openclaw ping api.openai.com

# 如果 DNS 失败，指定 DNS
docker run --dns 8.8.8.8 ...
```

### 群晖 NAS 上 OpenClaw 启动失败

1. 确认固件版本 ≥ 1.11
2. 检查 Docker 内存限制（至少给 2GB）
3. 查看容器日志：`docker logs openclaw`


---

*下一节：[FAQ →](02-faq.md)*
