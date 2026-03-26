# ManoBrowser 🖐️

**AI 在浏览器中的手 🤖🖐️**

让你的 AI 代理 / [OpenClaw](https://github.com/openclaw/openclaw) 通过 MCP 协议在本地操控你的真实浏览器，25+ 自动化工具，保留你的登录态，数据完全由你掌控。

🌐 **中文** | [English](./README.en.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT) [![GitHub stars](https://img.shields.io/github/stars/ClawCap/ManoBrowser.svg)](https://github.com/ClawCap/ManoBrowser)

---

## 💡 为什么需要 ManoBrowser？

市面上的浏览器自动化和数据提取工具，要么需要你提供账号密码，要么在服务器端模拟登录，要么只能抓公开数据。

**ManoBrowser 不同**：

| | 传统爬虫 / RPA / 取数工具 | ManoBrowser |
|---|---|---|
| **登录态** | 需要提供账号密码或 Cookie | ✅ 直接用你浏览器里已登录的会话 |
| **数据范围** | 只能抓公开页面 | ✅ 能访问你登录后才能看到的一切 |
| **运行环境** | 服务器端 / 无头浏览器 | ✅ 你自己的电脑、你自己的 Chrome |
| **隐私安全** | 数据经过第三方服务器 | ✅ 数据在本地处理，不经过任何第三方 |
| **使用方式** | 写代码 / 配置规则 / 学习平台 | ✅ 用自然语言告诉 AI 你要什么 |
| **AI 集成** | 需要额外对接 | ✅ 原生 MCP 协议，即插即用 |

> **一句话总结：你的浏览器、你的登录态、你的数据、你的控制权。**

---

## 🚀 快速体验

### 方式一：让龙虾帮你部署（推荐 🦞）

如果你正在使用 [OpenClaw](https://github.com/openclaw/openclaw)，直接把下面这句话发给你的龙虾：

```text
请帮我安装 ManoBrowser Skill，从这个 GitHub 仓库下载整个项目到你的 skills 目录：
https://github.com/ClawCap/ManoBrowser
```

龙虾会自动完成：下载整个 Skill 包（含所有子模块）→ 引导你安装 Chrome 插件 → 配置 MCP 连接。全程不需要你懂任何技术。

> ⚠️ 龙虾需要下载**整个仓库**，不是只看主 SKILL.md。Chrome 插件只是浏览器和 AI 之间的连接层，真正的自动化能力（数据提取、平台探索、API 逆向、工作流录制）都在 Skill 的子模块文件里。

### 方式二：手动安装（5 分钟）

**① 下载 Skill 包**

```bash
git clone https://github.com/ClawCap/ManoBrowser.git
```

将整个 `ManoBrowser/` 目录放到你的 AI 客户端的 skills 目录下（如 OpenClaw 的 `~/.openclaw/skills/manobrowser/`）。

> ⚠️ 必须下载整个仓库。Chrome 插件只是连接层，真正的自动化能力（数据提取、平台探索、API 逆向、工作流录制）都在 Skill 的子模块文件里。

**② 安装插件 & 配置连接**

让你的 AI agent 读取 `SKILL.md`，它会自动引导你完成 Chrome 插件安装和 MCP 连接配置。

> 所有配置步骤都在 SKILL.md 的「前置条件」章节中，包含插件下载链接、配置方法和常见问题排查。

---

## 📋 功能一览

### 25+ MCP 工具，覆盖浏览器操作全链路

| 类别 | 工具 | 说明 |
|------|------|------|
| **导航** | `chrome_navigate` | 后台导航到目标 URL |
| | `chrome_go_back_or_forward` | 前进 / 后退 |
| | `chrome_close_tabs` | 关闭标签页 |
| **交互** | `chrome_click_element` | 基于 UID 精准点击元素 |
| | `chrome_fill_or_select` | 表单填充、下拉选择 |
| | `chrome_keyboard` | 键盘输入、快捷键 |
| | `chrome_scroll` / `chrome_scroll_into_view` | 页面滚动 |
| | `chrome_input_upload_file` | 文件上传 |
| **信息获取** | `chrome_screenshot` | 页面截图 |
| | `chrome_accessibility_snapshot` | 获取页面元素树（含 UID） |
| | `chrome_get_web_content` | 提取页面文本内容 |
| | `chrome_get_document` | 获取页面 DOM |
| | `chrome_console` | 读取控制台日志 |
| **脚本执行** | `chrome_execute_script` | 在页面中执行 JavaScript |
| | `chrome_inject_script` | 注入持久化脚本 |
| **页面保护** | `chrome_page_protection_enable/disable` | 防止用户误操作干扰自动化流程 |
| **网络** | `chrome_network_capture_start` | 捕获网络请求（API 逆向用） |

### 五大能力模块

| 模块 | 说明 |
|------|------|
| 🖱️ **浏览器自动化** | 完整的浏览器远程控制：导航、点击、填表、截图、Cookie/Storage 管理 |
| 📊 **网页数据提取** | 从当前页面提取结构化数据，支持表格、列表、商品信息等任意格式 |
| 🔍 **平台数据探索** | 探索目标网站的数据结构，发现可采集的数据源和 API 接口 |
| 🔧 **API 逆向工程** | 拦截页面请求，分析 API 参数和签名，生成可复用的取数 Skill |
| 🎬 **工作流录制** | 将浏览器操作录制为 JSON 工作流，支持回放、编辑和分享 |

---

## 🎯 应用场景与示例

### 📱 社交平台数据采集
> "帮我把小红书收藏的所有笔记导出来"
> "搜索小红书上'露营装备'相关的热门笔记"
> "抓取这条抖音热门视频下的所有评论"
> "导出我的豆瓣观影记录和评分"
> "把我B站收藏夹里的视频列表整理出来"

### 🛒 电商与比价
> "监控这个商品的价格变化"
> "把购物车里的商品信息整理成表格"
> "对比三个平台上同款商品的价格"

### 📊 信息整理与研究
> "把这个网页上的表格数据导出为 JSON"
> "汇总这个论坛最近一周的热帖"
> "抓取这个招聘网站上符合条件的职位"

### 🔄 重复操作自动化
> "每天帮我签到这三个网站"
> "自动填写这个表单"
> "把这个流程录下来，以后一键执行"

### 🔧 开发者 / API 逆向
> "分析这个页面的翻页接口参数"
> "看看这个网站用了什么 API"
> "把这个 API 封装成一个可复用的 Skill"

---

## 🤔 适合谁用？

### 🦞 OpenClaw / AI Agent 用户
这是**最佳体验**。Agent 可以直接通过 MCP 工具操控你的浏览器，就像多了一双手——浏览器自动化、网页取数、社交平台数据采集、流程录制，全部用自然语言完成。

### 💻 开发者 / MCP 用户
任何支持 MCP 协议的 AI 客户端（Claude Code、Cursor、Windsurf 等）都能接入。你可以基于 ManoBrowser 的 25+ 工具构建自己的自动化工作流和数据管道。

### 📱 普通用户
不会写代码也没关系——通过 OpenClaw 龙虾，用自然语言就能完成浏览器自动化。"帮我导出豆瓣书单" 就是全部操作。

---

## 📁 项目结构

```
ManoBrowser/
├── SKILL.md                        ← 主 Skill 文件（AI agent 读这个）
├── browser-automation/             ← 🖱️ 浏览器自动化模块
├── web-data-extractor/             ← 📊 网页数据提取模块
├── platform-data-explorer/         ← 🔍 平台数据探索模块
├── api-skill-builder/              ← 🔧 API 逆向工程模块
└── chrome-workflow-build/          ← 🎬 工作流录制模块
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
