# 第7章·第2节：知识 Agent 详解

> 三个核心 Agent 的工作原理和配置方法。

---

## 1. 知识同步 Agent

每小时通过 Cron 运行，自动处理新的对话记录：

```bash
npm run agent:knowledge
```

工作流程：
1. 扫描 OpenClaw 和 Claude Code 的会话目录
2. 找到未处理的新对话
3. 调用 LLM 提取关键信息和结论
4. 生成结构化笔记，保存到 `content/notes/`
5. 更新知识索引

## 2. 研究 Agent

每天运行一次，根据你最近的对话主题自动做研究：

```bash
npm run agent:research
```

工作流程：
1. 分析最近 7 天的热门话题
2. 提取高频关键词
3. 在 Google、GitHub、HackerNews 上搜索
4. 生成研究报告，保存到 `content/reports/`

## 3. 社区研究 Agent

按需触发的研究助手，分析社区讨论：

| 研究类型 | 说明 |
|---------|------|
| 趋势分析 | 搜索近 3 个月讨论，分析趋势 |
| 工具对比 | 收集真实用户评价，对比优劣 |
| 最佳实践 | 搜索实战经验和代码示例 |
| 社区观点 | 分析共识和争议 |

用法：
```bash
# 在对话中直接说
"帮我调研一下 Cursor 和 Copilot 的对比"
```

---

*下一节：[记忆架构与生产实践 →](03-memory-architecture.md)*
