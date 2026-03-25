# ManoBrowser 🖐️

**AI 的手，伸进你的浏览器。**

通过 Chrome 插件 + MCP 协议，让 AI agent 直接操控你已登录的真实浏览器——导航、点击、填表、截图、提取数据，一切都在你自己的浏览器里发生。

🌐 **中文** | [English](./README.en.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT) [![GitHub stars](https://img.shields.io/github/stars/ClawCap/ManoBrowser.svg)](https://github.com/ClawCap/ManoBrowser)

---

## 💡 为什么需要 ManoBrowser？

市面上的取数工具要么需要你提供账号密码，要么在服务器端模拟登录，要么只能抓公开数据。

**ManoBrowser 不同**：

| | 传统爬虫/取数工具 | ManoBrowser |
|---|---|---|
| **登录态** | 需要提供账号密码或 Cookie | ✅ 直接用你浏览器里已登录的会话 |
| **数据范围** | 只能抓公开页面 | ✅ 能访问你登录后才能看到的一切 |
| **运行环境** | 服务器端/无头浏览器 | ✅ 你自己的电脑、你自己的 Chrome |
| **隐私安全** | 数据经过第三方服务器 | ✅ 数据在本地处理，不经过任何第三方 |
| **使用方式** | 写代码/配置规则 | ✅ 用自然语言告诉 AI 你要什么 |

> **一句话总结：你的浏览器、你的登录态、你的数据、你的控制权。**

---

## ✨ 五大能力

| 模块 | 说明 | 示例 |
|------|------|------|
| 🖱️ **浏览器操作** | 导航、点击、输入、截图、Cookie 管理 | "帮我打开豆瓣，截个图" |
| 📊 **数据提取** | 从当前页面提取结构化数据 | "把这个表格的数据整理成 JSON" |
| 🔍 **平台探索** | 探索目标网站的数据结构和可采集内容 | "看看小红书个人主页有哪些数据" |
| 🔧 **API 逆向** | 拦截页面请求，逆向分析 API | "分析这个页面的翻页接口" |
| 🎬 **工作流录制** | 将操作录制为可复用的 JSON 工作流 | "把刚才的操作录成工作流" |

---

## 🚀 快速体验

### 方式一：让龙虾帮你部署（推荐 🦞）

如果你正在使用 [OpenClaw](https://github.com/openclaw/openclaw)，直接把下面这句话发给你的龙虾：

```text
请按照这个 SKILL.md 帮我安装 ManoBrowser：
https://github.com/ClawCap/ManoBrowser/blob/main/SKILL.md
```

龙虾会自动引导你完成插件安装、MCP 配置，全程不需要你懂任何技术。

### 方式二：手动安装（3 分钟）

**① 安装 Chrome 插件**

📦 [下载 ManoBrowser 插件](https://deepmining.oss-cn-beijing.aliyuncs.com/web/static/ds-extension/ManoBrowser.zip)

1. 解压 zip 文件
2. Chrome 打开 `chrome://extensions`
3. 开启右上角「开发者模式」
4. 点击「加载已解压的扩展程序」→ 选择解压后的文件夹
5. 确认工具栏出现 ManoBrowser 图标 ✅

**② 配置 MCP 连接**

点击插件图标，复制 API 密钥，在你的 AI 客户端中添加配置：

<details>
<summary><b>Claude Code / OpenClaw（.mcp.json）</b></summary>

```json
{
  "mcpServers": {
    "browser": {
      "type": "http",
      "url": "https://datasaver.deepminingai.com/api/v2/mcp",
      "headers": {
        "Authorization": "Bearer <你的API密钥>"
      }
    }
  }
}
```
</details>

<details>
<summary><b>mcporter（config/mcporter.json）</b></summary>

```json
{
  "mcpServers": {
    "browser": {
      "baseUrl": "https://datasaver.deepminingai.com/api/v2/mcp",
      "headers": {
        "Authorization": "Bearer <你的API密钥>"
      }
    }
  }
}
```
</details>

> ⚠️ 这是**云端 HTTP MCP 服务**，不是 localhost / 本地端口 / WebSocket。

**③ 验证**

让 AI agent 执行：`chrome_navigate` 访问任意网页，成功即表示配置完成。

---

## 🤔 适合谁用？

### 有 OpenClaw / AI Agent 的用户
这是**最佳体验**。Agent 可以直接通过 MCP 工具操控你的浏览器，就像多了一双手。配合 [Know Your Owner](https://github.com/ClawCap/ManoBrowser) Skill，还能自动从你的社交平台生成个人画像。

### 开发者 / MCP 用户
任何支持 MCP 协议的 AI 客户端都能接入。你可以基于 ManoBrowser 的能力构建自己的自动化工作流。

---

## 📁 项目结构

```
ManoBrowser/
├── SKILL.md                        ← 主 Skill 文件（AI agent 读这个）
├── browser-automation/             ← 🖱️ 浏览器操作
├── web-data-extractor/             ← 📊 数据提取
├── platform-data-explorer/         ← 🔍 平台探索
├── api-skill-builder/              ← 🔧 API 逆向
└── chrome-workflow-build/          ← 🎬 工作流录制
    ├── assets/                     ← 模板文件
    ├── references/                 ← 参考文档
    └── scripts/                    ← 辅助脚本
```

---

## 🔐 隐私与安全

- **数据不经过第三方**：ManoBrowser 插件直接在你的浏览器中运行，提取的数据通过 MCP 协议传给你本地的 AI agent
- **你的登录态不会泄露**：插件不会上传你的 Cookie 或密码
- **完全可控**：你随时可以禁用插件、关闭浏览器、撤销 API 密钥
- **开源可审计**：所有 Skill 代码公开，你可以审查每一行

---

## 📄 License

[MIT](LICENSE) — 自由使用、修改、分发。

---

**⭐ 如果 ManoBrowser 对你有帮助，给个 Star 支持一下！**
