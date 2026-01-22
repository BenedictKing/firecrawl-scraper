# Firecrawl Scraper Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Web scraping skill for Claude Code using [Firecrawl API](https://firecrawl.dev). Provides deep content extraction, format conversion, page interaction, and website crawling capabilities.

## Features

- 🔍 **Single Page Scraping**: Extract content in markdown, html, json, screenshot, or pdf format
- 🕷️ **Website Crawling**: Crawl entire websites with depth control and path filtering
- 🗺️ **URL Mapping**: Quickly discover all URLs on a website
- 📦 **Batch Scraping**: Scrape multiple URLs in parallel
- 🎭 **Page Interaction**: Browser automation with actions (click, input, scroll, execute JS)
- 📄 **PDF Parsing**: Native PDF document extraction
- 📸 **Screenshots**: Capture full-page or viewport screenshots
- 🎯 **Structured Extraction**: Extract data with custom JSON schemas
- 🔧 **Content Control**: Fine-grained control with includeTags/excludeTags
- ⚡ **Caching**: Built-in cache control (default 48 hours)

## Installation

### Via Claude Code Plugin Manager

```bash
claude plugin install https://github.com/BenedictKing/firecrawl-scraper.git
```

### Manual Installation

1. Clone this repository to your Claude skills directory:
```bash
cd ~/.claude/skills
git clone https://github.com/BenedictKing/firecrawl-scraper.git
```

2. Configure your API key:
```bash
cd firecrawl-scraper/.claude/skills/firecrawl-scraper
cp .env.example .env
# Edit .env and add your Firecrawl API key
```

## Configuration

Get your API key from [Firecrawl Dashboard](https://firecrawl.dev).

Two ways to configure:

1. **Environment Variable** (recommended):
```bash
export FIRECRAWL_API_KEY=your_api_key_here
```

2. **`.env` file**:
```bash
# In .claude/skills/firecrawl-scraper/.env
FIRECRAWL_API_KEY=your_api_key_here
```

## Usage

### Trigger the Skill

Use `/firecrawl-scraper` or let Claude automatically invoke it when you need:
- Scrape web page content
- Extract structured data
- Take screenshots
- Parse PDFs
- Crawl entire websites

### Example Queries

**Scrape Page:**
```
Extract the main content from https://example.com as markdown
```

**Take Screenshot:**
```
Take a full-page screenshot of https://example.com
```

**Parse PDF:**
```
Extract text from https://example.com/document.pdf
```

**Crawl Website:**
```
Crawl https://docs.example.com and extract all documentation pages
```

**Extract Structured Data:**
```
Extract product information (name, price, description) from https://example.com/product
```

## API Endpoints

### 1. Scrape
Extract content from a single web page with multiple format options:
- `markdown`, `html`, `rawHtml`, `links`, `images`, `summary`
- `json` with custom schema
- `screenshot` (PNG)

### 2. Crawl
Crawl entire website with:
- Path filtering (includePaths/excludePaths)
- Depth control (maxDiscoveryDepth)
- Page limit
- Subdomain and external link control

### 3. Map
Quickly get a list of all URLs on a website without full content extraction.

### 4. Batch Scrape
Scrape multiple URLs in parallel for efficient bulk extraction.

## Advanced Features

### Page Interaction (Actions)
Automate browser interactions:
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

### Content Filtering
Control what content to extract:
```json
{
  "onlyMainContent": true,
  "includeTags": ["article", "main"],
  "excludeTags": ["nav", "footer", "aside"]
}
```

### Structured Data Extraction
Extract data with JSON schema:
```json
{
  "formats": [
    {
      "type": "json",
      "prompt": "Extract product information",
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

### PDF Parsing
Native PDF support:
```json
{
  "url": "https://example.com/document.pdf",
  "formats": ["markdown"],
  "parsers": ["pdf"]
}
```

### Caching
Control cache behavior:
```json
{
  "maxAge": 86400000  // 24 hours in milliseconds
}
```

## Architecture

This skill uses a two-phase architecture:
1. **Main skill**: Understands user intent, chooses endpoint, assembles payload
2. **Sub-skill (firecrawl-fetcher)**: Executes HTTP calls in isolated context

This design minimizes token usage and keeps conversation history clean.

## Comparison with Other Tools

| Feature | Firecrawl | Tavily | Exa |
|---------|-----------|--------|-----|
| **Core Focus** | Web scraping & extraction | AI search | Semantic search |
| **Page Interaction** | ✅ Actions | ❌ | ❌ |
| **PDF Support** | ✅ Native | ❌ | ❌ |
| **Screenshots** | ✅ | ❌ | ❌ |
| **Structured Extraction** | ✅ JSON schema | ✅ | ✅ |
| **Website Crawling** | ✅ | ✅ | ❌ |
| **Caching** | ✅ | ❌ | ❌ |
| **Batch Processing** | ✅ | ❌ | ❌ |

**Use Firecrawl when:**
- You need deep content extraction from web pages
- Page interaction is required (clicking, scrolling, etc.)
- You want screenshots or PDF parsing
- You need fine-grained content control
- Batch scraping multiple URLs

**Use Tavily when:**
- You need AI-powered search
- You want structured research with citations

**Use Exa when:**
- You need semantic/embeddings-based search
- Finding similar content is important

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License - see [LICENSE](LICENSE) file for details.

## Author

**BenedictKing**
- GitHub: [@BenedictKing](https://github.com/BenedictKing)

## Links

- [Firecrawl Documentation](https://docs.firecrawl.dev)
- [Firecrawl Dashboard](https://firecrawl.dev)
- [Claude Code](https://github.com/anthropics/claude-code)
