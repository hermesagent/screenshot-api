# Screenshot API

Free API to capture full-page website screenshots as PNG images.

**Live API**: [51-68-119-197.sslip.io/tools/screenshot](https://51-68-119-197.sslip.io/tools/screenshot)

## Quick Start

```bash
# Capture a screenshot (returns PNG)
curl "https://51-68-119-197.sslip.io/api/screenshot?url=https://example.com" -o screenshot.png

# With API key for higher limits
curl -H "X-API-Key: YOUR_KEY" \
  "https://51-68-119-197.sslip.io/api/screenshot?url=https://example.com&full_page=true" \
  -o screenshot.png
```

## Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `url` | string | required | URL to capture |
| `full_page` | boolean | `false` | Capture full scrollable page |
| `width` | integer | `1280` | Viewport width in pixels |
| `height` | integer | `720` | Viewport height in pixels |
| `format` | string | `png` | Output format: `png` or `jpeg` |
| `delay` | integer | `0` | Wait time in ms before capture (max 5000) |

## Rate Limits

| Tier | Limit |
|------|-------|
| No API key | 2 requests/minute |
| Free API key | 5 requests/minute, 50/day |

Get a free API key at [51-68-119-197.sslip.io/tools/screenshot](https://51-68-119-197.sslip.io/tools/screenshot).

## Use Cases

- **OG image generation** - Create social media preview images
- **Visual regression testing** - Compare screenshots across deploys
- **Web archiving** - Save snapshots of web pages
- **PDF generation** - Convert web pages to images for reports
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
      - name: Capture homepage screenshot
        run: |
          curl -s "https://51-68-119-197.sslip.io/api/screenshot?url=https://your-site.com&full_page=true" \
            -o screenshots/homepage-$(date +%Y%m%d).png
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
| [Tech Stack Detector](https://github.com/hermesagent/techstack-api) | Detect technologies used by websites |
| [SSL Certificate Checker](https://51-68-119-197.sslip.io/tools/ssl) | Validate SSL certificates |
| [SEO Analyzer](https://51-68-119-197.sslip.io/tools/seo) | Analyze on-page SEO factors |
| [Performance Audit](https://51-68-119-197.sslip.io/tools/performance) | Page load speed analysis |

## OpenAPI Spec

Available at: [/openapi/screenshot](https://51-68-119-197.sslip.io/openapi/screenshot)

## License

MIT
