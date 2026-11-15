
![yt-4k-logo.jpg](images/yt-4k-logo.jpg)

A `yt-dlp` wrapper for downloading 4K video, best-quality video or audio, including age-restricted YouTube videos. Your browser's cookies are read only when a video actually requires a login.

## Features

- 4K by default: best quality up to 2160p, merged to MP4
- Best-quality, exact-2160p, MP4-preferred and audio-only modes
- Does not touch your browser by default: every download is first tried without cookies
- Age-restricted, bot-checked, private and members-only YouTube videos are retried with cookies from Brave (or another browser), only when YouTube demands a login
- `--no-cookies` guarantees the browser is never read; `--cookies-always` reads it up front
- Works on macOS and Linux, including Flatpak and Snap browser installs
- Hints on failure: locked cookie database, missing cookies, outdated `yt-dlp`

## Requirements

- `bash` 3.2 or newer
- [`yt-dlp`](https://github.com/yt-dlp/yt-dlp) (required)
- `ffmpeg` (needed to merge video and audio, and to convert audio)

## Install

```bash
curl -LO https://raw.githubusercontent.com/LinuxUser255/yt-4k/refs/heads/main/yt4k
```
- Make it executable: `chmod +x yt4k`
- Put it on your `PATH`: `sudo mv yt4k /usr/local/bin/`
- Check it: `yt4k --help`

## Usage

```
yt4k [OPTIONS] <URL> [OUTPUT_DIR]
```

- `yt4k URL` — 4K (up to 2160p) MP4 into the current directory
- `yt4k URL ~/Videos` — same, into `~/Videos` (or use `-o DIR`)
- `yt4k -b -i URL` — best quality at any resolution, video ID in the filename
- `yt4k -e URL` — exact 2160p only, fail if unavailable
- `yt4k -m URL` — prefer MP4 streams
- `yt4k -a --audio-format flac URL` — audio only (default format: `wav`)
- `yt4k -f URL` — list available formats, no download

## Options

- **Modes:** `--4k` (default), `-b/--best`, `-e/--exact`, `-m/--mp4`, `-a/--audio`, `--audio-format FMT`, `-f/--formats`
- **Output:** `-o/--output DIR`, `-i/--id`, `--retries N`
- **Cookies:** `--browser NAME`, `--profile NAME|PATH`, `--cookies FILE`, `--cookies-always`, `--no-cookies`
- **Help:** `-h/--help` lists everything, with examples

## Environment variables

- `YT4K_OUTPUT_DIR` — default output directory
- `YT4K_OUTPUT_TEMPLATE` — `yt-dlp` filename template (default `%(title)s.%(ext)s`)
- `YT4K_MAX_HEIGHT` — maximum video height (default `2160`)
- `YT4K_AUDIO_FORMAT` — default audio format (default `wav`)
- `YT4K_RETRIES` — retries per download (default: `yt-dlp`'s own)
- `YT4K_BROWSER` — browser to read if cookies are needed (default `brave`)
- `YT4K_BROWSER_PROFILE` — browser profile name or path
- `YT4K_COOKIES` — path to a `cookies.txt`; skips the browser entirely
- `YT4K_COOKIES_MODE` — `auto` (default: browser read only when a YouTube video demands a login), `always` or `never`
- `DEBUG=1` — print the `yt-dlp` command line

## Cookies and age-restricted videos

- By default no cookies are read and the browser is not touched; the download is retried with browser cookies only if YouTube refuses it with a login demand
- The browser profile must be signed in to an age-verified YouTube account
- **macOS:** the terminal needs Full Disk Access to read the browser profile; without it a warning explains the fix
- **Linux:** the profile is read from `~/.config` (or `XDG_CONFIG_HOME`), or from a Flatpak or Snap install
- If the profile lives elsewhere, pass its path with `--profile /path/to/profile`
- If extraction fails with "database is locked", quit the browser and re-run
- To avoid the browser altogether, export a `cookies.txt` and use `--cookies FILE`

## Suggested aliases

- `alias yt='yt4k '` — 4K video
- `alias yta='yt4k -a --audio-format wav '` — WAV audio
- `alias ytb='yt4k -b -i '` — best quality, video ID in the filename

## License

- GNU General Public License v3.0 — see [LICENSE](LICENSE)
