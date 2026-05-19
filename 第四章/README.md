# 第四章：VS Code + Claude Code：项目级 AI 开发工作流

> 如果说桌面端是"对话助手"，CLI 是"终端工具"，那 VS Code + Claude Code 就是真正的**项目级 AI 开发工作流**。它能看到你的整个代码库，理解文件结构，直接修改代码，配合 MCP 还能连接数据库、浏览器、文件系统……是开发者最值得掌握的组合。

---

## 4.1 为什么选择 VS Code + Claude Code？

| 对比维度 | 桌面端 App | CLI | VS Code + Claude Code |
|----------|-----------|-----|----------------------|
| 适合场景 | 日常对话、写作 | 脚本自动化 | 项目开发、代码重构 |
| 代码感知 | ❌ 无法读取项目 | ✅ 当前目录 | ✅✅ 完整项目结构 |
| 文件操作 | ❌ | ✅ | ✅✅ 直接编辑保存 |
| 可视化界面 | ✅ 友好 | ❌ 纯终端 | ✅ 编辑器集成 |
| MCP 扩展 | ✅ 支持 | ✅ 支持 | ✅✅ 配置最方便 |
| 适合人群 | 所有人 | 进阶用户 | 开发者 / 重度用户 |

**核心优势：** Claude 可以直接"看到"你整个项目的文件树，理解各文件之间的关系，做出跨文件的修改，这是其他两种方式做不到的。

---

## 4.2 前置准备

在开始之前，请确认以下内容已完成：

- ✅ 已安装 **VS Code**（[下载地址](https://code.visualstudio.com/)）
- ✅ 已安装 **Node.js**（版本 ≥ 18，参考第三章 3.2 节）
- ✅ 已有 **Claude 账号**（Pro/Max 会员或 API Key，参考第一章）

> 🖼️ 【截图】VS Code 主界面截图，标注左侧插件市场图标

---

## 4.3 安装 Claude Code 插件

### 方法一：通过插件市场安装（推荐新手）

**第一步：打开插件市场**

在 VS Code 左侧边栏点击 **Extensions（扩展）** 图标，或使用快捷键：
- macOS：`⌘ + Shift + X`
- Windows：`Ctrl + Shift + X`

> 🖼️ 【截图】VS Code 插件市场界面截图

**第二步：搜索插件**

在搜索框中输入：
```
Claude Code
```

找到由 **Anthropic** 官方发布的插件，点击 **Install（安装）**。

> 🖼️ 【截图】搜索结果页面，标注 Anthropic 官方插件

**第三步：等待安装完成**

安装完成后，VS Code 左侧边栏会出现 Claude Code 的图标。

> 🖼️ 【截图】安装完成后左侧边栏的 Claude Code 图标

---

### 方法二：通过终端命令安装

如果你已经按照第三章安装了 Claude Code CLI，可以直接在终端运行：

```bash
code --install-extension anthropic.claude-code
```

安装成功后重启 VS Code 即可。

---

## 4.4 登录与配置

### 登录账号

安装完成后，点击左侧 Claude Code 图标，会弹出登录界面：

**方式一：使用 Claude 账号登录（推荐）**

1. 点击 **Sign in with Claude.ai**
2. 浏览器会自动打开 claude.ai 授权页面
3. 确认授权后回到 VS Code，登录即完成

> 🖼️ 【截图】VS Code 内 Claude Code 登录界面

**方式二：使用 API Key 登录**

1. 点击 **Use API Key**
2. 前往 [console.anthropic.com](https://console.anthropic.com) 获取 API Key
3. 将 API Key 粘贴到输入框，回车确认

> 🖼️ 【截图】API Key 输入界面

---

## 4.5 基本使用方法

### 打开 Claude Code 面板

登录后，点击左侧 Claude Code 图标即可打开对话面板。此时 Claude 已经能看到你当前打开的整个项目目录。

> 🖼️ 【截图】Claude Code 面板已打开，右侧显示对话界面

### 核心操作方式

**① 直接对话提问**

在底部输入框直接输入你的需求，例如：
```
帮我分析这个项目的整体结构
这个函数有没有 bug？
帮我把这个 Python 文件重构一下
```

**② 引用特定文件**

使用 `@` 符号可以精准引用项目中的文件：
```
@src/main.py 这个文件的逻辑有什么问题？
@README.md 帮我补充安装说明
```

**③ 选中代码后提问**

在编辑器中选中一段代码，右键点击 → **Ask Claude**，可以直接针对选中内容提问。

> 🖼️ 【截图】右键菜单中的 Ask Claude 选项

**④ 使用快捷键唤起**

- macOS：`⌘ + Shift + P` → 输入 `Claude`，可以看到所有 Claude 相关命令
- Windows：`Ctrl + Shift + P` → 输入 `Claude`

---

## 4.6 MCP 配置：让 Claude 连接你的所有工具

MCP（Model Context Protocol）是 Anthropic 推出的开放协议，允许 Claude 通过"插件"连接外部工具和服务。配置好 MCP 之后，Claude 不再只是一个"聊天窗口"，而是可以：

- 直接读写你电脑的文件
- 操控浏览器自动完成任务
- 查询数据库
- 调用各种第三方 API

### MCP 配置文件位置

VS Code 中 MCP 的配置文件为 `claude_desktop_config.json`，路径如下：

| 系统 | 配置文件路径 |
|------|------------|
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

如果文件不存在，手动创建即可。

### 配置文件基本格式

```json
{
  "mcpServers": {
    "服务名称": {
      "command": "启动命令",
      "args": ["参数1", "参数2"],
      "env": {
        "API_KEY": "你的key"
      }
    }
  }
}
```

---

## 4.7 推荐 MCP 合集：7 个必配工具

### 🗂️ MCP 1：Filesystem（文件系统）

**作用：** 让 Claude 直接读写你本地指定目录的文件，是最基础也是最常用的 MCP。

**安装命令：**
```bash
npm install -g @modelcontextprotocol/server-filesystem
```

**配置示例：**
```json
"filesystem": {
  "command": "npx",
  "args": [
    "-y",
    "@modelcontextprotocol/server-filesystem",
    "/Users/你的用户名/Documents"
  ]
}
```

> ⚠️ 注意：路径填写你希望 Claude 能访问的目录，建议不要直接授权根目录，选一个工作文件夹即可。

---

### 🌐 MCP 2：Puppeteer（浏览器控制）

**作用：** 让 Claude 能控制 Chrome 浏览器，自动打开网页、截图、点击、抓取网页内容。

**安装命令：**
```bash
npm install -g @modelcontextprotocol/server-puppeteer
```

**配置示例：**
```json
"puppeteer": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
}
```

**使用场景举例：**
- "帮我打开这个网页，截图给我看"
- "帮我抓取这个页面的所有链接"
- "自动填写这个表单并提交"

---

### 🔍 MCP 3：Brave Search（网页搜索）

**作用：** 让 Claude 能实时搜索互联网，获取最新信息，不再受训练数据截止日期限制。

**获取 API Key：** 前往 [brave.com/search/api](https://brave.com/search/api/) 注册，每月有免费额度。

**配置示例：**
```json
"brave-search": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-brave-search"],
  "env": {
    "BRAVE_API_KEY": "你的BraveAPIKey"
  }
}
```

---

### 🗄️ MCP 4：SQLite（数据库查询）

**作用：** 让 Claude 能直接读取和查询本地 SQLite 数据库，适合做数据分析、查看应用数据。

**安装命令：**
```bash
npm install -g @modelcontextprotocol/server-sqlite
```

**配置示例：**
```json
"sqlite": {
  "command": "npx",
  "args": [
    "-y",
    "@modelcontextprotocol/server-sqlite",
    "/path/to/your/database.db"
  ]
}
```

---

### 📝 MCP 5：GitHub（代码仓库管理）

**作用：** 让 Claude 直接操作你的 GitHub 仓库，读取代码、创建 Issue、提交 PR、查看 Actions 状态。

**获取 Token：** GitHub → Settings → Developer settings → Personal access tokens

**配置示例：**
```json
"github": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-github"],
  "env": {
    "GITHUB_PERSONAL_ACCESS_TOKEN": "你的GitHubToken"
  }
}
```

**使用场景举例：**
- "帮我查看最近的 commit 记录"
- "帮我创建一个新的 Issue，描述这个 bug"
- "帮我把这个分支的修改提交上去"

---

### 🗒️ MCP 6：Notion（笔记与知识库）

**作用：** 让 Claude 读取和写入你的 Notion 工作区，实现 AI 与知识库的双向同步。

**获取 Token：** Notion → Settings → Integrations → 创建新集成

**配置示例：**
```json
"notion": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-notion"],
  "env": {
    "NOTION_API_TOKEN": "你的NotionToken"
  }
}
```

---

### 🖥️ MCP 7：Sequential Thinking（思维链增强）

**作用：** 让 Claude 在处理复杂问题时，能分步骤、有条理地进行深度推理，显著提升复杂任务的完成质量。

**配置示例：**
```json
"sequential-thinking": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
}
```

**适用场景：** 复杂代码架构设计、多步骤任务规划、学术论文逻辑梳理。

---

## 4.8 完整配置文件示例

将以上 MCP 组合成一个完整配置文件（按需保留你需要的部分）：

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/你的用户名/Documents"
      ]
    },
    "puppeteer": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
    },
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "你的BraveAPIKey"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "你的GitHubToken"
      }
    },
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

> 🖼️ 【截图】VS Code 中打开配置文件，展示填写好的 MCP 配置

配置完成后，**重启 VS Code**，Claude Code 面板中会显示已加载的 MCP 工具列表。

> 🖼️ 【截图】Claude Code 面板中显示已激活的 MCP 工具列表

---

## 4.9 典型使用工作流

以"开发一个 Python 小工具"为例，展示完整工作流：

```
1. 用 VS Code 打开项目文件夹
2. Claude 自动读取项目结构
3. 输入需求："帮我写一个批量重命名文件的脚本"
4. Claude 生成代码并直接写入文件
5. 运行测试，如有问题直接说："第15行报错了，帮我修复"
6. Claude 定位问题，修改代码
7. 配合 GitHub MCP，一键提交："帮我把修改提交到 main 分支"
```

> 🖼️ 【截图】完整工作流的实际操作截图

---

## 4.10 常见问题

**Q：插件安装后看不到 Claude Code 图标？**

重启 VS Code，如果还是看不到，检查插件是否已启用（Extensions 列表中确认 Claude Code 状态为 Enabled）。

**Q：MCP 配置后没有生效？**

1. 检查 JSON 格式是否正确（括号、引号、逗号）
2. 确认 Node.js 版本 ≥ 18
3. 完全退出并重启 VS Code

**Q：提示"无法连接到 Claude 服务"？**

检查网络是否需要代理，确认 API Key 或账号登录状态是否正常。

---

## 本章小结

| 步骤 | 操作 |
|------|------|
| 1 | 安装 VS Code |
| 2 | 安装 Claude Code 插件 |
| 3 | 登录账号或配置 API Key |
| 4 | 配置 MCP 工具（按需） |
| 5 | 重启 VS Code 验证生效 |

完成以上步骤后，你就拥有了一个能读懂项目、能上网搜索、能操控浏览器、能管理 GitHub 的超级 AI 开发助手。

---

> 上一章：[第三章 · CLI 版本——终端部署与进阶使用 ←](../第三章/README.md)  
> 下一章：[第五章 · 用 Claude 撰写学术论文 →](../第五章/README.md)
