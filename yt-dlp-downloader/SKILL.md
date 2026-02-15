---
name: yt-dlp-downloader
description: 使用 yt-dlp 下载视频、音频和字幕。支持 YouTube、Bilibili 等主流视频平台。用于用户给出视频链接时下载视频内容。
---

# yt-dlp 视频下载器

## 前置依赖安装

### 1. 安装 yt-dlp

```bash
# macOS - 使用 pip (推荐)
pip3 install yt-dlp

# macOS - 或使用 Homebrew
brew install yt-dlp

# Ubuntu/Debian
sudo apt install yt-dlp

# Windows (使用 winget)
winget install yt-dlp
```

### 2. 安装 ffmpeg (必须)

ffmpeg 用于合并视频和音频，必须安装：

```bash
# macOS - 使用 pip 安装 (推荐)
pip3 install ffmpeg-downloader
echo "y" | python3 -m ffmpeg_downloader install

# macOS - 或使用 Homebrew
brew install ffmpeg

# Ubuntu/Debian
sudo apt install ffmpeg

# Windows
winget install ffmpeg
```

验证安装：
```bash
yt-dlp --version
ffmpeg -version
```

## 下载流程

### 默认下载 (1080P 带音频)

除非用户明确指定分辨率，否则默认下载 1080P 并确保带音频：

```bash
cd ~/Downloads && yt-dlp -f "bestvideo[height<=1080][ext=mp4]+bestaudio[ext=m4a]/best[height<=1080][ext=mp4]/best" --write-subs --write-auto-subs --sub-lang "zh-CN,en" -o "%(title)s_1080p.%(ext)s" "视频链接"
```

关键参数：
- `bestvideo[height<=1080]` - 限制最高 1080P 视频
- `+bestaudio[ext=m4a]` - 合并最佳音质（确保有声音）
- `/best[height<=1080][ext=mp4]/best` - 降级方案
- `--write-subs --write-auto-subs` - 下载字幕和自动字幕
- `--sub-lang "zh-CN,en"` - 首选中文字幕

### 查看可用格式

```bash
yt-dlp --list-formats "视频链接"
```

### 用户指定分辨率

```bash
# 720p
cd ~/Downloads && yt-dlp -f "bestvideo[height<=720][ext=mp4]+bestaudio[ext=m4a]/best[height<=720][ext=mp4]/best" --write-subs --write-auto-subs --sub-lang "zh-CN,en" -o "%(title)s_720p.%(ext)s" "视频链接"

# 4K/原画 (不限制分辨率)
cd ~/Downloads && yt-dlp -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best" --write-subs --write-auto-subs --sub-lang "zh-CN,en" -o "%(title)s_4k.%(ext)s" "视频链接"
```

### 仅下载音频

```bash
cd ~/Downloads && yt-dlp -x --audio-format mp3 -o "%(title)s.mp3" "视频链接"
```

## 常见问题

### ffmpeg 未安装

如果下载时出现 "ffmpeg is not installed" 警告：
```bash
pip3 install ffmpeg-downloader
echo "y" | python3 -m ffmpeg_downloader install
```

ffmpeg 路径：`~/Library/Application Support/ffmpeg-downloader/ffmpeg/ffmpeg`

### 视频没有声音

检查格式是否包含 audio，使用 `--list-formats` 查看：
```bash
yt-dlp --list-formats "视频链接"
```

确保选择 `video+audio` 格式，如 `137+140` (YouTube) 或 `100026+30280` (Bilibili)

### 字幕问题

- Bilibili 字幕需要登录账号才能下载
- YouTube 自动字幕可能需要 deno 运行时

### 平台支持

支持 YouTube、Bilibili、抖音、小红书、Twitter、Instagram 等 1700+ 网站。
