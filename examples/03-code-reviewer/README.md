# 案例3：AI 代码审查助手

## 目标
使用自定义 Skill 让 OpenClaw 成为你的代码审查助手。

## 安装

```bash
# 复制 Skill 到 OpenClaw 全局 Skills 目录
cp -r . ~/.openclaw/skills/code-reviewer/
```

## 使用

```
帮我审查一下 /path/to/your/code.py
```

或者更详细的：

```
帮我做一个全面的代码审查：
1. 文件：src/api/handler.js
2. 重点关注：安全性和性能
3. 项目框架：Express.js + MongoDB
```

## 进阶用法

结合 GitHub MCP，自动审查 PR：

```
帮我审查 GitHub 上 my-repo 的 PR #42
```
