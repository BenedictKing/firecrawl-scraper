# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-01-22

### Added
- Initial release of Firecrawl Scraper skill
- Support for 4 core Firecrawl API endpoints:
  - `/v2/scrape` - Single page scraping with multiple format options
  - `/v2/crawl` - Website crawling with depth and path control
  - `/v2/map` - Quick URL discovery
  - `/v2/batch-scrape` - Parallel multi-URL scraping
- Multiple output formats: markdown, html, rawHtml, links, images, summary, json, screenshot
- Page interaction via actions (wait, click, write, press, scroll, executeJavascript)
- Native PDF parsing support
- Content filtering with includeTags/excludeTags
- Cache control with maxAge parameter
- Structured data extraction with JSON schema
- Two-phase architecture (main skill + sub-skill)
- Environment variable and .env file support for API key
- Comprehensive documentation in English and Chinese
