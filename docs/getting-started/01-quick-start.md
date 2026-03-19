# 5分钟快速开始

让我们用最快的方式体验 OpenClaw！

---

## 🎯 本节目标

- ✅ 安装 OpenClaw
- ✅ 启动第一个对话
- ✅ 体验基础功能
- ✅ 配置第一个定时任务

预计用时：**5-10 分钟**

---

## 📦 步骤 1: 安装 Node.js

OpenClaw 需要 Node.js 22+：

```bash
# 使用 nvm 安装（推荐）
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc  # 或 source ~/.zshrc
nvm install 22
nvm use 22

# 验证安装
node --version  # 应显示 v22.x.x
npm --version
```

---

## 🚀 步骤 2: 安装 OpenClaw

```bash
# 全局安装
npm install -g openclaw

# 验证安装
openclaw --version

# 输出示例: 2026.3.8
```

---

## 🔑 步骤 3: 配置 API Key

OpenClaw 需要连接 LLM 提供商。这里以 **Anthropic Claude** 为例：

### 方式 1: 使用配置向导（推荐）

```bash
openclaw configure
```

按提示输入：
- API Provider: 选择 `anthropic`
- API Key: 输入你的 Anthropic API Key
- Base URL: 默认（直接回车）
- Default Model: 选择 `claude-sonnet-4-5`

### 方式 2: 手动配置

创建配置文件 `~/.openclaw/openclaw.json`：

```json
{
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "https://api.anthropic.com",
        "apiKey": "sk-ant-your-api-key-here",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "claude-sonnet-4-5",
            "name": "Claude Sonnet 4.5"
          }
        ]
      }
    }
  }
}
```

### 🔒 安全提示

```bash
# 推荐使用环境变量
export ANTHROPIC_API_KEY="sk-ant-your-key"

# 添加到 ~/.zshrc 或 ~/.bashrc
echo 'export ANTHROPIC_API_KEY="sk-ant-your-key"' >> ~/.zshrc
source ~/.zshrc
```

---

## 💬 步骤 4: 启动第一个对话

```bash
# 启动 TUI (Terminal UI)
openclaw tui
```

### 尝试以下对话

```bash
# 1. 打招呼
> 你好，请介绍一下自己

# 2. 文件操作
> 请列出当前目录的文件

# 3. 创建文件
> 请在当前目录创建一个 hello.txt 文件，内容是 "Hello OpenClaw!"

# 4. 读取文件
> 请读取 hello.txt 的内容

# 5. 系统信息
> 请告诉我系统的内存使用情况
```

### 常用命令

```bash
/help         # 查看帮助
/model opus   # 切换到 Claude Opus 4.6
/exit         # 退出对话
```

---

## ⏰ 步骤 5: 配置第一个定时任务

让我们创建一个每天早上 9 点的提醒：

### 方式 1: 在对话中配置

```bash
openclaw tui

> 请每天早上 9 点提醒我开会
```

OpenClaw 会自动创建定时任务！

### 方式 2: 使用命令行

```bash
openclaw cron add \
  --name "每日会议提醒" \
  --cron "0 9 * * *" \
  --tz "Asia/Shanghai" \
  --message "该开会了！"
```

### 查看任务

```bash
# 列出所有定时任务
openclaw cron list

# 输出示例:
# ID  | Name           | Cron         | Status  | Next Run
# ----+----------------+--------------+---------+------------------
# 1   | 每日会议提醒   | 0 9 * * *    | enabled | 2026-03-20 09:00
```

---

## 🎉 成功！

恭喜！你已经完成了 OpenClaw 的快速入门。

### 你学会了：

- ✅ 安装 OpenClaw
- ✅ 配置 API Key
- ✅ 使用 TUI 进行对话
- ✅ 让 AI 操作文件系统
- ✅ 创建定时任务

---

## 🚀 下一步

### 推荐学习路径

1. **详细安装指南** → [02-installation.md](02-installation.md)
   - 多模型配置
   - 高级配置选项
   - 企业环境部署

2. **记忆系统** → [../advanced/memory-system.md](../advanced/memory-system.md)
   - 让 AI 真正"记住"你
   - 分层记忆架构
   - 记忆管理技巧

3. **实战案例** → [../use-cases/personal-assistant.md](../use-cases/personal-assistant.md)
   - 构建个人助理
   - 知识管理系统
   - 自动化工作流

---

## 🐛 遇到问题？

### 常见问题

**Q: 安装失败，提示权限错误**
```bash
# 尝试使用 sudo
sudo npm install -g openclaw

# 或配置 npm 全局目录
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

**Q: 对话无响应**
```bash
# 检查 API Key 是否正确
openclaw models list

# 测试 API 连接
openclaw test
```

**Q: 定时任务不执行**
```bash
# 确保 Gateway 正在运行
openclaw gateway

# 查看任务日志
openclaw cron runs --name "任务名称" --limit 5
```

### 获取帮助

- 📖 [完整文档](../README.md)
- 🐛 [故障排查](../advanced/troubleshooting.md)
- 💬 社区支持（GitHub Issues）

---

**下一节**: [详细安装与配置](02-installation.md)
