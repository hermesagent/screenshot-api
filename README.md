# Hermesforge Screenshot API

Visual web perception infrastructure for AI agents and developers. Capture full-page screenshots via a simple REST API — sync or async with webhook callbacks.

**Live API**: [hermesforge.dev/api](https://hermesforge.dev/api) | **Docs**: [hermesforge.dev/api](https://hermesforge.dev/api)

## Features

- **WebP, PNG, JPEG** — WebP is 49% smaller than PNG by default
- **Ad/tracker blocking** — Blocks 25+ ad networks and tracking scripts
- **Custom JavaScript injection** — Dismiss popups, modify styles before capture
- **Full-page capture** — Scroll and capture entire page length
- **Configurable viewport** — Set width, height, and delay
- **Async queue + webhooks** — Fire-and-forget screenshot jobs for agent pipelines
- **Anonymous watermark** — Free tier adds a small `hermesforge.dev` attribution pill

## Quick Start

```bash
# Basic screenshot
curl "https://hermesforge.dev/api/screenshot?url=https://example.com" -o screenshot.png

# WebP format (49% smaller)
curl "https://hermesforge.dev/api/screenshot?url=https://example.com&format=webp" -o screenshot.webp

# Block ads + full page
curl "https://hermesforge.dev/api/screenshot?url=https://news-site.com&block_ads=true&full_page=true" -o clean.png

# Inject JS to dismiss cookie banner
curl "https://hermesforge.dev/api/screenshot?url=https://example.com&js=document.querySelector('.cookie-banner')?.remove()" -o screenshot.png

# With API key (higher limits, no watermark)
curl -H "X-API-Key: YOUR_KEY" \
  "https://hermesforge.dev/api/screenshot?url=https://example.com&format=webp&block_ads=true" \
  -o screenshot.webp
```

## Async Queue (for AI Agent Pipelines)

Submit a job and poll, or receive results via webhook:

```bash
# Submit async job
curl -X POST -H "X-API-Key: YOUR_KEY" -H "Content-Type: application/json" \
  "https://hermesforge.dev/api/screenshot/queue" \
  -d '{"url":"https://example.com","format":"webp","webhook_url":"https://yourapp.com/callback"}'
# → {"job_id": "abc123", "status": "queued"}

# Poll status
curl -H "X-API-Key: YOUR_KEY" "https://hermesforge.dev/api/screenshot/status/abc123"

# Fetch result
curl -H "X-API-Key: YOUR_KEY" "https://hermesforge.dev/api/screenshot/result/abc123" -o result.webp
```

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `url` | string | required | URL to capture |
| `full_page` | boolean | `false` | Capture full scrollable page |
| `width` | integer | `1280` | Viewport width in pixels |
| `height` | integer | `720` | Viewport height in pixels |
| `format` | string | `png` | Output format: `png`, `jpeg`, or `webp` |
| `delay` | integer | `0` | Wait time in ms before capture (max 5000) |
| `block_ads` | boolean | `false` | Block ads and tracking scripts |
| `js` | string | — | JavaScript to execute before capture |

## Rate Limits & Pricing

| Tier | Price | Requests/Day | Requests/Month |
|------|-------|-------------|----------------|
| Anonymous | Free | 20 | — |
| Free (email) | Free | 50 | 500 |
| Starter | $4 one-time / 30-day | 200 | 2,000 |
| Pro | $9 one-time / 30-day | 1,000 | 10,000 |
| Business | $29 one-time / 30-day | 5,000 | 50,000 |

No recurring charges. Keys expire after 30 days. [Get a key →](https://hermesforge.dev/api/keys)

## Use Cases

- **AI agent pipelines** — Give your agent eyes. Screenshot any URL, pass image to vision model.
- **OG image generation** — Create social media preview images programmatically
- **Visual regression testing** — Compare screenshots across deploys
- **Web monitoring** — Track visual changes on competitor sites
- **Clean screenshots** — Remove ads, popups, cookie banners before capture
- **LangChain integration** — [langchain-hermes](https://hermesforge.dev/packages/) provides `HermesScreenshotTool` and `HermesAsyncScreenshotTool`

## LangChain Integration

```python
from langchain_hermes import HermesScreenshotTool

tool = HermesScreenshotTool(api_key="YOUR_KEY")
result = tool.run("https://example.com")
# Returns base64-encoded screenshot for vision model input
```

## GitHub Actions Example

```yaml
name: Visual Regression
on: [push]

jobs:
  screenshot:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Capture screenshot
        run: |
          curl -s -H "X-API-Key: ${{ secrets.HERMES_API_KEY }}" \
            "https://hermesforge.dev/api/screenshot?url=https://your-site.com&format=webp&block_ads=true" \
            -o screenshot.webp
      - uses: actions/upload-artifact@v4
        with:
          name: screenshot
          path: screenshot.webp
```

## OpenAPI Spec

Full spec available at [hermesforge.dev/openapi.json](https://hermesforge.dev/openapi.json)

## Related

- [Hermesforge Chart Rendering API](https://hermesforge.dev/charts) — Render Chart.js charts to PNG/JPEG server-side
- [Hermesforge Dead Link Checker](https://github.com/hermesagent/dead-link-checker) — Check URLs for broken links
