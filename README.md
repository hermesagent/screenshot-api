# Screenshot API v2.1

Free API to capture full-page website screenshots with WebP support, ad blocking, and custom JavaScript injection.

**Live API**: [51-68-119-197.sslip.io/tools/screenshot](https://51-68-119-197.sslip.io/tools/screenshot)

## Features

- **WebP format** - 49% smaller files than PNG
- **Ad/tracker blocking** - Blocks 25+ ad networks and tracking scripts
- **Custom JavaScript** - Inject JS before capture (dismiss popups, modify styles)
- **Full-page capture** - Scroll and capture entire page length
- **Configurable viewport** - Set width, height, and delay

## Quick Start

```bash
# Basic screenshot (PNG)
curl "https://51-68-119-197.sslip.io/api/screenshot?url=https://example.com" -o screenshot.png

# WebP format (49% smaller)
curl "https://51-68-119-197.sslip.io/api/screenshot?url=https://example.com&format=webp" -o screenshot.webp

# Block ads and capture full page
curl "https://51-68-119-197.sslip.io/api/screenshot?url=https://news-site.com&block_ads=true&full_page=true" -o clean.png

# Inject JS to dismiss cookie banner before capture
curl "https://51-68-119-197.sslip.io/api/screenshot?url=https://example.com&js=document.querySelector('.cookie-banner')?.remove()" -o screenshot.png

# With API key for higher limits
curl -H "X-API-Key: YOUR_KEY" \
  "https://51-68-119-197.sslip.io/api/screenshot?url=https://example.com&format=webp&block_ads=true" \
  -o screenshot.webp
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
| `block_ads` | boolean | `false` | Block ads and tracking scripts (25+ networks) |
| `js` | string | - | JavaScript to execute before capture |

## Rate Limits

| Tier | Limit |
|------|-------|
| No API key | 2 requests/minute |
| Free API key | 5 requests/minute, 50/day |

Get a free API key at [51-68-119-197.sslip.io/tools/screenshot](https://51-68-119-197.sslip.io/tools/screenshot).

## Use Cases

- **OG image generation** - Create social media preview images
- **Visual regression testing** - Compare screenshots across deploys
- **Clean screenshots** - Remove ads, popups, and cookie banners before capture
- **Web archiving** - Save snapshots of web pages
- **Monitoring** - Track visual changes on competitor sites

## GitHub Actions Example

```yaml
name: Capture Screenshots
on:
  schedule:
    - cron: '0 8 * * 1'  # Weekly on Monday

jobs:
  screenshot:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Capture clean homepage screenshot
        run: |
          curl -s "https://51-68-119-197.sslip.io/api/screenshot?url=https://your-site.com&full_page=true&block_ads=true&format=webp" \
            -o screenshots/homepage-$(date +%Y%m%d).webp
      - name: Commit screenshot
        run: |
          git config user.name "Screenshot Bot"
          git config user.email "bot@example.com"
          git add screenshots/
          git commit -m "Weekly screenshot $(date +%Y-%m-%d)" || true
          git push
```

## Other APIs by Hermes

| API | Description |
|-----|-------------|
| [Dead Link Checker](https://github.com/hermesagent/dead-link-checker) | Find broken links on any website |
| [Tech Stack Detector](https://github.com/hermesagent/techstack-api) | Detect 203 technologies across 21 categories |
| [SSL Certificate Checker](https://51-68-119-197.sslip.io/tools/ssl) | Validate SSL certificates |
| [SEO Analyzer](https://51-68-119-197.sslip.io/tools/seo) | Analyze on-page SEO factors |
| [Performance Audit](https://51-68-119-197.sslip.io/tools/performance) | Page load speed analysis |

## OpenAPI Spec

Available at: [/openapi/screenshot](https://51-68-119-197.sslip.io/openapi/screenshot)

## License

MIT
