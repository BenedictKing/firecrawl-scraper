# Firecrawl Scraper 技能

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

基于 [Firecrawl API](https://firecrawl.dev) 的 Claude Code 网页抓取技能。提供深度内容提取、格式转换、页面交互和网站爬取能力。

## 功能特性

- 🔍 **单页抓取**：提取 markdown、html、json、screenshot 或 pdf 格式内容
- 🕷️ **网站爬取**：爬取整个网站，支持深度控制和路径过滤
- 🗺️ **URL 映射**：快速发现网站上的所有 URL
- 📦 **批量抓取**：并行抓取多个 URL
- 🎭 **页面交互**：浏览器自动化操作（点击、输入、滚动、执行 JS）
- 📄 **PDF 解析**：原生 PDF 文档提取
- 📸 **网页截图**：捕获全页或视口截图
- 🎯 **结构化提取**：使用自定义 JSON schema 提取数据
- 🔧 **内容控制**：通过 includeTags/excludeTags 精细控制
- ⚡ **缓存机制**：内置缓存控制（默认 48 小时）

## 安装

### 通过 Claude Code 插件管理器

```bash
claude plugin install https://github.com/BenedictKing/firecrawl-scraper.git
```

### 手动安装

1. 克隆此仓库到 Claude 技能目录：
```bash
cd ~/.claude/skills
git clone https://github.com/BenedictKing/firecrawl-scraper.git
```

2. 配置 API 密钥：
```bash
cd firecrawl-scraper/.claude/skills/firecrawl-scraper
cp .env.example .env
# 编辑 .env 并添加你的 Firecrawl API 密钥
```

## 配置

从 [Firecrawl Dashboard](https://firecrawl.dev) 获取 API 密钥。

两种配置方式：

1. **环境变量**（推荐）：
```bash
export FIRECRAWL_API_KEY=your_api_key_here
```

2. **`.env` 文件**：
```bash
# 在 .claude/skills/firecrawl-scraper/.env 中
FIRECRAWL_API_KEY=your_api_key_here
```

## 使用方法

### 触发技能

使用 `/firecrawl-scraper` 或让 Claude 在需要时自动调用：
- 抓取网页内容
- 提取结构化数据
- 截取网页截图
- 解析 PDF
- 爬取整个网站

### 示例查询

**抓取页面：**
```
提取 https://example.com 的主要内容为 markdown 格式
```

**截图：**
```
对 https://example.com 进行全页截图
```

**解析 PDF：**
```
从 https://example.com/document.pdf 提取文本
```

**爬取网站：**
```
爬取 https://docs.example.com 并提取所有文档页面
```

**提取结构化数据：**
```
从 https://example.com/product 提取产品信息（名称、价格、描述）
```

## API 端点

### 1. Scrape（抓取）
从单个网页提取内容，支持多种格式：
- `markdown`、`html`、`rawHtml`、`links`、`images`、`summary`
- 自定义 schema 的 `json`
- `screenshot`（PNG）

### 2. Crawl（爬取）
爬取整个网站，支持：
- 路径过滤（includePaths/excludePaths）
- 深度控制（maxDiscoveryDepth）
- 页面限制
- 子域名和外部链接控制

### 3. Map（映射）
快速获取网站上所有 URL 列表，无需完整内容提取。

### 4. Batch Scrape（批量抓取）
并行抓取多个 URL，实现高效批量提取。

## 高级功能

### 页面交互（Actions）
自动化浏览器交互：
```json
{
  "actions": [
    {"type": "wait", "milliseconds": 2000},
    {"type": "click", "selector": "#load-more"},
    {"type": "scroll", "direction": "down", "amount": 500},
    {"type": "executeJavascript", "script": "window.scrollTo(0, document.body.scrollHeight)"}
  ]
}
```

### 内容过滤
控制要提取的内容：
```json
{
  "onlyMainContent": true,
  "includeTags": ["article", "main"],
  "excludeTags": ["nav", "footer", "aside"]
}
```

### 结构化数据提取
使用 JSON schema 提取数据：
```json
{
  "formats": [
    {
      "type": "json",
      "prompt": "提取产品信息",
      "schema": {
        "type": "object",
        "properties": {
          "name": {"type": "string"},
          "price": {"type": "number"},
          "description": {"type": "string"}
        },
        "required": ["name", "price"]
      }
    }
  ]
}
```

### PDF 解析
原生 PDF 支持：
```json
{
  "url": "https://example.com/document.pdf",
  "formats": ["markdown"],
  "parsers": ["pdf"]
}
```

### 缓存控制
控制缓存行为：
```json
{
  "maxAge": 86400000  // 24 小时（毫秒）
}
```

## 架构

此技能使用两阶段架构：
1. **主技能**：理解用户意图，选择端点，组装负载
2. **子技能（firecrawl-fetcher）**：在隔离上下文中执行 HTTP 调用

这种设计最小化了令牌使用，保持对话历史清晰。

## 与其他工具的比较

| 功能 | Firecrawl | Tavily | Exa |
|------|-----------|--------|-----|
| **核心定位** | 网页抓取与提取 | AI 搜索 | 语义搜索 |
| **页面交互** | ✅ Actions | ❌ | ❌ |
| **PDF 支持** | ✅ 原生 | ❌ | ❌ |
| **网页截图** | ✅ | ❌ | ❌ |
| **结构化提取** | ✅ JSON schema | ✅ | ✅ |
| **网站爬取** | ✅ | ✅ | ❌ |
| **缓存机制** | ✅ | ❌ | ❌ |
| **批量处理** | ✅ | ❌ | ❌ |

**使用 Firecrawl 当：**
- 需要从网页深度提取内容
- 需要页面交互（点击、滚动等）
- 需要截图或 PDF 解析
- 需要精细的内容控制
- 批量抓取多个 URL

**使用 Tavily 当：**
- 需要 AI 驱动的搜索
- 需要带引用的结构化研究

**使用 Exa 当：**
- 需要语义/基于嵌入的搜索
- 查找相似内容很重要

## 贡献

欢迎贡献！请随时提交 Pull Request。

## 许可证

MIT 许可证 - 详见 [LICENSE](LICENSE) 文件。

## 作者

**BenedictKing**
- GitHub: [@BenedictKing](https://github.com/BenedictKing)

## 链接

- [Firecrawl 文档](https://docs.firecrawl.dev)
- [Firecrawl Dashboard](https://firecrawl.dev)
- [Claude Code](https://github.com/anthropics/claude-code)
