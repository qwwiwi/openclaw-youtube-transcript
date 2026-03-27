# YouTube Transcript Skill for OpenClaw

Fetch YouTube video transcripts using [TranscriptAPI.com](https://transcriptapi.com) -- directly from your OpenClaw agent.

## What It Does

- Transcribes any YouTube video (full URLs, shorts, video IDs)
- Returns text with timestamps or structured JSON
- Includes video metadata (title, author, thumbnail)
- Auto-registers for API key (100 free credits, no card required)

## Quick Start

### 1. Install the skill

Copy the `transcript` folder into your OpenClaw skills directory:

```
~/.openclaw/workspace/skills/transcript/
```

Or clone this repo:

```bash
git clone https://github.com/qwwiwi/openclaw-youtube-transcript.git \
  ~/.openclaw/workspace/skills/transcript
```

### 2. Get an API key

Ask your agent:

> "Get me a TranscriptAPI key"

The agent will walk you through email registration (100 free credits, no card).

Or register manually at [transcriptapi.com/signup](https://transcriptapi.com/signup) and run:

```bash
node ./scripts/tapi-auth.js save-key --key sk_YOUR_KEY
```

### 3. Use it

Send your agent a YouTube link:

> "What's this video about? https://youtube.com/watch?v=dQw4w9WgXcQ"

The agent will fetch the transcript, summarize key points, and offer the full text on request.

## How It Works

The skill uses [TranscriptAPI.com](https://transcriptapi.com) to extract captions/subtitles from YouTube videos via their API.

**API endpoint:**
```
GET https://transcriptapi.com/api/v2/youtube/transcript
  ?video_url=VIDEO_URL
  &format=text
  &include_timestamp=true
  &send_metadata=true
```

**Supported URL formats:**
- `https://youtube.com/watch?v=ID`
- `https://youtu.be/ID`
- `https://youtube.com/shorts/ID`
- Bare 11-character video IDs

## File Structure

```
transcript/
├── SKILL.md              # Agent instructions (OpenClaw skill format)
├── scripts/
│   └── tapi-auth.js      # CLI for account setup and key management
└── README.md             # This file
```

## Configuration

The API key is stored in `~/.openclaw/openclaw.json` under:

```json
{
  "skills": {
    "entries": {
      "transcriptapi": {
        "apiKey": "sk_...",
        "enabled": true
      }
    }
  }
}
```

You can also set the environment variable:

```bash
export TRANSCRIPT_API_KEY=sk_YOUR_KEY
```

## Pricing

- **Free tier:** 100 credits, 300 req/min
- 1 credit per successful request
- Errors don't cost credits
- Top up at [transcriptapi.com/billing](https://transcriptapi.com/billing)

## Requirements

- [OpenClaw](https://github.com/openclaw/openclaw) agent
- Node.js (for the auth script)

## Error Codes

| Code | Meaning | Action |
|------|---------|--------|
| 401 | Bad API key | Check key or re-setup |
| 402 | No credits | Top up at billing page |
| 404 | No transcript | Video may not have captions |
| 408 | Timeout | Retry once after 2s |
| 429 | Rate limited | Wait and retry |

## License

MIT

## Links

- [TranscriptAPI.com](https://transcriptapi.com)
- [OpenClaw](https://github.com/openclaw/openclaw)
- [OpenClaw Docs](https://docs.openclaw.ai)
