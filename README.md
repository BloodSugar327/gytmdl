# Glomatico's YouTube Music Downloader
A Python CLI app for downloading YouTube Music songs with tags from YouTube Music.

**Discord Server:** https://discord.gg/aBjMEZ9tnq

## Why not just use yt-dlp directly?
While this project uses yt-dlp internally, all songs metadata are fetched using [YouTube Music's API](https://github.com/sigma67/ytmusicapi), which includes proper square cover, lyrics, track number, total tracks, etc.

## Prerequisites
* Python 3.8 or higher
* FFmpeg on your system PATH
    * Up to date binaries can be obtained from the links below:
        * Windows: https://github.com/AnimMouse/ffmpeg-stable-autobuild/releases
        * Linux: https://johnvansickle.com/ffmpeg/
* (Optional) The cookies file of your YouTube Music browser session
    * You can get your cookies by using one of the following extensions on your browser of choice at the YouTube Music website with your account signed in:
        * Firefox: https://addons.mozilla.org/addon/export-cookies-txt
        * Chromium based browsers: https://chrome.google.com/webstore/detail/gdocmgbfkjnnpapoeobnolbbkoibbcif
    * With cookies, you can download age-restricted content, private playlists and songs in premium formats if you have an active subscription. You will have to set the cookies file path using the command line arguments or the config file (see [Configuration](#configuration)).
    * **YouTube cookies can expire very quickly**. As a workaround, export your cookies using your browser incognito mode.
  
## Installation
Install the package `gytmdl` using pip:
```bash
pip install gytmdl
```

## Usage
```bash
gytmdl [OPTIONS] URLS...
```

### Examples
* Download a song:
    ```bash
    gytmdl "https://music.youtube.com/watch?v=3BFTio5296w"
    ```
* Download an album:
    ```bash
    gytmdl "https://music.youtube.com/playlist?list=OLAK5uy_lvpL_Gr_aVEq-LaivwJaSK5EbFd4HeamM"
    ```
* Choose which albums or singles to download from an artist:
    ```bash
    gytmdl "https://music.youtube.com/channel/UCwZEU0wAwIyZb4x5G_KJp2w"
    ```

**Songs that are not part of album (standard YouTube videos) are not supported**. To make sure you get valid links, use YouTube Music to search and enable filtering by albums, songs.

### Interactive prompt controls
* Enter - Confirm selection
* Space - Toggle selection
* Arrow keys - Move selection
* Ctrl + A - Select all

## Configuration
gytmdl can be configured by using the command line arguments or the config file. The config file is created automatically when you run gytmdl for the first time at `~/.gytmdl/config.json` on Linux and `%USERPROFILE%\.gytmdl\config.json` on Windows. Config file values can be overridden using command line arguments.
| Command line argument / Config file key   | Description                                                                 | Default value                |
| ----------------------------------------- | --------------------------------------------------------------------------- | ---------------------------- |
| `--save-cover`, `-s` / `save_cover`       | Save cover as a separate file.                                              | `false`                      |
| `--overwrite` / `overwrite`               | Overwrite existing files.                                                   | `false`                      |
| `--read-urls-as-txt`, `-r` / -            | Interpret URLs as paths to text files containing URLs separated by newlines | `false`                      |
| `--config-path` / -                       | Path to config file.                                                        | `<home>/.gytmdl/config.json` |
| `--log-level` / `log_level`               | Log level.                                                                  | `INFO`                       |
| `--print-exceptions` / `print_exceptions` | Print exceptions.                                                           | `false`                      |
| `--output-path`, `-o` / `output_path`     | Path to output directory.                                                   | `./YouTube Music`            |
| `--temp-path` / `temp_path`               | Path to temporary directory.                                                | `./temp`                     |
| `--cookies-path`, `-c` / `cookies_path`   | Path to .txt cookies file.                                                  | `null`                       |
| `--ffmpeg-path` / `ffmpeg_path`           | Path to FFmpeg binary.                                                      | `ffmpeg`                     |
| `--itag`, `-i` / `itag`                   | Itag (audio codec/quality).                                                 | `140`                        |
| `--cover-size` / `cover_size`             | Cover size.                                                                 | `1200`                       |
| `--cover-format` / `cover_format`         | Cover format.                                                               | `jpg`                        |
| `--cover-quality` / `cover_quality`       | Cover JPEG quality.                                                         | `94`                         |
| `--template-folder` / `template_folder`   | Template of the album folders as a format string.                           | `{album_artist}/{album}`     |
| `--template-file` / `template_file`       | Template of the song files as a format string.                              | `{track:02d} {title}`        |
| `--template-date` / `template_date`       | Date tag template.                                                          | `%Y-%m-%dT%H:%M:%SZ`         |
| `--exclude-tags`, `-e` / `exclude_tags`   | Comma-separated tags to exclude.                                            | `null`                       |
| `--truncate` / `truncate`                 | Maximum length of the file/folder names.                                    | `40`                         |
| `--no-config-file`, `-n` / -              | Don't load the config file.                                                 | `false`                      |

### Tag variables
The following variables can be used in the template folder/file and/or in the `exclude_tags` list:
- `album`
- `album_artist`
- `artist`
- `cover`
- `date`
- `lyrics`
- `media_type`
- `rating`
- `title`
- `track`
- `track_total`
- `url`

### Itags (audio codec/quality)
The following free itags are available:
* `140` (AAC 128kbps)
* `139` (AAC 48kbps)
* `251` (Opus 128kbps)
* `250` (Opus 64kbps)
* `249` (Opus 48kbps)
  
The following premium itags are available if provided a cookies file with an active subscription:
* `141` (AAC 256kbps)
* `774` (Opus 256kbps)

### Cover formats
The following cover formats are available:
* `jpg`
* `png`

