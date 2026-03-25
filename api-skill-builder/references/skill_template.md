---
name: {skill-name}
description: {一句话描述：从什么平台获取什么数据}
version: 1.0.0
tags: [{平台名}, 数据提取, API]
platform: {平台名}({域名})
mcp_tools_required: [fetch_api]
---

# {Skill 中文名}

## 概述

从{平台名}（{域名}）的「{模块路径}」获取{数据描述}。

**输入**：{用户需要提供的参数}
**输出**：{返回数据的格式和内容}

## 前置条件

1. 浏览器已登录{平台名}，登录态有效
2. DataSaver Chrome 扩展已安装并连接（提供 `fetch_api` 工具）
3. MCP 端点已配置（见下方 MCP 配置）

## MCP 配置

```
MCP_URL: {MCP端点URL}
API_KEY: {API密钥}
```

## 参数说明

| 参数 | 类型 | 必填 | 默认值 | 说明 |
|------|------|------|--------|------|
| {param1} | string | ✅ | - | {说明} |
| {param2} | string | ✅ | - | {说明} |
| {param3} | string | ❌ | {默认值} | {说明} |

## 执行步骤

### Step 1: {步骤描述，如"获取配置数据"}

```
工具: fetch_api
参数:
  url: {API URL}
  method: GET/POST
  cookieDomain: .{域名}
  includeCookies: true
```

{步骤说明，从返回数据中提取什么}

### Step 2: {步骤描述}

```
工具: fetch_api
参数:
  url: {API URL}
  method: POST
  headers: {"Content-Type": "application/json"}
  cookieDomain: .{域名}
  includeCookies: true
  body: {请求体JSON}
```

<critical_warning>
⚠️ {在逆向过程中发现的参数陷阱，如大小写问题}
</critical_warning>

### Step N: 合并数据并输出 JSON

{合并逻辑说明}

## API 响应结构

```json
{
  "code": 0,
  "data": {
    // 响应结构示例
  }
}
```

## 错误处理

| 错误信息 | 原因 | 解决方案 |
|---------|------|----------|
| {错误1} | {原因} | {解决方案} |
| {错误2} | {原因} | {解决方案} |

## 使用示例

### 示例1: {场景描述}
```
{自然语言请求示例}
→ {参数映射}
```
