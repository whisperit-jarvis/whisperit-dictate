<div align="center">
  <img src="sponsor-images/whisperit-dictate-logo.png" alt="Whisperit Dictate" width="128" height="128">
  <h1>Whisperit Dictate</h1>
  <p><strong>Free, open-source, offline speech-to-text by <a href="https://whisperit.io">Whisperit</a></strong></p>
</div>

---

Whisperit Dictate is a free desktop companion app for local speech-to-text, brought to you by the [Whisperit](https://whisperit.io) team. It works completely offline on any computer — your voice never leaves your machine.

Press a shortcut, speak, and your words appear in any text field. No cloud, no subscription, no data leaving your device.

## Why Whisperit Dictate?

- **Free**: Accessibility tooling belongs in everyone's hands, not behind a paywall
- **Open Source**: Build on top of it, extend it, make it yours
- **Private**: Your voice stays on your computer — zero cloud processing
- **Simple**: One tool, one job — transcribe what you say and put it into a text box

## How It Works

1. **Press** a configurable keyboard shortcut to start/stop recording (or use push-to-talk mode)
2. **Speak** your words while the shortcut is active
3. **Release** and Whisperit Dictate processes your speech using Whisper
4. **Get** your transcribed text pasted directly into whatever app you're using

The process is entirely local:

- Silence is filtered using VAD (Voice Activity Detection) with Silero
- Transcription uses your choice of models:
  - **Whisper models** (Small/Medium/Turbo/Large) with GPU acceleration when available
  - **Parakeet V3** — CPU-optimized model with excellent performance and automatic language detection
- Works on **Windows**, **macOS**, and **Linux**

## Quick Start

### Installation

1. Download the latest release from the [releases page](https://github.com/whisperit-jarvis/whisperit-dictate/releases)
2. Install the application
3. Launch Whisperit Dictate and grant necessary system permissions (microphone, accessibility)
4. Configure your preferred keyboard shortcuts in Settings
5. Start transcribing!

### Development Setup

For detailed build instructions including platform-specific requirements, see [BUILD.md](BUILD.md).

## Architecture

Whisperit Dictate is built as a Tauri application combining:

- **Frontend**: React + TypeScript with Tailwind CSS for the settings UI
- **Backend**: Rust for system integration, audio processing, and ML inference
- **Core Libraries**:
  - `whisper-rs`: Local speech recognition with Whisper models
  - `transcription-rs`: CPU-optimized speech recognition with Parakeet models
  - `cpal`: Cross-platform audio I/O
  - `vad-rs`: Voice Activity Detection
  - `rdev`: Global keyboard shortcuts and system events
  - `rubato`: Audio resampling

### Debug Mode

Whisperit Dictate includes an advanced debug mode for development and troubleshooting. Access it by pressing:

- **macOS**: `Cmd+Shift+D`
- **Windows/Linux**: `Ctrl+Shift+D`

### CLI Parameters

Whisperit Dictate supports command-line flags for controlling a running instance and customizing startup behavior. These work on all platforms (macOS, Windows, Linux).

**Remote control flags** (sent to an already-running instance):

```bash
whisperit-dictate --toggle-transcription    # Toggle recording on/off
whisperit-dictate --toggle-post-process     # Toggle recording with post-processing on/off
whisperit-dictate --cancel                  # Cancel the current operation
```

**Startup flags:**

```bash
whisperit-dictate --start-hidden            # Start without showing the main window
whisperit-dictate --no-tray                 # Start without the system tray icon
whisperit-dictate --debug                   # Enable debug mode with verbose logging
whisperit-dictate --help                    # Show all available flags
```

Flags can be combined for autostart scenarios:

```bash
whisperit-dictate --start-hidden --no-tray
```

> **macOS tip:** When installed as an app bundle, invoke the binary directly:
>
> ```bash
> /Applications/Whisperit\ Dictate.app/Contents/MacOS/whisperit-dictate --toggle-transcription
> ```

## Known Issues & Current Limitations

This project is actively being developed and has some known issues. We believe in transparency about the current state.

### Major Issues (Help Wanted)

**Whisper Model Crashes:**

- Whisper models crash on certain system configurations (Windows and Linux)
- Does not affect all systems — issue is configuration-dependent

**Wayland Support (Linux):**

- Limited support for Wayland display server
- Requires [`wtype`](https://github.com/atx/wtype) or [`dotool`](https://sr.ht/~geb/dotool/) for text input to work correctly (see Linux Notes below)

### Linux Notes

**Text Input Tools:**

For reliable text input on Linux, install the appropriate tool for your display server:

| Display Server | Recommended Tool | Install Command                                    |
| -------------- | ---------------- | -------------------------------------------------- |
| X11            | `xdotool`        | `sudo apt install xdotool`                         |
| Wayland        | `wtype`          | `sudo apt install wtype`                           |
| Both           | `dotool`         | `sudo apt install dotool` (requires `input` group) |

Without these tools, the app falls back to enigo which may have limited compatibility, especially on Wayland.

**Other Notes:**

- **Runtime library dependency (`libgtk-layer-shell.so.0`)**: If startup fails with a shared library error, install the runtime package for your distro (`libgtk-layer-shell0` on Ubuntu/Debian, `gtk-layer-shell` on Fedora/Arch).
- The recording overlay is disabled by default on Linux because certain compositors treat it as the active window.
- If you have trouble with the app, running with `WEBKIT_DISABLE_DMABUF_RENDERER=1` may help.
- **Global keyboard shortcuts (Wayland):** On Wayland, system-level shortcuts must be configured through your desktop environment or window manager. Use the CLI flags as the command for your custom shortcut.

### Platform Support

- **macOS** (both Intel and Apple Silicon)
- **x64 Windows**
- **x64 Linux**

### System Requirements

**For Whisper Models:**

- **macOS**: M series Mac, Intel Mac
- **Windows**: Intel, AMD, or NVIDIA GPU
- **Linux**: Intel, AMD, or NVIDIA GPU (Ubuntu 22.04, 24.04)

**For Parakeet V3 Model:**

- **CPU-only operation** — runs on a wide variety of hardware
- **Minimum**: Intel Skylake (6th gen) or equivalent AMD processors
- **Performance**: ~5x real-time speed on mid-range hardware
- **Automatic language detection** — no manual language selection required

## Troubleshooting

### Manual Model Installation

If you're behind a proxy, firewall, or in a restricted network environment, you can manually download and install models. See the model URLs and installation paths in the [upstream documentation](https://github.com/cjpais/handy#manual-model-installation-for-proxy-users-or-network-restrictions).

### Custom Whisper Models

Whisperit Dictate can auto-discover custom Whisper GGML models placed in the `models` directory. Obtain a Whisper model in GGML `.bin` format (e.g., from [Hugging Face](https://huggingface.co/models?search=whisper%20ggml)), place it in your models directory, and restart the app.

## Contributing

1. Check existing issues on this repository
2. Fork the repository and create a feature branch
3. Test thoroughly on your target platform
4. Submit a pull request with a clear description of changes

## License

MIT License — see [LICENSE](LICENSE) file for details.

## Acknowledgements

Whisperit Dictate is a fork of [Handy](https://github.com/cjpais/handy) by [CJ Pais](https://github.com/cjpais). We are grateful for CJ's work making speech-to-text accessible to everyone.

Additional thanks to:

- **Whisper** by OpenAI for the speech recognition model
- **whisper.cpp and ggml** for cross-platform whisper inference/acceleration
- **Silero** for lightweight VAD
- **Tauri** team for the excellent Rust-based app framework
