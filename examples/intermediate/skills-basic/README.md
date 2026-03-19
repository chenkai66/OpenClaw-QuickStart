# Skills 系统基础

学习 OpenClaw 的 Skills 系统，扩展 AI 能力。

## 什么是 Skills？

Skills 是 OpenClaw 的插件系统，类似手机上的 App：
- 每个 Skill 封装特定功能
- 可以从社区安装
- 也可以自己开发

## 安装 Skill

### 方式 1: 从社区安装

```bash
# 搜索可用的 Skills
openclaw skills search

# 安装 Skill
openclaw skills install knowledge-agent

# 查看已安装的 Skills
openclaw skills list
```

### 方式 2: 从 GitHub 安装

```bash
# 从 GitHub 仓库安装
openclaw skills install github:username/skill-name

# 示例：安装 awesome-openclaw-usecases 中的示例
openclaw skills install github:hesamsheikh/social-media-skill
```

## 使用 Skill

### 在对话中使用

```
You: /skill knowledge-agent --action sync

OpenClaw: 好的，启动 knowledge-agent Skill...

[执行 Skill]
Knowledge Agent 开始同步...
- 读取对话历史
- 提取知识点
- 生成结构化笔记
- 更新知识图谱

✅ 同步完成！
- 新增笔记: 5 条
- 更新标签: 12 个
- 知识图谱新增节点: 8 个
```

### 通过命令行使用

```bash
# 直接执行 Skill
openclaw skill run knowledge-agent --action sync

# 带参数执行
openclaw skill run reddit-readonly \
  --subreddit technology \
  --limit 10 \
  --filter hot
```

## 常用 Skills

### 1. Knowledge Agent

**用途**: 知识管理和自动归档

```bash
openclaw skills install knowledge-agent

# 使用
openclaw skill run knowledge-agent --action sync
```

### 2. Research Agent

**用途**: 自动化研究和报告生成

```bash
openclaw skills install research-agent

# 生成研究报告
openclaw skill run research-agent \
  --topic "AI Agent 2026趋势" \
  --output report.md
```

### 3. Reddit Reader

**用途**: Reddit 内容聚合

```bash
openclaw skills install reddit-readonly

# 获取热门内容
openclaw skill run reddit-readonly \
  --subreddit MachineLearning \
  --limit 20
```

## Skill 开发基础

### 最简 Skill 结构

```
my-skill/
├── SKILL.md          # Skill 说明文档
├── config.json       # 配置文件
└── index.js          # 主逻辑
```

### SKILL.md 示例

````markdown
# My First Skill

## Description
这是我的第一个 OpenClaw Skill。

## Usage
```bash
openclaw skill run my-skill --param value
```

## Parameters
- `param`: 参数说明
````

### config.json 示例

```json
{
  "name": "my-skill",
  "version": "1.0.0",
  "description": "My first OpenClaw skill",
  "author": "Your Name",
  "entrypoint": "index.js",
  "dependencies": {}
}
```

### index.js 示例

```javascript
// OpenClaw Skill 入口

module.exports = {
  async run(params) {
    console.log('Skill 开始执行...');
    console.log('参数:', params);
    
    // 你的逻辑
    const result = await doSomething(params);
    
    return {
      success: true,
      data: result
    };
  }
};

async function doSomething(params) {
  // 实现你的功能
  return 'Hello from my skill!';
}
```

## 测试 Skill

```bash
# 在 Skill 目录中
cd my-skill

# 本地测试
openclaw skill test .

# 查看日志
openclaw skill logs my-skill
```

## 下一步

- [定时任务](../cron-tasks/README.md)
- [记忆系统](../memory-usage/README.md)
- [自定义 Skill 开发](../../advanced/custom-skills/README.md)
