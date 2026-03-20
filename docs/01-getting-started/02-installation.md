# 第1章·第2节：安装指南（全平台）

> 5 分钟搞定安装，从零到跑起来。

---

## 方式一：一键安装脚本（推荐）

### Linux / macOS

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### Windows PowerShell

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

> ⚠️ Windows 建议使用 WSL2，更稳定

---

## 方式二：npm 全局安装

```bash
# 确保 Node.js >= 22.16
node --version

# 全局安装
npm install -g openclaw@latest

# 验证
openclaw --version
```

---

## 方式三：Docker 部署

```bash
# 拉取镜像
docker pull openclaw/openclaw:latest

# 运行
docker run -d \
  --name openclaw \
  -p 18789:18789 \
  -v ~/.openclaw:/root/.openclaw \
  openclaw/openclaw:latest
```

---

## 方式四：从源码编译

```bash
# 克隆仓库
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# 安装依赖
npm install

# 构建
npm run build

# 启动
npm start
```

---

## 安装后：初始化引导

安装完成后，运行初始化向导：

```bash
openclaw onboard --install-daemon
```

向导会引导你完成以下步骤：

### Step 1：风险提示
```
OpenClaw 运行时权限较大，是否了解风险？
> Yes
```

### Step 2：选择模式
```
选择安装模式：
> QuickStart （推荐新手）
  Advanced   （自定义配置）
```

### Step 3：选择 AI 模型提供商
```
选择模型服务商：
  Anthropic (Claude)    — 质量最好，需要 API Key
  OpenAI (GPT-4)        — 稳定可靠
> Qwen (通义千问)        — 免费额度充足，国内推荐
  Google (Gemini)        — 免费额度
  自定义 (Custom)        — 自定义 API 端点
```

> 💡 **国内用户推荐选择 Qwen**，免费额度充足，无需翻墙

### Step 4：配置 API Key
- 如果选择 Qwen：会自动打开浏览器登录阿里云
- 如果选择 Anthropic：需要输入 API Key

### Step 5：选择渠道
```
选择要启用的渠道：
  Telegram
  Discord
  WhatsApp
> Skip for now （先跳过，后面再配）
```

### Step 6：选择 Skills
```
是否安装推荐 Skills？
> No （先跳过，后面再学习）
```

### Step 7：启动
```
选择启动方式：
> TUI  （终端界面，推荐学习时使用）
  Web  （浏览器面板）
```

---

## 验证安装

```bash
# 检查服务状态
openclaw status

# 如果看到 Gateway 监听在 18789 端口，说明安装成功
# 输出示例：
# Gateway:   running (port 18789)
# Agent:     running (pid 12345)
# Daemon:    active
```

---

## 国内服务器特殊说明

### 网络问题处理

如果安装时遇到网络问题：

```bash
# 方法1：使用 npm 镜像
npm config set registry https://registry.npmmirror.com

# 方法2：如果 npm install 失败（OOM）
# 创建 swap 空间
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# 永久生效
echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab
```

### 阿里云 ECS 推荐配置

| 项目 | 推荐 |
|------|------|
| 实例规格 | ecs.c6.large (2vCPU 4GB) |
| 系统 | Ubuntu 22.04 LTS |
| 磁盘 | 40GB SSD |
| 安全组 | 开放 18789 端口（Web UI） |

---

## 常见安装问题

### Q: npm install failed; cleaning up and retrying
**原因**：内存不足（2GB 以下容易 OOM）
**解决**：配置 swap 空间（见上方）

### Q: node: command not found
**解决**：安装 Node.js
```bash
# 使用 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
source ~/.bashrc
nvm install 24
```

### Q: 安装后 openclaw 命令找不到
**解决**：
```bash
# 查找安装位置
which openclaw || find / -name "openclaw" -type f 2>/dev/null

# 添加到 PATH
export PATH="$PATH:$(npm config get prefix)/bin"
echo 'export PATH="$PATH:$(npm config get prefix)/bin"' >> ~/.bashrc
```

---

## 下一节

👉 [03-第一次对话](03-first-chat.md) — 和你的 AI 龙虾聊天
