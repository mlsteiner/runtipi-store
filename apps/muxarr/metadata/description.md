# Muxarr

*Ever had your player pick the wrong audio language, or show 20 subtitle options you'll never use? Most media files ship with far more tracks than you need.*

Muxarr cleans them up by removing unwanted audio and subtitle tracks and standardizing track metadata. It uses **mkvmerge** for MKV files and **ffmpeg** with stream copy for other containers, so tracks are remuxed rather than re-encoded and there is zero quality loss. A 4GB file takes about a minute instead of hours, even on low-end hardware like a NAS or Raspberry Pi.

**Hooks into Sonarr & Radarr** for original language detection and automatic processing - new imports get cleaned up as they arrive.

### Quick Start

```yaml
services:
  muxarr:
    image: ghcr.io/kirovair/muxarr:latest
    container_name: muxarr
    environment:
      - TZ=Europe/Amsterdam
      - PUID=1000
      - PGID=1000
    volumes:
      - /path/to/data:/data
      - /path/to/media:/media
    ports:
      - 8183:8183
    restart: unless-stopped
```

## Features

- **Strip unwanted tracks** - remove audio (commentary, foreign dubs) and subtitles (SDH, foreign) without re-encoding, so quality is untouched. A typical 4GB file processes in about a minute depending on disk speed. Cutting spare audio tracks alone often saves 10-30%.
- **Original language detection** - integrates with your *arr stack so foreign films and shows always keep the correct audio track
- **Automatic processing** - webhook support to process new imports as they arrive
- **Per-directory profiles** - different language and track rules for different collections (e.g. anime vs western media)
- **Language priority & track limits** - control track ordering per language, limit tracks per language (e.g. keep only the best English audio track), and choose between best quality or smallest size
- **Smart metadata fixes** - cleans up encoder tags and codec dumps from track names. Uses mkvpropedit for metadata-only changes (instant, no remux needed)
- **Safe by default** - validates the output file before replacing the original. If anything fails, the original is untouched.
- **Library overview** - full breakdown of codecs, resolutions, and languages across your library

Supports Matroska (`.mkv`, `.webm`) and MP4-family (`.mp4`, `.m4v`).