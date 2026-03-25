---
name: api-skill-builder
description: 根据用户需求，通过逆向网页API生成可复用的API取数Skill。适用于：图表数据（Canvas/ECharts/G2等前端渲染的图表）、需要登录态的平台数据、任何通过浏览器能看到但DOM中无法直接提取的数据。核心方法：找到API endpoint → 逆向请求参数 → 用fetch_api直接调API → 生成标准化Skill。
version: 1.0.0
tags: [API逆向, 数据提取, Skill生成, 元技能]
---

# API 取数 Skill 生成器

## 概述

这是一个**元技能（Meta-skill）**，将"逆向网页API并生成可复用取数Skill"的方法论封装为标准流程。

**核心思路：页面上能看到的数据，一定来自某个 API 请求。从 request 中取数最靠谱。**

**输入**：用户描述要从某个网页/平台获取什么数据
**输出**：一个完整的、可复用的 API 取数 Skill（SKILL.md + workflow.json）

## 前置条件

1. 浏览器已登录目标平台，登录态有效
2. DataSaver Chrome 扩展已安装并连接（提供 `fetch_api` 等工具）
3. MCP 端点已配置

## 执行流程（4个阶段）

```
阶段1: 侦察（找到 API endpoint）
  ↓
阶段2: 逆向（破解请求参数）
  ↓
阶段3: 验证（确认能拿到完整数据）
  ↓
阶段4: 生成（产出可复用 Skill）
```

---

### 阶段1: 侦察 — 找到 API Endpoint

**目标**：确定目标数据来自哪个 API 请求。

#### Step 1.1: 页面截图 + 结构分析

用 `chrome_screenshot` 和 `chrome_get_web_content` 了解页面布局，确认：
- 数据展示方式（表格/图表/卡片等）
- 是 DOM 渲染还是 Canvas 渲染
- 有哪些筛选条件（类目、日期、维度等）

#### Step 1.2: Performance API 探测

在目标页面执行 JS，获取已加载的 API 请求列表：

```javascript
// 用 chrome_execute_script 执行
JSON.stringify(
  performance.getEntriesByType('resource')
    .filter(e => e.initiatorType === 'xmlhttprequest' || e.initiatorType === 'fetch')
    .map(e => ({ name: e.name, duration: Math.round(e.duration) }))
)
```

从结果中筛选出可能的数据 API（排除埋点、静态资源等）。

#### Step 1.3: JS Bundle 搜索 API 常量

<critical>
很多平台在 JS 中有 API 路径常量映射表（如 `TRACK2_INDUSTRY_DEMAND → chartview/3001`）。
</critical>

方法：
1. 从 Performance API 结果中找到主 JS bundle 的 URL
2. 用 `fetch_api` 下载 JS 文件内容
3. 搜索关键词（页面标题、功能名、"api"、"chartview"、"data" 等）
4. 找到 API 路径常量 → 推断完整 endpoint

```javascript
// 用 fetch_api 下载 JS 文件后搜索
const jsContent = await fetch('https://example.com/static/main.js').then(r => r.text());
// 搜索 API 关键词
const apiPatterns = jsContent.match(/["'](\/api\/[^"']+)["']/g);
```

#### Step 1.4: Webpack Chunk 搜索（备选）

如果 JS bundle 被拆分（webpack code splitting），可遍历已加载的 chunks：

```javascript
// 在 chrome_execute_script 中执行
const chunks = window.webpackChunk_N_E || window.webpackChunkapp || [];
let results = [];
chunks.forEach((chunk, i) => {
  const modules = chunk[1];
  Object.entries(modules).forEach(([id, fn]) => {
    const src = fn.toString();
    if (src.includes('你的搜索关键词')) {
      results.push({ chunkIndex: i, moduleId: id, snippet: src.substring(src.indexOf('关键词') - 50, src.indexOf('关键词') + 100) });
    }
  });
});
JSON.stringify(results);
```

**阶段1产出**：确认目标 API 的完整 URL（如 `POST /api/idea/chartview/3001`）

---

### 阶段2: 逆向 — 破解请求参数

**目标**：搞清楚 API 需要什么参数、参数名是什么、值怎么来。

#### Step 2.1: 获取配置/常量接口

大多数平台有配置接口，返回可用字段、枚举值、类目树等：

```
工具: fetch_api
url: https://目标平台/api/config 或 /api/constants
method: GET
cookieDomain: .目标域名.com
includeCookies: true
```

从配置接口中提取：
- 可用字段列表（指标名、字段名）
- 类目/分类树结构
- 枚举值映射

#### Step 2.2: JS 源码逆向参数构造

在 JS bundle 或 webpack chunks 中搜索 API 路径常量，找到调用该 API 的代码段：

关注点：
- **参数名**（⚠️ 大小写敏感！同一 API 可能混用大小写）
- **参数类型**（string/array/number）
- **必填/可选**
- **默认值**
- **参数值来源**（硬编码/用户选择/其他接口返回）

#### Step 2.3: 试探性请求

用 `fetch_api` 发送试探请求，逐步确认参数：

```
工具: fetch_api
url: https://目标平台/api/数据接口
method: POST
headers: {"Content-Type": "application/json"}
cookieDomain: .目标域名.com
includeCookies: true
body: { 最小参数集 }
```

**逐步验证策略**：
1. 先发最小参数（只传必填项）→ 看返回什么
2. 逐个加可选参数 → 观察返回数据变化
3. 测试参数名大小写变体 → 确认哪个版本有效
4. 确认返回数据是否完整覆盖页面展示的内容

<critical_warning>
⚠️ 参数名大小写陷阱！
实际案例：灵犀 API 中 `xindex`（小写i）和 `sizeIndex`（大写I）在同一个请求中共存。
用 `xIndex`（大写I）请求成功但数据不完整。必须逐一验证！
</critical_warning>

**阶段2产出**：完整的请求参数文档（参数名、类型、必填/可选、来源、示例值）

---

### 阶段3: 验证 — 确认数据完整性

**目标**：确保 API 返回的数据与页面展示一致。

#### Step 3.1: 数据对比验证

1. 截图页面上显示的数据
2. 用 API 获取同样条件的数据
3. 逐一对比确认数值一致

#### Step 3.2: 边界情况测试

- 不同参数组合是否都能正常返回
- 空数据情况的处理
- 分页情况（如果数据量大）
- 多次请求覆盖所有指标（如果单次请求无法获取全部）

#### Step 3.3: 响应结构文档化

记录完整的响应 JSON 结构：
- 各字段含义
- 数据类型（注意：数值可能以字符串形式返回）
- 嵌套结构（如 extraInfo.median）
- 分页信息

**阶段3产出**：经过验证的完整 API 调用方案 + 响应结构文档

---

### 阶段4: 生成 — 产出可复用 Skill

**目标**：生成标准化的 Skill 文件。

#### Step 4.1: 生成 workflow.json

保存到 `workflow-workflows/{skill-name}-workflow.json`：

```json
{
  "name": "{skill-name}",
  "description": "描述",
  "parameters": [
    {
      "name": "参数名",
      "type": "string",
      "required": true,
      "example": "示例值",
      "description": "说明"
    }
  ],
  "platform": "平台名(域名)",
  "steps": [
    {
      "step": 1,
      "tool_name": "fetch_api",
      "tool_params": { ... },
      "description": "步骤说明",
      "output": { ... }
    }
  ],
  "critical_notes": ["注意事项"]
}
```

#### Step 4.2: 生成 SKILL.md

参考 [skill_template.md](references/skill_template.md) 模板生成，保存到 `workflow-skills/{skill-name}/SKILL.md`。

**SKILL.md 必须包含**：
1. YAML 头部（name, description, version, tags, platform, mcp_tools_required）
2. 概述（输入/输出说明）
3. 前置条件（登录态、扩展要求）
4. MCP 配置
5. 参数说明表
6. 执行步骤（每步含工具名、参数、说明）
7. API 响应结构
8. 错误处理表
9. 使用示例

<critical>
⚠️ 生成的 SKILL.md 中必须包含所有在逆向过程中发现的"坑"（参数大小写、复数形式等），
用 critical_warning 标记，防止后续使用时踩坑。
</critical>

#### Step 4.3: 展示摘要并等待确认

```
✅ API 取数 Skill 生成完成！

📊 生成统计：
- Skill名称：{skill-name}
- 目标平台：{platform}
- API端点：{endpoint}
- 需要请求次数：{N}次
- 用户参数：{M}个
- 覆盖指标：{指标列表}

📁 文件位置：
- Skill: workflow-skills/{skill-name}/SKILL.md
- Workflow: workflow-workflows/{skill-name}-workflow.json

请确认是否符合预期？
```

---

## 不靠谱的方法（踩过的坑，不要再试）

| 方法 | 为什么不行 |
|------|-----------|
| `chrome_network_debugger_start` | DS扩展的bug，总是附加到活动标签而非指定标签 |
| `chrome_network_capture_start` | webRequest API 不捕获 response body |
| XHR/fetch Proxy 注入 | 页面 JS 在注入前已缓存了原始引用，且前端可能有缓存不发新请求 |
| `chrome_inject_script` MAIN world | 刷新后不持久化，且变量在 MAIN world 不可见 |
| 从 DOM 提取 Canvas 图表数据 | 图表是 Canvas 渲染，数据不在 DOM 中 |
| `echarts.getInstanceByDom()` | 生产环境不暴露全局图表实例 |

**正确的路径永远是：找 API → 逆向参数 → `fetch_api` 直接调**

## 关键技巧速查

| 技巧 | 说明 |
|------|------|
| Performance API | `performance.getEntriesByType('resource')` 快速找到页面加载的 API 列表 |
| JS Bundle 搜索 | 下载 JS 文件后搜索 API 路径关键词，找常量映射表 |
| Webpack Chunk 遍历 | `window.webpackChunkXXX` 数组包含所有已加载模块，可搜索关键词 |
| 配置接口先行 | 先调 constants/config 类接口，获取字段定义、枚举值、类目树 |
| 参数大小写验证 | 同一 API 可能混用大小写参数名，必须逐一测试 |
| fetch_api 自带 cookie | DS 扩展的 fetch_api 自动注入浏览器 cookie，不需要手动传 |
| 试探性请求 | 从最小参数集开始，逐步加参数观察返回变化 |

## 已生成的 Skill 参考

- `lingxi-market-supply-demand` — 灵犀市场供需数据（参考实现：[查看](../workflow-skills/lingxi-market-supply-demand/SKILL.md)）
