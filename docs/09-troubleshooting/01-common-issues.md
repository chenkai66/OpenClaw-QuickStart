# 第9章·第1节：常见问题排查

> 遇到问题不要慌 — 90% 的问题在这里都有答案。

---

## 安装类问题

### 1. npm install failed / OOM
**症状**：安装过程中内存不足
**解决**：
```bash
# 创建 swap 空间
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile swap swap defaults 0 0' | sudo tee -a /etc/fstab

# 重新安装
npm install -g openclaw@latest
```

### 2. node: command not found
**解决**：
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
source ~/.bashrc
nvm install 24
```

### 3. openclaw 命令找不到
**解决**：
```bash
export PATH="$PATH:$(npm config get prefix)/bin"
echo 'export PATH="$PATH:$(npm config get prefix)/bin"' >> ~/.bashrc
```

---

## 连接类问题

### 4. AI 不回复
**检查步骤**：
```bash
# 1. 检查服务状态
openclaw status

# 2. 检查日志
openclaw gateway logs | tail -20

# 3. 测试 API Key
curl -X POST "你的API端点" \
  -H "Authorization: Bearer 你的APIKey" \
  -H "Content-Type: application/json" \
  -d '{"model":"qwen-max","messages":[{"role":"user","content":"hello"}]}'
```

### 5. 钉钉机器人无响应
**检查清单**：
1. `openclaw status` — Gateway 是否运行
2. `openclaw gateway logs | grep dingtalk` — 有无错误
3. 钉钉开放平台是否已发布版本
4. 消息接收模式是否为 Stream
5. clientId 和 clientSecret 是否正确

### 6. 连接频繁断开
**解决**：
```bash
# 使用 systemd 管理，自动重启
sudo systemctl enable openclaw
sudo systemctl restart openclaw
```

---

## 性能类问题

### 7. AI 回复很慢
**可能原因**：
- 模型服务商负载高
- 网络延迟
- 上下文太长

**解决**：
- 换更快的模型（如 qwen-turbo）
- 清理会话历史 `/new`
- 优化网络连接

### 8. 服务器内存持续增长
**解决**：
```bash
# 定期重启
openclaw gateway restart

# 或设置 crontab 每天凌晨重启
0 3 * * * systemctl restart openclaw
```

---

## 常用诊断命令

```bash
# 系统状态
openclaw status

# 实时日志
openclaw gateway logs -f

# 查看配置
openclaw config get

# 检查端口
lsof -i :18789

# 检查进程
ps aux | grep openclaw

# 检查网络
curl -I https://dashscope.aliyuncs.com
```
