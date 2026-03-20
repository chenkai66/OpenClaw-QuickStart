# 案例4：AI 知识库管理

## 目标
让 OpenClaw 自动将对话中的知识整理成结构化笔记。

## 安装

```bash
# 创建知识库目录
mkdir -p ~/knowledge/{tech,project,personal}

# 安装 Skill
cp -r . ~/.openclaw/skills/knowledge-sync/
```

## 使用

```
帮我记住：PostgreSQL 的默认端口是 5432，最大连接数默认是 100。
```

```
把我们刚才讨论的 Docker 部署方案整理成笔记。
```

```
帮我整理一下 ~/knowledge/ 里所有关于 Python 的笔记。
```

## 进阶：结合 Cron 自动整理

设置每天自动整理当天的对话知识：

```
每天晚上 22:00，请：
1. 回顾今天的所有对话
2. 提取有价值的知识点
3. 整理并保存到 ~/knowledge/
4. 更新知识索引
```
