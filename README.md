<p align="center">
  <img src="./logo-miaumiau-02.png" alt="PDFronin" width="360" />
</p>

# MiauMiau

Native Windows voice-to-text with local model. Press a hotkey, speak, your words get typed into the focused window. Transcription runs locally on your GPU via whisper.cpp (Vulkan).

## Quick start

Download `MiauMiau.exe` from the latest release — it's a self-contained single file, no installation required. Just run it.

Default hotkey: **Ctrl+Shift+R** (toggle mode). Press once to start recording, press again to stop. The transcribed text is pasted at your cursor and copied to the clipboard.

## Stack

- **UI:** WPF on .NET 9 (Windows-native, no Chromium, no Python runtime)
- **STT:** [Whisper.net](https://github.com/sandrohanea/whisper.net) with the Vulkan runtime — GPU-accelerated on any AMD / Intel / NVIDIA GPU on Win11, CPU fallback when no Vulkan device is present
- **Audio:** NAudio (WASAPI)
- **Hotkeys:** `RegisterHotKey` (toggle mode) or low-level keyboard hook (push-to-talk mode)
- **Tray:** Hardcodet.NotifyIcon.Wpf
- **LLM post-processing (optional):** OpenRouter for cleanup / translation / custom prompts

## Build

Prereq: .NET 9 SDK.

```bat
build.bat
```

Output: `dist\MiauMiau\MiauMiau.exe` (self-contained single-file, ReadyToRun-compiled, ~30-50 MB).

## Installer (optional)

Prereq: [Inno Setup 6](https://jrsoftware.org/isinfo.php).

```bat
iscc installer.iss
```

Output: `installer_output\MiauMiau-Setup-1.3.1.exe`.

## Config

`%APPDATA%\miaumiau\config.json` (auto-created on first run; a `meowtype` legacy dir is migrated automatically).

Whisper models are downloaded on demand to `%APPDATA%\miaumiau\models\`.

Changing the Whisper model, device, language, or AI settings requires an app restart.

## Notes

- **Vulkan vs CUDA.** We ship Vulkan only — works on every modern GPU on Win11 with stock drivers and avoids the 2+ GB CUDA toolkit. Throughput is comparable to CUDA for the Whisper workload.
- **Portable.** The exe is fully self-contained (bundles .NET runtime and all dependencies). Copy it anywhere and run — no installer, no .NET SDK, no setup.
