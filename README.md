# Cobalt Self-Hosted

Universal media downloader for YouTube, TikTok, Twitter, Instagram, Reddit, and 20+ platforms.

## Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│ Content Repurp. │────▶│   Cobalt API    │────▶│  WARP Proxy     │
│ / Stravix       │     │ :9000           │     │  (YouTube)      │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

## Supported Platforms

- ✅ YouTube (via WARP proxy)
- ✅ TikTok (video, photos, audio)
- ✅ Twitter/X (video, images)
- ✅ Instagram (reels, posts)
- ✅ Reddit (video, gifs)
- ✅ Pinterest
- ✅ Twitch clips
- ✅ SoundCloud
- ✅ Vimeo
- ✅ Dailymotion
- ✅ Tumblr
- ✅ Bilibili
- ✅ And more...

## Deploy on Coolify

1. Create new service → Docker Compose
2. Point to this repo
3. Set environment variables:
   - `API_URL=https://cobalt.ashketing.com/`
   - `CORS_URL=https://repurpose.ashketing.com,https://stravix.com`
4. Deploy

## Local Development

```bash
# Copy env file
cp .env.example .env

# Start services
docker-compose up -d

# Test
curl http://localhost:9000/
```

## API Usage

```bash
# Download YouTube video
curl -X POST http://localhost:9000/ \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://youtu.be/dQw4w9WgXcQ"}'

# Download TikTok
curl -X POST http://localhost:9000/ \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://www.tiktok.com/@user/video/123"}'

# Audio only
curl -X POST http://localhost:9000/ \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{"url": "https://youtu.be/dQw4w9WgXcQ", "downloadMode": "audio"}'
```

## Response Format

```json
{
  "status": "tunnel",
  "url": "https://cobalt.ashketing.com/tunnel?id=...",
  "filename": "video.mp4"
}
```

## Optional: YouTube Cookies

If YouTube blocks even with WARP, add cookies:

1. Copy `cookies.example.json` to `cookies.json`
2. Export cookies from your browser (use a throwaway account)
3. Fill in the values
4. Restart: `docker-compose restart`

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| API_URL | required | Public URL of your instance |
| CORS_URL | * | Allowed origins for CORS |
| YOUTUBE_PROXY | socks5://warp:1080 | Proxy for YouTube requests |
| RATE_LIMIT | 100 | Requests per window |
| RATE_LIMIT_WINDOW | 60 | Window in seconds |
| DURATION_LIMIT | 10800 | Max video duration (3h) |

## Integrations

### Content Repurpose Tool
```typescript
const response = await fetch('https://cobalt.ashketing.com/', {
  method: 'POST',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    url: youtubeUrl,
    downloadMode: 'audio',
    audioFormat: 'mp3'
  })
});
const { url } = await response.json();
// url = direct download link
```

### Stravix
Same API - import media from any supported platform for content repurposing.

## Docs

- [Cobalt GitHub](https://github.com/imputnet/cobalt)
- [API Documentation](https://github.com/imputnet/cobalt/blob/main/docs/api.md)
- [Environment Variables](https://github.com/imputnet/cobalt/blob/main/docs/api-env-variables.md)
