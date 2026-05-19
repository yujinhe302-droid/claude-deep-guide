# 第三章：CLI 版本——终端部署与进阶使用

> 本章面向希望通过命令行使用 Claude Code 的用户。CLI 版本功能最完整、可扩展性最强，但安装步骤相对繁琐，需要提前配置 Node.js 环境。请跟着本章一步一步操作，不要跳步骤。

---

## 3.1 为什么要用 CLI 版本？

CLI（Command Line Interface）版本是 Claude Code 最原始、也最强大的形态。与桌面端 APP 相比，它有以下优势：

| 对比项 | 桌面端 APP | CLI 版本 |
|--------|-----------|---------|
| 安装难度 | ⭐ 简单 | ⭐⭐⭐ 需要配置环境 |
| 功能完整性 | 大部分功能 | 完整功能 |
| 自动化能力 | 较弱 | 强，可写脚本批量执行 |
| MCP 扩展 | 支持 | 完整支持 |
| 适合场景 | 日常对话、轻量任务 | 大型项目、自动化工作流 |
| 系统集成 | 独立运行 | 可与终端工具深度集成 |

**适合使用 CLI 的人：**
- 习惯使用终端的开发者
- 需要批量处理文件或自动化任务的用户
- 希望在 CI/CD 流程中集成 Claude 的工程师
- 想要完整控制 Claude Code 行为的进阶用户

---

## 3.2 前置准备：安装 Node.js 环境

Claude Code CLI 基于 Node.js 运行，因此在安装 Claude Code 之前，**必须先安装 Node.js**。

### 3.2.1 检查是否已安装 Node.js

打开终端（macOS 用 Terminal 或 iTerm2，Windows 用 PowerShell 或 CMD），输入以下命令：

```bash
node -v
```

如果看到类似 `v18.0.0` 或更高版本的输出，说明已安装，可跳过 3.2.2 节。

如果提示 `command not found` 或报错，说明还没有安装，继续往下看。

```bash
npm -v
```

同样检查 npm（Node.js 的包管理器）是否可用，正常情况下安装 Node.js 后 npm 会一并安装。

> 📌 **版本要求：** Claude Code 要求 Node.js **18 或以上版本**，推荐使用最新 LTS（长期支持）版本。

---

### 3.2.2 安装 Node.js（macOS）

**方法一：官网直接下载（推荐新手）**

1. 打开浏览器，访问 Node.js 官网：[https://nodejs.org](https://nodejs.org)
2. 点击 **"LTS"** 版本的下载按钮（LTS = 长期支持，更稳定）

> 🖼️ **【截图】Node.js 官网下载页面**

3. 下载完成后，双击 `.pkg` 安装包，按照提示一路点击「继续」和「安装」
4. 安装完成后，重新打开终端，输入 `node -v` 验证是否安装成功

> 🖼️ **【截图】终端验证 Node.js 版本**

**方法二：使用 Homebrew 安装（适合已有 Homebrew 的用户）**

```bash
brew install node
```

---

### 3.2.3 安装 Node.js（Windows）

1. 访问 [https://nodejs.org](https://nodejs.org)，下载 **LTS** 版本的 Windows 安装包（`.msi` 文件）

> 🖼️ **【截图】Node.js 官网 Windows 下载**

2. 双击安装包，按提示安装，**注意勾选 "Add to PATH"**（这一步很重要，否则后续命令行找不到 node）

> 🖼️ **【截图】安装时勾选 Add to PATH**

3. 安装完成后，打开 PowerShell，输入 `node -v` 验证

---

## 3.3 安装 Claude Code CLI

Node.js 安装完成后，就可以通过 npm 安装 Claude Code 了。

### 3.3.1 执行安装命令

打开终端，输入以下命令：

```bash
npm install -g @anthropic-ai/claude-code
```

- `-g` 表示全局安装，安装后在任意目录都可以使用
- 安装过程可能需要 1~3 分钟，请耐心等待

> 🖼️ **【截图】执行 npm 安装命令**

### 3.3.2 验证安装成功

安装完成后，输入以下命令验证：

```bash
claude --version
```

如果看到版本号输出（如 `1.x.x`），说明安装成功。

> 🖼️ **【截图】验证 Claude Code 安装成功**

> ⚠️ **常见问题：** 如果提示 `permission denied`，在命令前加 `sudo`：
> ```bash
> sudo npm install -g @anthropic-ai/claude-code
> ```
> 然后输入你的电脑密码（输入时不显示字符，属正常现象）。

---

## 3.4 登录与身份验证

Claude Code CLI 支持两种登录方式，与桌面端相同：

### 方式一：使用 Anthropic 账号登录（订阅会员）

在终端输入：

```bash
claude
```

首次运行会提示你登录，按照提示操作：

1. 终端会显示一个授权链接
2. 复制该链接，在浏览器中打开
3. 登录你的 Anthropic 账号并授权
4. 授权完成后，终端会自动检测到登录状态

> 🖼️ **【截图】CLI 登录授权流程**

### 方式二：使用 API Key 登录

如果你使用的是 API Key 而不是订阅账号，执行以下命令：

```bash
export ANTHROPIC_API_KEY="你的API Key"
```

或者创建一个 `.env` 文件，写入：

```
ANTHROPIC_API_KEY=你的API Key
```

> 💡 **API Key 获取方式：** 登录 [https://console.anthropic.com](https://console.anthropic.com) → API Keys → Create Key

---

## 3.5 基本使用方法

### 3.5.1 进入交互模式

在终端输入 `claude`，回车后即进入对话界面，可以直接和 Claude 对话：

```bash
claude
```

> 🖼️ **【截图】进入 Claude CLI 交互界面**

### 3.5.2 在项目目录中使用（最常用）

CLI 版本最大的优势是**可以感知你的项目文件**。在项目目录下启动 Claude，它就能读取、理解、修改你的代码文件。

```bash
# 进入你的项目目录
cd /你的项目路径

# 启动 Claude
claude
```

启动后，Claude 会自动扫描当前目录的文件结构，你可以直接对它说：

- "帮我看看这个项目的整体结构"
- "修改 main.py 中的第 30 行逻辑"
- "帮我写一个 README.md"

### 3.5.3 单次命令模式（非交互）

不想进入对话界面，只想执行一次任务？使用 `-p` 参数：

```bash
claude -p "帮我总结一下当前目录下所有 Python 文件的功能"
```

这在写自动化脚本时非常有用。

### 3.5.4 常用命令速查

| 命令 | 说明 |
|------|------|
| `claude` | 启动交互式对话 |
| `claude -p "问题"` | 单次提问，不进入交互模式 |
| `claude --version` | 查看版本号 |
| `claude --help` | 查看帮助文档 |
| `claude config` | 打开配置菜单 |
| `claude mcp` | 管理 MCP 插件 |

---

## 3.6 进阶：配置 MCP 扩展

MCP（Model Context Protocol）是 Claude Code 的插件系统，可以让 Claude 连接外部工具，比如：浏览器、数据库、文件系统、GitHub 等。

CLI 版本对 MCP 的支持最为完整。

### 3.6.1 查看已安装的 MCP

```bash
claude mcp list
```

### 3.6.2 添加 MCP

```bash
claude mcp add
```

按照提示选择 MCP 类型并配置。MCP 的详细使用方法会在**第七章**专门介绍。

---

## 3.7 更新与卸载

### 更新 Claude Code CLI

```bash
npm update -g @anthropic-ai/claude-code
```

### 卸载 Claude Code CLI

```bash
npm uninstall -g @anthropic-ai/claude-code
```

---

## 3.8 本章小结

| 步骤 | 操作 |
|------|------|
| ① | 安装 Node.js 18+ |
| ② | `npm install -g @anthropic-ai/claude-code` |
| ③ | 登录账号或配置 API Key |
| ④ | `cd 项目目录` → `claude` 启动 |
| ⑤ | 按需配置 MCP 扩展 |

CLI 版本安装步骤虽多，但一旦配置完成，它能做的事情远比桌面端更多。特别是需要处理大型项目、批量任务或自动化工作流时，CLI 是最佳选择。

下一章我们进入 **VS Code 插件篇**，讲解如何在 VS Code 中配置和使用 Claude Code，打造最顺手的 AI 编程环境。

---

> 上一章：[第二章 · 桌面端下载安装与三界面使用详解 ←](../第二章/README.md)  
> 下一章：[第四章 · VS Code 插件：项目级 AI 开发工作流 →](../第四章/README.md)
