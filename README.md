# Asset Production Pipeline: One Hero Image → Every Channel Format

Automatically resize, crop, and export a single approved hero image into platform-specific formats for retail, print, digital, and social — with brand-consistent overlays and correct naming conventions. Built with **n8n** and **Sharp**.

![n8n workflow](https://img.shields.io/badge/n8n-workflow-orange) ![Image Processing](https://img.shields.io/badge/Image-Processing-purple) ![license MIT](https://img.shields.io/badge/license-MIT-green)

## What It Does

This n8n workflow takes one approved hero image and produces a complete set of platform-ready assets:

- **Multi-Channel Export** — Generates crops and resizes for Instagram (1080×1080, 1080×1350), Facebook (1200×628), Web Banner (1920×600), Retail POS (A3, A4), and Print (300dpi CMYK-ready)
- **Smart Cropping** — Uses focal-point detection to crop intelligently, keeping the product centered across all aspect ratios
- **Brand Overlay System** — Applies logo, text zones, legal lines, and safe-area guides per channel template
- **Naming Convention** — Auto-names exports following `{campaign}_{product}_{channel}_{size}_{version}` format
- **Batch Processing** — Feed it multiple hero images and it processes the full set in parallel
- **Quality Validation** — Checks output resolution, file size, and format against channel requirements before delivery
- **Asset Log** — Logs every export to Google Sheets with metadata, timestamps, and delivery status

## Channels Supported

| Channel | Formats | Specs |
|---------|---------|-------|
| **Instagram Feed** | 1:1, 4:5 | 1080×1080, 1080×1350, sRGB, max 30MB |
| **Instagram Stories** | 9:16 | 1080×1920, sRGB |
| **Facebook Ads** | 1.91:1 | 1200×628, sRGB |
| **Web Banner** | Custom | 1920×600, 728×90, sRGB |
| **Retail POS** | A3, A4 | 300dpi, CMYK-ready, 3mm bleed |
| **Print Ads** | Custom | 300dpi, CMYK-ready, trim marks |
| **Email Header** | 2:1 | 600×300, sRGB, max 200KB |

## How It Works

```
Webhook → Validate → Download Hero Image → Detect Focal Point
→ Generate Channel Variants → Apply Brand Overlays → Quality Check
→ Export & Name → Log to Sheet → Notify Team + Webhook Response
```

1. **Webhook Trigger** — Receives hero image URL, campaign name, product name, target channels, and brand template ID
2. **Download & Analyze** — Downloads the source image and detects the focal point for smart cropping
3. **Channel Variants** — Generates resized and cropped versions for each target channel using Sharp
4. **Brand Overlays** — Applies channel-specific brand templates (logo placement, text zones, legal copy)
5. **Quality Check** — Validates each export against channel specs (resolution, file size, color space)
6. **Export** — Saves to cloud storage with standardized naming convention
7. **Asset Log** — Logs all exports to Google Sheets with full metadata
8. **Notification** — Posts summary to Slack with download links

## Quick Start

### Prerequisites
- n8n installed (self-hosted or cloud)
- Sharp (Node.js image processing library) available on your n8n server
- Google Sheets credentials
- Cloud storage (S3, Google Cloud Storage, or similar)
- Slack webhook (optional)

### Setup
1. **Import the workflow** — Open n8n, go to Workflows → Import from File, select `workflow.json`
2. **Configure brand templates** — Set up your brand overlay templates (logo, fonts, colors)
3. **Set up Google Sheet** — Create a sheet named "Asset Log" with columns: Date, Campaign, Product, Channel, Format, Size, File Name, URL, Status
4. **Activate the workflow** — Toggle to Active

### Usage

```bash
curl -X POST https://your-n8n-instance.com/webhook/asset-pipeline \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://storage.example.com/hero/summer-fragrance-hero.jpg",
    "campaign_name": "Summer Fragrance 2025",
    "product_name": "Eau de Soleil",
    "channels": ["instagram_feed", "instagram_stories", "facebook_ads", "retail_pos", "web_banner"],
    "brand_template": "luxury_beauty_v2",
    "include_bleed": true,
    "notify_channel": "#design-assets"
  }'
```

## Customization

- **Add Channels** — Extend `channelSpecs` with Pinterest, TikTok, LinkedIn, or custom retail formats
- **Custom Templates** — Create brand overlay templates for different product lines or sub-brands
- **Video Thumbnails** — Add a branch that generates video thumbnail variants from the same hero
- **Auto-Upload** — Connect to Meta Ads or Google Ads to upload assets directly to ad platforms
- **Approval Workflow** — Add a review step where assets are sent for approval before final export

## License

MIT — free to use, modify, and share.

## Author

**Ivan Siyanko** — AI Automator
Website: [siyanko.com](https://siyanko.com) | GitHub: [@ivansiyanko](https://github.com/ivansiyanko)

---
Built with [n8n](https://n8n.io) — the workflow automation platform
