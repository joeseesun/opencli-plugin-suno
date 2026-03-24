# opencli-plugin-suno

[OpenCLI](https://github.com/jackwener/opencli) plugin for [Suno](https://suno.com) AI music generation.

## Install

```bash
opencli plugin install github:joeseesun/opencli-plugin-suno
```

## Commands

| Command | Description |
|---------|-------------|
| `opencli suno generate` | 生成音乐，自动等待完成并下载 MP3/LRC/SRT |
| `opencli suno download` | 按 ID 下载歌曲（MP3 + 同步歌词） |
| `opencli suno history` | 查看最近生成的歌曲列表 |
| `opencli suno status` | 查询指定歌曲的生成状态 |
| `opencli suno refresh` | 验证登录状态并查看剩余积分 |
| `opencli suno lyrics` | 获取歌曲原始歌词文本 |

## Prerequisites

- Chrome browser with Suno logged in
- [opencli](https://github.com/jackwener/opencli) installed: `npm install -g @jackwener/opencli`
- opencli daemon running: opencli will start it automatically

## Usage

### Generate music

```bash
opencli suno generate \
  --prompt "[Verse]\nHello world\n[Chorus]\nThis is a test" \
  --tags "pop, male-vocals" \
  --title "My Song" \
  --dir ~/Music
```

### Download by ID

```bash
opencli suno download --ids "id1,id2" --dir ~/Music
```

### View history

```bash
opencli suno history --limit 10
```

### Check status

```bash
opencli suno status --ids "id1,id2"
```

### Check credits

```bash
opencli suno refresh
```

## How it works

This plugin uses Chrome DevTools Protocol (CDP) to:
1. Navigate to `suno.com/create` to acquire a Clerk JWT token from the browser session
2. Call Suno's internal API directly with the token
3. Download completed songs via yt-dlp (bypasses TLS fingerprinting)

**Idempotency**: `generate` uses localStorage to prevent duplicate submissions on retry.

**Lyrics**: Downloads LRC (karaoke format) and SRT (subtitle format) from Suno's aligned lyrics API.

## Author

[@vista8](https://x.com/vista8) · 微信公众号「向阳乔木推荐看」
