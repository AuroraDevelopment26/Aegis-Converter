# Aegis Converter

A Windows desktop media converter with an Electron interface, a Python image conversion engine, and bundled FFmpeg video conversion.

## Features

- Batch conversion from raster images supported by Pillow and from SVG rasterized in Electron.
- PNG, JPEG, WebP, BMP, TIFF, and ICO output.
- JPEG/WebP quality control and automatic white backgrounds for transparent JPEG input.
- Video conversion to MP4, WebM, MOV, MKV, or AVI, with available audio tracks converted and preserved.
- Collision-safe output filenames; existing files are never overwritten.
- Local processing; image contents are not uploaded.
- Separate Logs and Settings views in the sidebar. Conversion logs are stored locally in the Electron user data folder.
- Optional update check against the latest GitHub Release.
- One-click portable Windows updates: downloads the matching release ZIP, verifies its GitHub SHA-256 digest, validates and extracts it, then restarts into the updated app.
- Inno Setup installer build for Windows.

Animated image formats are converted using their first frame. The available raster input formats depend on the Pillow codecs bundled with the application. SVG files are rasterized locally by Electron's Chromium engine before the Python conversion step. Video encoding is performed locally using the bundled FFmpeg 6.1.1 binary. Its GPL v3 license and build/source metadata are shipped alongside the binary in the installed app's `resources/ffmpeg` folder. Video subtitles are not copied.

Video output uses H.264/AAC for MP4, MOV, and MKV; VP9/Opus for WebM; and MPEG-4/PCM for AVI. WebM encoding can take longer because VP9 is computationally intensive.

Automatic updates currently apply only to the portable Windows build and require that its app folder is writable. Publish each update with a versioned portable asset named `Aegis-Converter-v<version>-win-x64.zip` (for example `Aegis-Converter-v0.3.0-win-x64.zip`). GitHub's release asset SHA-256 digest is checked before extraction. The user confirms **Download and install**; the app closes during replacement and restarts when it is complete. Inno Setup installs are not self-updated by this portable updater.

## Development

Requirements: Windows, Node.js, Python 3.10+, and Inno Setup 6 to create the installer.

```powershell
npm.cmd install
python -m pip install -r requirements.txt
npm.cmd start
```

If the Python launcher cannot find the intended interpreter, set `PYTHON_EXECUTABLE` to its full path before starting Electron or building the backend.

Run the Python backend tests with:

```powershell
python -m unittest discover -s backend/tests -v
```

## GitHub update checks

Set `repository` in [config/update.json](./config/update.json) to the public repository in `owner/name` format. Publish GitHub Releases with semantic version tags such as `v0.2.0`. The update check reports a newer release and can open its GitHub page; it does not silently download or install updates.

## Build the Windows installer

Install Inno Setup 6, then run:

```powershell
npm.cmd install
python -m pip install -r requirements.txt
npm.cmd run installer
```

The Electron app is staged in `dist/win-unpacked` and the Inno Setup installer is written to `dist/installer`. If `ISCC.exe` is not on `PATH` or in its standard install location, set `INNO_SETUP_COMPILER` to its full path.
