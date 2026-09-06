# 🎵 Freefy — a free, standalone Spotify-style music player

A single `index.html` file. No server, no build step, no install. It plays
**full-length mainstream songs** for free by using **YouTube** for both the
catalog (YouTube Data API v3) and playback (YouTube IFrame Player).

## Run it

Just open `index.html` in any browser (double-click it, or drag it into a tab).

The first time, it asks for a **free YouTube Data API key**. It's stored only in
your browser (`localStorage`) — it is never put in the file, so the file is safe
to commit and share.

## Get your free API key(s) (~5 min each)

1. Create a project in the [Google Cloud Console](https://console.cloud.google.com/projectcreate) (any name).
2. Enable the [YouTube Data API v3](https://console.cloud.google.com/apis/library/youtube.googleapis.com).
3. Go to [Credentials](https://console.cloud.google.com/apis/credentials) → **Create credentials** → **API key**.
4. Paste the key into Freefy when prompted (the 🔑 **Keys** button, top-right).

Leave billing **off** — it stays free. Each key gives **10,000 quota units/day**
(≈ 100 searches). Playback itself is unlimited and doesn't use quota. Quota
resets daily at midnight US Pacific time.

## Multiple keys = more daily quota

Freefy accepts a **pool of keys** (one per line in the 🔑 Keys dialog). It uses
them in order and automatically rolls to the next when one hits its daily limit,
falling back to Piped only when all are spent.

**Important:** quota is **per Google Cloud _project_, not per key.** Two keys in
the same project share one 10,000/day budget. To actually add quota:

- Make a **separate project per key** → each adds ~10,000 units/day (~100 searches).
- A Google account allows **~12 projects** by default → ~1,200 searches/day.
- **More Google accounts** multiply it further.

> ⚠️ Creating projects/accounts purely to exceed quota is a gray area under
> Google's Terms. For personal use it's rarely an issue, but you've been told.

> **Tip:** In the Cloud Console, restrict each key to the *YouTube Data API v3*
> so it can't be misused for anything else.

## Features

- 🔍 Search **Songs**, **Artists**, and **Albums**, filtered to *real songs*
  (no reactions/covers/live/sped-up), ranked audio-first like YouTube Music
- 🔗 **Paste a YouTube link** into the search box to play that exact video — the
  song filters are skipped, so one-off uploads by random channels (which keyword
  search deliberately hides) still play. Video, `youtu.be`, Shorts, playlist and
  channel links all work; a video link costs **1 quota unit** instead of 100, and
  resolves with **no API key at all** via the free backup
- ▶️ Full playback: play/pause, next/prev, seek, volume/mute, shuffle & repeat
- 🎤 **Synced lyrics** (from LRCLIB) — lines highlight in time and are
  click-to-seek. Open the **full-screen Now Playing** view (click the track,
  or press `L`) for big art + lyrics
- 📻 **Queue** — add to queue, play next, reorder, remove, clear
- 📀 Open an artist for top tracks, or an album/playlist for its track list;
  **add a whole album/artist straight into a playlist or the queue** in one click
- 📚 **Playlists** — create, rename, reorder tracks, delete. Add songs via the
  `＋` button or right-click. Saved forever; playing them makes **zero API calls**
- 🖱️ **Right-click** any song/album/artist for a full context menu
- ❤️ **Liked Songs** — tap the ♥ on any track
- 🏠 **Home** — greeting, genre tiles, recent searches, recently played
- ⏱️ **Sleep timer** and **playback speed** (0.75×–2×) in the ⋯ menu
- 💾 **Export / Import** — download your whole library to a file and restore it
- ⌨️ Keyboard: `Space` play/pause · `Shift+←/→` prev/next · `L` lyrics · `M` mute · `Esc` close
- 📱 Responsive layout

## Never wastes API calls

- **Search results are saved.** Search "Drake" once and it's stored in your
  browser — searching it again replays instantly with **no API call**. (Hit
  `↻ refresh` on a results page if you want fresh data.)
- **Your library is offline.** Songs in playlists / Liked already hold their
  video IDs, so they play using only YouTube's free player — no quota used.

## Automatic free fallback

If you ever burn through the daily YouTube quota (~100 searches), Freefy
**automatically switches searching to [Piped](https://github.com/TeamPiped/Piped)**,
a free keyless backup — no interruption. Playback always stays on YouTube's
official player. (You can also skip the API key entirely and run on Piped alone,
though the official key is more reliable.)

## How it works

| Need | Uses |
|------|------|
| Search / browse | YouTube Data API v3 → Piped fallback |
| Playback | YouTube IFrame Player API (always free) |
| Key, likes, playlists, search cache | Browser `localStorage` (nothing leaves your machine) |

*Playback is via YouTube's official embedded player, which stays visible per
YouTube's terms of service.*
