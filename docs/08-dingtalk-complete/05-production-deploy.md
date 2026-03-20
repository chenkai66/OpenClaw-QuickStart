# 第8章·第5节：生产环境部署

> 让你的钉钉 AI 机器人 7x24 稳定运行。

---

## 使用 systemd 管理服务

### 创建 systemd 服务文件

```bash
sudo cat > /etc/systemd/system/openclaw.service << 'SYSTEMD'
[Unit]
Description=OpenClaw AI Agent Gateway
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root
ExecStart=/usr/local/bin/openclaw gateway start --foreground
Restart=always
RestartSec=10
Environment="OPENCLAW_HOME=/root/.openclaw"
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
SYSTEMD
```

### 启用并启动

```bash
# 重新加载 systemd
sudo systemctl daemon-reload

# 启用开机自启
sudo systemctl enable openclaw

# 启动服务
sudo systemctl start openclaw

# 查看状态
sudo systemctl status openclaw

# 查看日志
journalctl -u openclaw -f
```

---

## 日志管理

### 查看实时日志

```bash
journalctl -u openclaw -f
```

### 查看钉钉相关日志

```bash
journalctl -u openclaw | grep dingtalk
```

---

## 监控告警

### 简单健康检查脚本

```bash
cat > /root/check_openclaw.sh << 'CHECK'
#!/bin/bash
if ! systemctl is-active --quiet openclaw; then
  echo "OpenClaw is down, restarting..."
  systemctl restart openclaw
  # 可以在这里添加钉钉告警通知
fi
CHECK
chmod +x /root/check_openclaw.sh
```

添加到 crontab：
```bash
# 每 5 分钟检查一次
*/5 * * * * /root/check_openclaw.sh
```

---

## 安全最佳实践

1. **不要以 root 运行**（生产环境）
   ```bash
   useradd -r -s /bin/false openclaw
   chown -R openclaw: /opt/openclaw
   ```

2. **限制文件系统访问**
   ```json
   {
     "tools": {
       "read": { "allowPaths": ["/home/openclaw/workspace"] },
       "write": { "allowPaths": ["/home/openclaw/workspace"] }
     }
   }
   ```

3. **启用 exec 审批**
   ```json
   {
     "approvals": {
       "exec": { "enabled": true }
     }
   }
   ```

4. **配置防火墙**
   ```bash
   # 只开放必要端口
   ufw allow 22/tcp    # SSH
   ufw allow 18789/tcp # OpenClaw Web UI（如果需要远程访问）
   ufw enable
   ```

---

## 备份策略

```bash
# 备份 OpenClaw 配置和数据
tar -czf openclaw-backup-$(date +%Y%m%d).tar.gz \
  ~/.openclaw/openclaw.json \
  ~/.openclaw/agents/ \
  ~/.openclaw/skills/
```

---

## 恭喜！

你已经完成了 OpenClaw + 钉钉的完整对接！现在你有了一个：
- 7x24 运行的 AI 助手
- 可以通过钉钉随时交互
- 支持流式输出、文件传输
- 具备定时任务能力

👉 [返回目录](../../README.md)
