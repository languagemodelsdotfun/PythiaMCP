<img width="1024" height="1008" alt="PythiaMCP" src="https://github.com/user-attachments/assets/5803b152-5896-4258-b884-b23a2bd6365a" />


# Pythia Voice MCP

A voice interface sidecar for LLM clients. Provides text-to-speech output via MCP and speech-to-text input via wake word detection.

## Features

- **Text-to-Speech** - Local TTS using KittenTTS (ONNX, no cloud API)
- **Speech-to-Text** - Local STT using faster-whisper with Silero VAD
- **Wake Word Detection** - Trigger dictation with customizable wake word
- **MCP Server** - SSE transport for LLM client integration
- **Text Injection** - Dictated text injected into active window
- **Barge-In** - Interrupt TTS playback by speaking

## Requirements

### System Dependencies

**Debian/Ubuntu:**
```bash
sudo apt install xdotool portaudio19-dev
```

**Fedora:**
```bash
sudo dnf install xdotool portaudio-devel
```

**Arch:**
```bash
sudo pacman -S xdotool portaudio
```

### Python

Python 3.10+ required.

## Installation

```bash
# Clone or download
cd PythiaMCP-0.1.0

# Create virtual environment
python -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt
```

## Usage

```bash
# Start with defaults
python main.py

# Custom port
python main.py --port 8000

# Start with dictation enabled
python main.py --dictation

# List audio devices
python main.py --list-devices

# Verbose logging
python main.py -v      # INFO level
python main.py -vv     # DEBUG level
```

## Hotkeys

| Key | Action |
|-----|--------|
| `q` | Quit |
| `d` | Toggle dictation |
| `s` | Open settings |
| `i` | Select input device |
| `o` | Select output device |
| `v` | Cycle TTS voice |
| `c` | Copy MCP config |
| `?` | Help |
| `Up/Down` | Adjust TTS speed |
| `Space` | Skip TTS |

## MCP Configuration

Add to your LLM client's MCP config:

```json
{
  "mcpServers": {
    "pythia": {
      "url": "http://127.0.0.1:7984/sse"
    }
  }
}
```

### Available Tools

- `speak(text, voice?, speed?)` - Text-to-speech output

### Available Voices

- `expr-voice-2-f`, `expr-voice-2-m`
- `expr-voice-3-f`, `expr-voice-3-m`
- `expr-voice-4-f`, `expr-voice-4-m`
- `expr-voice-5-f`, `expr-voice-5-m`

## Configuration

Settings are stored in `~/.config/pythia/config.json`. Use the Settings modal (`s`) to configure:

- Wake word and position
- Whisper model (tiny.en → large-v3)
- Silence timeout
- TTS speed and voice
- Text injection method
- Barge-in behavior

## Wake Word Usage

1. Press `d` to enable dictation
2. Say your wake word (default: "Computer") followed by your message
3. Text is transcribed and injected into the active window

Example: "Computer, what is the weather today?" → injects "what is the weather today?"

## License

GPL-3.0 - See LICENSE file.
