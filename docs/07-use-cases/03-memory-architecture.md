# 第7章·第3节：记忆架构与生产实践

> 四层记忆系统和生产环境的最佳实践。

---

## 记忆系统架构

系统使用 4 层记忆架构：

| 层级 | 持久性 | 内容 |
|------|-------|------|
| 用户偏好 | 长期 | 姓名、习惯、沟通风格 |
| 决策历史 | 中期 | 过去的选择和项目决策 |
| 技术知识 | 中短期 | 学到的概念、代码模式 |
| 对话历史 | 短期 | 最近的交互记录 |

## 生产环境经验

### 1. 让对话有意义

```bash
# 给重要对话设标题
openclaw chat --title "React 性能优化" "讨论 React 性能"
```

### 2. 归档旧会话

```bash
# 归档 30 天前的会话
openclaw session archive --older-than 30d

# 手动备份
cp -r ~/.openclaw/agents/main/sessions \
  ~/.openclaw/backups/sessions-$(date +%Y%m%d)
```

### 3. 监控 Cron 任务

```bash
# 查看最近执行记录
openclaw cron runs --name "Knowledge Sync" --limit 50

# 查看失败的任务
openclaw cron runs --name "Knowledge Sync" --failed
```

### 4. 保护 API Key

在 `.gitignore` 里加上：

```gitignore
# OpenClaw 配置和认证文件（包含 API Key）
.openclaw/
**/auth-profiles.json

# 环境变量和密钥
.env
.env.local
*.key
*.pem
```

---

*下一节：[钉钉对接前置准备 →](../08-dingtalk-complete/01-prerequisites.md)*
