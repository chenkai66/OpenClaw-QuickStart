# 第6章·第1节：记忆系统

> OpenClaw 不是金鱼——它能记住你说过的话，而且越用越聪明。

---

## 记忆文件结构

```
~/.openclaw/workspace/
+-- MEMORY.md              # 记忆索引（控制在 40 行内）
+-- memory/
    +-- projects.md        # 当前项目状态
    +-- lessons.md         # 踩坑记录
    +-- YYYY-MM-DD.md      # 每日原始日志
    +-- archive/           # 归档目录
```

---

## 记忆层级

| 层级 | 持久性 | 内容 |
|------|-------|------|
| 用户偏好 | 长期 | 姓名、习惯、沟通风格 |
| 决策历史 | 中期 | 过去的选择、项目决策 |
| 技术知识 | 中短期 | 学到的概念、代码模式 |
| 对话历史 | 短期 | 最近的交互记录 |

---

## 防丢失：memoryFlush

对话太长时，OpenClaw 会压缩旧消息来节省上下文窗口。启用 memoryFlush，让 AI 在压缩之前自动把重要信息存到文件里：

```json
{
  "agents": {
    "defaults": {
      "compaction": {
        "reserveTokensFloor": 20000,
        "memoryFlush": {
          "enabled": true,
          "softThresholdTokens": 4000
        }
      }
    }
  }
}
```

详见 [进阶技巧](03-advanced-tips.md) 章节。

---

## memorySearch：语义搜索记忆

OpenClaw 支持对记忆文件做语义搜索。需要一个 Embedding API。

### 免费方案：SiliconFlow bge-m3

```json
{
  "tools": {
    "memorySearch": {
      "enabled": true,
      "embedding": {
        "provider": "openai-compatible",
        "baseUrl": "https://api.siliconflow.cn/v1",
        "apiKey": "你的SiliconFlow-Key",
        "model": "BAAI/bge-m3"
      }
    }
  }
}
```

在 [SiliconFlow](https://siliconflow.cn/) 注册就能免费拿到 API Key。

### 工作原理

1. 记忆文件被切块后转成向量
2. 你问"我们之前讨论部署的事是怎么定的？"，OpenClaw 会做语义搜索
3. 找到的相关记忆会被注入当前上下文

---

## 自动知识同步（进阶）

如果你同时用 Claude Code 和 OpenClaw，可以设置定时任务把两边的记忆合并：

```bash
# 每小时同步一次
openclaw cron add \
  --name "Knowledge Sync" \
  --cron "0 * * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "读取所有记忆文件，去重，更新 MEMORY.md 索引"

# 每晚 23:00 生成当日总结
openclaw cron add \
  --name "Daily Summary" \
  --cron "0 23 * * *" \
  --tz "Asia/Shanghai" \
  --delivery announce \
  --message "根据 memory/$(date +%Y-%m-%d).md 总结今天的工作"
```

---

## 记忆维护

在 HEARTBEAT.md 里加上自动清理：

```markdown
## 记忆维护
- 每周日 03:00：
  1. MEMORY.md 去重
  2. 归档 30 天前的日志
  3. 检查 projects.md 是否与实际一致
```

---

## 日志格式规范

```markdown
### [PROJECT:MyApp] 部署完成
- **结论**：通过 nginx 部署在 80 端口
- **改动文件**：/etc/nginx/sites-available/myapp
- **教训**：直接暴露端口不行，必须走反向代理
- **标签**：#myapp #部署 #nginx
```

---

## 健康检查

```bash
# 查看记忆文件大小
wc -l ~/.openclaw/workspace/MEMORY.md      # 应该 < 40 行
ls -lt ~/.openclaw/workspace/memory/*.md    # 看最近的日志

# 统计会话文件数
ls ~/.openclaw/agents/main/sessions/*.jsonl | wc -l
```

---

## 使用建议

1. **MEMORY.md 保持精简**：不超过 40 行，只放索引
2. **开启 memoryFlush**：防止长对话丢失上下文
3. **记结论，不记过程**："用 nginx 部署在 80 端口"比"调了半天 bug"强
4. **善用 #标签**：让 memorySearch 更容易命中
5. **每周维护**：清理过期条目
6. **定期备份**：`cp -r ~/.openclaw/ ~/.openclaw-backup-$(date +%Y%m%d)`


---

## ContextEngine：可插拔上下文引擎（v2026.3.7 新增）

v2026.3.7 是记忆系统的一次大重构。之前的记忆管理靠 `memory_search`、`memory_get` 这些 Tools，需要 Agent 主动调用才能获取历史信息。问题很明显：

- Agent 忘了调就等于没记忆
- 每次调用工具都额外消耗 Token
- 重复记录、旧信息不清理，记忆库越来越臃肿

现在改成了 **hooks 机制**：记忆的保存、上下文补充、信息更新全在后台自动完成，Agent 不用操心。

### ContextEngine 生命周期

```
bootstrap   → 初始化上下文
ingest      → 注入新信息
assemble    → 组装最终 Prompt 上下文
compact     → 压缩/裁剪上下文（控制 Token）
afterTurn   → 每轮对话后的后处理
```

### 实际效果

- **不用再手动配置 memorySearch**：系统自动在关键节点处理记忆
- **记忆衰减**：长期没用到的信息会自动降权，保持记忆库轻量
- **可插拔**：不喜欢默认策略？可以换成 RAG、知识图谱、无损压缩等插件，Agent 配置完全不用改

### 配置

默认情况下不需要额外配置，升级到 v2026.3.7+ 就自动启用新的 hooks 逻辑。如果想手动指定 ContextEngine 插件：

```json
{
  "contextEngine": {
    "provider": "default",
    "compactThreshold": 80000,
    "memoryDecayEnabled": true
  }
}
```

> 如果你之前配了 `memoryFlush`、`memorySearch` 等参数，升级后仍然兼容，但建议迁移到新的 ContextEngine 配置。


---

*下一节：[定时任务 →](02-cron-jobs.md)*

