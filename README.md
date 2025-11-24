# ynet — YouTube Content Downloader (.NET)

A polished .NET implementation of a YouTube downloader and MP3/MP4 tagging toolset. Designed for Windows desktop users and .NET developers who want a reliable, testable, and extensible downloader with metadata, album art, and lyrics support.

Keywords: ynet, youtube downloader .net, youtube mp3, youtube playlist downloader, youtubeexplode, ffmpeg, mp3 metadata, yt to mp3, yt downloader, windows youtube, youtube app, windows youtube download, youtube download, youtube mp4, youtube music

## Quick overview

- Project type: .NET 9.0 solution with library, tests, and platform UIs.
- Core library: `src/YTP.Core` — download and metadata plumbing.
- Windows UI: `src/YTP.WindowsUI` — WPF application (Windows-only).
- macOS UI: `src/YTP.MacUI` — Avalonia project for macOS.
- Tests: `src/YTP.Core.Tests` — unit tests for core functionality.
- Solution file: `ktd.sln`

## Features

- Download single YouTube videos.
- Download entire YouTube playlists.
- Save downloads as MP3 audio files.
- Save downloads as MP4 video files.
- Select MP3 bitrate (e.g., 128k, 192k, 256k, 320k).
- Select video quality (best available, up to 2160p where available).
- Intelligent title and artist parsing for metadata tagging.
- Multi-artist support (e.g., "Artist1 ft. Artist2").
- Automatic album naming: uses playlist title or derived album names.
- Embed high-quality cover art into MP3 files.
- Attempt lyrics scraping and embed lyrics into MP3s when found.
- Configurable output directory per user preference.
- Download queue management for sequential downloads.
- Real-time activity logging for debugging and monitoring.
- Ability to abort active downloads.
- Settings: FFmpeg path, default output directory, quality defaults, metadata toggles.
- Dark/Light/System appearance options in supported UIs.
- Clipboard paste support for quick URL entry.
- Toggleable progress bar for visual feedback.


## Prerequisites

- .NET 9.0 SDK (download from https://dotnet.microsoft.com/download).
- Windows Desktop (for building/running the WPF `YTP.WindowsUI` project).
- FFmpeg installed or available in the published bundle. The app will call `ffmpeg` by default if no path is configured.

## Build the solution

Open PowerShell in the repository root and run:

```powershell
# restore packages and build the entire solution
dotnet restore ktd.sln
dotnet build ktd.sln -c Release
```

Build output will go into the standard `bin/` and `obj/` folders under each project.

## Run the projects

Run the core console runner (quick confirmation of wiring and settings):

```powershell
dotnet run --project src\YTP.Core\YTP.Core.csproj
```

Run the Windows desktop application (WPF):

```powershell
dotnet run --project src\YTP.WindowsUI\YTP.WindowsUI.csproj
```

Run the macOS UI (Avalonia) on macOS:

```bash
dotnet run --project src/YTP.MacUI/YTP.MacUI.csproj
```

Notes:

- The WPF UI will only run on Windows.
- The Avalonia macOS project requires the appropriate runtime on macOS.

## Run tests

Execute the test suite for the core library:

```powershell
dotnet test src\YTP.Core.Tests\YTP.Core.Tests.csproj -c Release
```

## Publish & distribute

Publish the Windows UI as a single-file self-contained executable (example for x64):

```powershell
dotnet publish src\YTP.WindowsUI\YTP.WindowsUI.csproj -c Release -r win-x64 /p:PublishSingleFile=true /p:SelfContained=true -o publish\windows\YTP.WindowsUI
```

Publish the core library as a framework-dependent pack:

```powershell
dotnet publish src\YTP.Core\YTP.Core.csproj -c Release -o publish\core
```

Packaging recommendations:

- For WPF targets, publish for `win-x64` or `win-x86` as appropriate.
- Include an FFmpeg binary in your published bundle or instruct users to install FFmpeg.
- Test the published app on a clean Windows machine to ensure bundled assets are present.

## Configuration & FFmpeg

- The app exposes a Settings UI where users can set the FFmpeg path.
- If `FfmpegPath` is empty, the code falls back to the `ffmpeg` command and expects it on PATH.
- Settings are persisted by the library and loaded by each UI project.

## Troubleshooting

- "FFmpeg Not Found": ensure `ffmpeg` is installed or set the path in Settings.
- "Download Failed": verify the URL is public and not age-restricted or private.
- "No Metadata": some videos lack structured metadata; check the Activity Log for details.
- Crashes: check `ytp_unhandled_exception.txt` (or similar) in the OS temp folder for stack traces written by the app.

If you need help, open an issue: https://github.com/NASSERRRR/ytp/issues/new
### Do not open issues about the macos version, unless you want to fix them yourself

## Contributing

Contributions are welcome. Please:

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature`.
3. Add tests and documentation for your changes.
4. Open a Pull Request describing the change and motivation.

## Implementation notes

- Download logic uses `YoutubeExplode` for fetching stream and metadata.
- Conversion and merging are handled via FFmpeg called from `FFmpegService`.
- Image processing uses `SixLabors.ImageSharp`.
- Tagging uses `TagLibSharp` for ID3 and metadata writes.

## License

This repository is released under the MIT License — see `LICENSE` for details.
s