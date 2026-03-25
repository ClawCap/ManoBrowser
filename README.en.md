# ManoBrowser 🖐️

**AI's hand in your browser.**

Control your real, logged-in Chrome browser through a Chrome extension + MCP protocol — navigate, click, fill forms, take screenshots, extract data. Everything happens in your own browser.

🌐 [中文](./README.md) | **English**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT) [![GitHub stars](https://img.shields.io/github/stars/ClawCap/ManoBrowser.svg)](https://github.com/ClawCap/ManoBrowser)

---

## 💡 Why ManoBrowser?

Most data extraction tools require you to hand over credentials, run headless browsers on remote servers, or only scrape public data.

**ManoBrowser is different**:

| | Traditional Scrapers | ManoBrowser |
|---|---|---|
| **Auth** | Requires credentials or cookies | ✅ Uses your browser's existing login sessions |
| **Data scope** | Public pages only | ✅ Anything you can see when logged in |
| **Runs on** | Remote servers / headless browsers | ✅ Your own computer, your own Chrome |
| **Privacy** | Data passes through third-party servers | ✅ Data stays local, no third parties |
| **Interface** | Code / config rules | ✅ Natural language via AI agent |

> **TL;DR: Your browser, your sessions, your data, your control.**

---

## ✨ Five Core Capabilities

| Module | What it does | Example |
|--------|-------------|---------|
| 🖱️ **Browser Automation** | Navigate, click, type, screenshot, cookie management | "Open Douban and take a screenshot" |
| 📊 **Data Extraction** | Extract structured data from the current page | "Turn this table into JSON" |
| 🔍 **Platform Explorer** | Discover data structures and collectible content | "What data is available on this profile page?" |
| 🔧 **API Reverse Engineering** | Intercept page requests, analyze APIs | "Analyze the pagination API on this page" |
| 🎬 **Workflow Recording** | Record operations as reusable JSON workflows | "Save what I just did as a workflow" |

---

## 🚀 Quick Start

### Option 1: Let your AI agent set it up (Recommended 🦞)

If you're using [OpenClaw](https://github.com/openclaw/openclaw), just send this to your agent:

```text
Please install ManoBrowser following this SKILL.md:
https://github.com/ClawCap/ManoBrowser/blob/main/SKILL.md
```

Your agent will guide you through the entire setup — no technical knowledge needed.

### Option 2: Manual installation (3 minutes)

**① Install the Chrome Extension**

📦 [Download ManoBrowser Extension](https://deepmining.oss-cn-beijing.aliyuncs.com/web/static/ds-extension/ManoBrowser.zip)

1. Unzip the downloaded file
2. Open `chrome://extensions` in Chrome
3. Enable "Developer mode" (top right)
4. Click "Load unpacked" → select the unzipped folder
5. Confirm the ManoBrowser icon appears in your toolbar ✅

**② Configure MCP Connection**

Click the extension icon, copy your API key, then add this to your AI client config:

<details>
<summary><b>Claude Code / OpenClaw (.mcp.json)</b></summary>

```json
{
  "mcpServers": {
    "browser": {
      "type": "http",
      "url": "https://datasaver.deepminingai.com/api/v2/mcp",
      "headers": {
        "Authorization": "Bearer <your-api-key>"
      }
    }
  }
}
```
</details>

<details>
<summary><b>mcporter (config/mcporter.json)</b></summary>

```json
{
  "mcpServers": {
    "browser": {
      "baseUrl": "https://datasaver.deepminingai.com/api/v2/mcp",
      "headers": {
        "Authorization": "Bearer <your-api-key>"
      }
    }
  }
}
```
</details>

> ⚠️ This is a **cloud HTTP MCP service**, not localhost / local port / WebSocket.

**③ Verify**

Ask your AI agent to call `chrome_navigate` to visit any webpage. If it works, you're all set.

---

## 🤔 Who is this for?

### OpenClaw / AI Agent users
The **best experience**. Your agent can directly control your browser via MCP tools — like giving it an extra pair of hands.

### Developers / MCP users
Any MCP-compatible AI client can connect. Build your own automation workflows on top of ManoBrowser's capabilities.

---

## 📁 Project Structure

```
ManoBrowser/
├── SKILL.md                        ← Main skill file (AI agents read this)
├── browser-automation/             ← 🖱️ Browser operations
├── web-data-extractor/             ← 📊 Data extraction
├── platform-data-explorer/         ← 🔍 Platform exploration
├── api-skill-builder/              ← 🔧 API reverse engineering
└── chrome-workflow-build/          ← 🎬 Workflow recording
    ├── assets/                     ← Template files
    ├── references/                 ← Reference docs
    └── scripts/                    ← Helper scripts
```

---

## 🔐 Privacy & Security

- **No third-party data routing**: The extension runs directly in your browser; extracted data goes to your local AI agent via MCP
- **Your sessions stay private**: The extension never uploads your cookies or passwords
- **Fully controllable**: Disable the extension, close your browser, or revoke your API key anytime
- **Open source & auditable**: All skill code is public — inspect every line

---

## 📄 License

[MIT](LICENSE) — free to use, modify, and distribute.

---

**⭐ Star this repo if ManoBrowser helps you!**
