# Hello World - OpenClaw 第一个示例

## 目标

创建你的第一个 OpenClaw 对话，了解基本交互模式。

## 前置条件

- ✅ 已安装 OpenClaw
- ✅ 已配置 API Key

## 步骤

### 1. 启动 OpenClaw TUI

```bash
openclaw tui
```

### 2. 基础对话

```
You: 你好，请介绍一下自己

OpenClaw: 你好！我是 OpenClaw，一个 AI Agent 运行时平台。我可以：
- 直接操作你的操作系统（执行命令、读写文件）
- 记住我们的对话和你的偏好
- 定时执行任务并主动提醒你
- 通过 Skills 扩展我的能力

我现在在 macOS/Linux/Windows 上运行，使用 [Model Name] 作为语言模型。

你想让我帮你做什么？
```

### 3. 文件操作示例

```
You: 请在当前目录创建一个 test.txt 文件，内容是 "Hello OpenClaw!"

OpenClaw: 好的，我来创建这个文件。

[执行命令]
echo "Hello OpenClaw!" > test.txt

✅ 文件创建成功！我已经在当前目录创建了 test.txt 文件。

你想查看文件内容吗？
```

### 4. 读取文件

```
You: 请读取 test.txt 的内容

OpenClaw: 好的，让我读取文件。

[执行命令]
cat test.txt

文件内容：
```
Hello OpenClaw!
```

就是你刚才让我写入的内容。
```

### 5. 系统操作

```
You: 请告诉我当前目录有哪些文件

OpenClaw: 让我列出当前目录的文件。

[执行命令]
ls -la

当前目录包含以下文件：
- test.txt (13 bytes) - 刚才创建的
- [其他文件...]

需要我对这些文件做什么操作吗？
```

## 常用命令

在 OpenClaw TUI 中：

```bash
/help         # 查看帮助
/model opus   # 切换到 Claude Opus 模型
/clear        # 清除对话历史
/exit         # 退出
```

## 下一步

- [文件操作进阶](../file-operations/README.md)
- [API 调用](../api-calls/README.md)
- [Skills 使用](../../intermediate/skills-basic/README.md)

