# Social Media Auto Posting System using n8n

A complete automation workflow built with n8n for creating and publishing social media content across multiple platforms.

## Overview

This workflow automates the full social media posting process.
Content moves from idea to publication without manual effort.

You manage content in Google Sheets.
AI generates captions and hashtags.
Posts publish automatically to each platform.

## Features

### AI Powered Content Generation

- Platform specific captions using OpenAI GPT 5
- Optimized hashtags per platform
- Image generation prompts
- Tone adjustment for each network

### Multi Platform Publishing

Automated posting to:

- Facebook page posts with landscape images
- Instagram feed posts with portrait images
- Twitter X character optimized posts
- Pinterest board creation and pin posting
- LinkedIn professional posts with media

### Google Sheets Integration

- Central content management
- Platform wise status tracking
- Post URL and ID logging
- Bulk content planning

### Intelligent Workflow

- Parallel execution per platform
- Automatic retries on failure
- Image format optimization
- Pinterest board creation if missing

## Architecture

### Workflow Structure

```
Google Sheets
Status: New -> Pending -> Posted
        |
        v
AI Agent (OpenAI GPT 5)
Captions
Hashtags
Image prompts
        |
        v
Update Google Sheet
        |
        v
Parallel Platform Posting
Facebook
Instagram
Twitter
Pinterest
LinkedIn
```

## Technical Stack

- n8n
- OpenAI GPT 5
- Google Sheets API
- Facebook Graph API
- Twitter API v2
- Pinterest API
- LinkedIn API

## Usage

1. Add content to Google Sheets
2. Set status to New
3. Run the workflow
4. Posts publish automatically
5. Status updates to Posted

## License

Free for personal and commercial use.
Credit required when shared publicly.

## Contact

For questions or collaboration, connect via GitHub.
