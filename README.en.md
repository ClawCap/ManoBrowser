# ManoBrowser 🖐️

**AI's hand in your browser 🤖🖐️**

Let your AI assistant use the browser just like you do — visit pages, extract data, complete tasks. 25+ automation tools, all running in your own Chrome. Your real browser, your login sessions, your data, your control.

🌐 [中文](./README.md) | **English**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT) [![GitHub stars](https://img.shields.io/github/stars/ClawCap/ManoBrowser.svg)](https://github.com/ClawCap/ManoBrowser)

---

## 💡 Why ManoBrowser?

Most browser automation and data extraction tools require you to hand over credentials, run headless browsers on remote servers, or only scrape public data.

**ManoBrowser is different**:

| | Traditional Scrapers / RPA | ManoBrowser |
|---|---|---|
| **Auth** | Requires credentials or cookies | ✅ Uses your browser's existing login sessions |
| **Data scope** | Public pages only | ✅ Anything you can see when logged in |
| **Runs on** | Remote servers / headless browsers | ✅ Your own computer, your own Chrome |
| **Privacy** | Data passes through third-party servers | ✅ Data stays local, no third parties |
| **Interface** | Code / config rules / learn a platform | ✅ Natural language via AI agent |
| **AI integration** | Requires extra setup | ✅ Works with major AI clients out of the box |

> **TL;DR: Your browser, your sessions, your data, your control.**

---

## 🚀 Quick Start

### Option 1: Let your AI agent set it up (Recommended 🦞)

If you're using [OpenClaw](https://github.com/openclaw/openclaw), just send this to your agent:

```text
Please install the ManoBrowser Skill by downloading the entire project to your skills directory:
https://github.com/ClawCap/ManoBrowser
```

Your agent will automatically: download the full skill pack (including all sub-modules) → guide you through Chrome extension installation → configure connection. No technical knowledge needed.

> ⚠️ The agent needs to download the **entire repository**, not just the main SKILL.md. The Chrome extension is just the connection layer between browser and AI — the real automation capabilities (data extraction, platform exploration, API reverse engineering, workflow recording) are in the skill's sub-module files.

### Option 2: Manual installation (5 minutes)

**① Download the Skill Pack**

```bash
git clone https://github.com/ClawCap/ManoBrowser.git
```

Place the entire `ManoBrowser/` directory in your AI client's skills directory (e.g. OpenClaw's `~/.openclaw/skills/manobrowser/`).

> ⚠️ You must download the entire repository. The Chrome extension is just the connection layer — the real automation capabilities (data extraction, platform exploration, API reverse engineering, workflow recording) are in the skill's sub-module files.

**② Install Extension & Configure Connection**

Have your AI agent read `SKILL.md` — it will guide you through Chrome extension installation and connection setup automatically.

> All setup steps are in the "Prerequisites" section of SKILL.md, including extension download link, configuration methods, and troubleshooting.

---

## 📋 Feature Overview

### 25+ Tools Covering the Full Browser Automation Pipeline

| Category | Tool | Description |
|----------|------|-------------|
| **Navigation** | `chrome_navigate` | Navigate to a URL in background |
| | `chrome_go_back_or_forward` | Go back / forward |
| | `chrome_close_tabs` | Close tabs |
| **Interaction** | `chrome_click_element` | Click elements by UID (most reliable) |
| | `chrome_fill_or_select` | Form filling, dropdown selection |
| | `chrome_keyboard` | Keyboard input, shortcuts |
| | `chrome_scroll` / `chrome_scroll_into_view` | Page scrolling |
| | `chrome_input_upload_file` | File upload |
| **Information** | `chrome_screenshot` | Page screenshots |
| | `chrome_accessibility_snapshot` | Get page element tree (with UIDs) |
| | `chrome_get_web_content` | Extract page text content |
| | `chrome_get_document` | Get page DOM |
| | `chrome_console` | Read console logs |
| **Scripts** | `chrome_execute_script` | Execute JavaScript in page |
| | `chrome_inject_script` | Inject persistent scripts |
| **Protection** | `chrome_page_protection_enable/disable` | Prevent user interference during automation |
| **Network** | `chrome_network_capture_start` | Capture network requests (for API reverse engineering) |

### Five Core Capability Modules

| Module | Description |
|--------|-------------|
| 🖱️ **Browser Automation** | Full remote browser control: navigate, click, fill forms, screenshot, cookie/storage management |
| 📊 **Web Data Extraction** | Extract structured data from any page — tables, lists, product info, any format |
| 🔍 **Platform Data Explorer** | Discover data structures and collectible data sources on target websites |
| 🔧 **API Reverse Engineering** | Intercept page requests, analyze API parameters and signatures, generate reusable data skills |
| 🎬 **Workflow Recording** | Record browser operations as JSON workflows, supports replay, editing, and sharing |

---

## 🎯 Use Cases & Examples

### 📱 Social Media Data Collection
> "Export all my bookmarked posts from Xiaohongshu"
> "Search for trending posts about 'camping gear' on Xiaohongshu"
> "Scrape all comments from this viral Douyin video"
> "Export my Douban movie ratings"
> "List all videos in my Bilibili favorites"

### 🛒 E-commerce & Price Comparison
> "Monitor price changes for this product"
> "Organize my shopping cart items into a spreadsheet"
> "Compare prices across three platforms"

### 📊 Research & Information Gathering
> "Export the table data from this webpage as JSON"
> "Summarize the top posts from this forum this week"
> "Scrape job listings matching my criteria"

### 🔄 Repetitive Task Automation
> "Check in on these three websites daily"
> "Auto-fill this form"
> "Record this workflow so I can replay it later"

### 🔧 Developer / API Reverse Engineering
> "Analyze the pagination API parameters on this page"
> "What APIs does this website use?"
> "Package this API into a reusable skill"

---

## 🤔 Who is this for?

### 🦞 OpenClaw / AI Agent Users
The **best experience**. Your agent uses the browser just like you would — browser automation, web scraping, social media data collection, workflow recording — all with natural language.

### 💻 Developers
Major AI clients (Claude Code, Cursor, Windsurf, etc.) can connect. Build your own automation workflows and data pipelines on top of ManoBrowser's 25+ tools.

### 📱 Non-technical Users
No coding required — with OpenClaw, you can automate your browser using natural language. "Export my Douban book list" is all you need to say.

---

## 📁 Project Structure

```
ManoBrowser/
├── SKILL.md                        ← Main skill file (AI agents read this)
├── browser-automation/             ← 🖱️ Browser automation module
├── web-data-extractor/             ← 📊 Web data extraction module
├── platform-data-explorer/         ← 🔍 Platform data explorer module
├── api-skill-builder/              ← 🔧 API reverse engineering module
└── chrome-workflow-build/          ← 🎬 Workflow recording module
    ├── assets/                     ← Template files
    ├── references/                 ← Reference docs
    └── scripts/                    ← Helper scripts
```

---

## 🔐 Privacy & Security

- **No third-party data routing**: The extension runs directly in your browser; extracted data goes straight to your local AI assistant
- **Your sessions stay private**: The extension never uploads your cookies or passwords
- **Fully controllable**: Disable the extension, close your browser, or revoke your API key anytime
- **Open source & auditable**: All skill code is public — inspect every line

---

## 📄 License

[MIT](LICENSE) — free to use, modify, and distribute.

---

**⭐ Star this repo if ManoBrowser helps you!**
