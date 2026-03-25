# ManoBrowser 🖐️

**AI's hand in your browser.**

通过 MCP 协议连接你的 Chrome 浏览器，让 AI agent 能够远程操控网页——导航、点击、填表、截图、提取数据，全部在你已登录的真实浏览器中完成。

**Your real browser, your login sessions, your data, your control.**

---

## ✨ 能力总览

| 模块 | 说明 |
|------|------|
| **浏览器基本操作** | 导航、点击、输入、截图、Cookie/Storage 管理 |
| **网页数据提取** | 从当前页面提取结构化数据（表格、列表、商品信息等） |
| **平台探索** | 探索目标平台的数据结构，发现可采集的数据源 |
| **API 逆向** | 拦截页面请求，逆向分析 API，生成可复用的取数 Skill |
| **工作流录制** | 将浏览器操作录制为 JSON 工作流，支持回放和分享 |

## 🚀 快速开始

### 1. 安装 Chrome 插件

📦 [下载 ManoBrowser 插件](https://deepmining.oss-cn-beijing.aliyuncs.com/web/static/ds-extension/ManoBrowser.zip)

1. 解压下载的 zip 文件
2. Chrome 打开 `chrome://extensions`
3. 开启右上角「开发者模式」
4. 点击「加载已解压的扩展程序」，选择解压后的文件夹
5. 确认工具栏出现 ManoBrowser 图标

### 2. 配置 MCP 连接

点击插件图标，复制 API 密钥，然后在你的 AI 客户端中配置 MCP 服务：

**Claude Code / OpenClaw（`.mcp.json`）：**
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

> ⚠️ 这是云端 HTTP MCP 服务，不是 localhost / 本地端口 / WebSocket。

### 3. 验证连通

让你的 AI agent 调用 `chrome_navigate` 访问任意网页，成功即表示配置完成。

## 📁 项目结构

```
ManoBrowser/
├── SKILL.md                      ← 主 Skill 文件（AI agent 读这个）
├── browser-automation/           ← 浏览器操作模块
├── web-data-extractor/           ← 网页数据提取模块
├── platform-data-explorer/       ← 平台探索模块
├── api-skill-builder/            ← API 逆向模块
└── chrome-workflow-build/        ← 工作流录制模块
```

## 🤝 适用场景

- **AI agent 浏览器自动化**：让 agent 操控你的浏览器完成任务
- **数据采集**：从已登录的平台提取个人数据（收藏、关注、历史等）
- **流程自动化**：录制重复性操作为工作流，一键回放
- **平台逆向**：分析网站 API，生成可复用的数据采集脚本

## 📄 License

[MIT](LICENSE)
