<p align="center">
  <a href="./README.md">简体中文</a> | 
  <a href="./README_en.md">English</a>
</p>

---

# Slime Rhythm

A lightweight, beat-driven falling-style rhythm game based on HTML5 Canvas and the Web Audio API. This project comes with a fully functional web-based visual chart editor.

## The Game (`game.html`)
- **Slime Notes**: Refers to the beat-based falling mechanics of *Rift of the NecroDancer*, but without overly complex movement mechanics, closer to traditional falling-style rhythm games.
- **Judgment System**: Includes PERFECT, GREAT, GOOD, and MISS judgments. Supports hold notes.
- **Cross-Platform Support**:
  - PC: Keyboard input (`S`, `D`, `J`, `K`).
  - Mobile: Direct screen taps with multi-touch support.
- **Personalization**:
  - One-click toggle between Light / Dark mode.
  - Fully customizable UI theme color hue.
  - Supports uploading custom note patterns for each of the 4 lanes.
- **Accessibility**: Built-in Auto-Play mode for easy chart previewing.
- **Multi-Audio Format Support**: The game supports most mainstream audio formats, including MP3, OGG, WAV, FLAC, etc. (Note: Some browsers may have compatibility issues with certain formats).

## Chart Editor (`editor.html`)
- **Visual Editing**: Intuitive waterfall lane interface.
- **Flexible Beat Control**: Supports 1/1, 1/2, and 1/4 beat snapping precision. (It is recommended to only use 1/1 snapping; otherwise, reading the chart might become difficult).
- **Multiple Note Modes**: Supports drawing both short tap notes and long hold notes.
- **Multi-BPM / Speed Change Support**: Add or remove BPM changes at the current beat at any time, adapting to songs with variable tempos.
- **Local Import / Export**: Supports importing local audio files (most mainstream formats). Supports one-click import and export of charts in JSON format.

### Editor Guide

After opening `editor.html` in your browser:
- **Add Note**: Left-click on the grid.
- **Add Hold Note**: Switch the input mode to "Drag (Hold)" in the left panel, then hold the left mouse button and drag upwards on the grid.
- **Delete Note**: Right-click on an existing note.
- **Move Timeline**: Mouse wheel up or down.
- **Zoom Lanes**: Hold `Ctrl` + Mouse wheel.
- **Play / Pause**: `Space` bar.

## How to Run

Due to modern browser security policies (CORS and Web Audio API requiring user interaction), **do not double-click the HTML files directly to open them**. You must run this project through a local HTTP server.

### Method 1: Using VS Code (Recommended)
1. Open the project folder in VS Code.
2. Install the **Live Server** extension.
3. Right-click `game.html` or `editor.html` and select **"Open with Live Server"**.

### Method 2: Using Python
If you have Python installed, open a command line / terminal in the project root directory and enter:
```bash
# Python 3.x
python -m http.server 8000
```
Then visit `http://localhost:8000/game.html` in your browser.

## Project Structure

```text
slime-rhythm-game/
│
├── game.html          # Main game program
├── editor.html        # Chart editor
├── song_list.json     # Song list configuration loaded on the game's home page
│
├── sfx/               # Game sound effects folder
│   ├── perfect.mp3    # Perfect judgment SFX
│   ├── great.mp3      # Great judgment SFX
│   ├── good.mp3       # Good judgment SFX
│   ├── empty.mp3      # Empty swing / non-judgment area tap SFX
│   └── long.mp3       # Looping SFX during hold notes
└── music/             # Music and chart folder
    ├── music1.mp3     # Music file
    └── chart1.json    # Chart file
```

* Example of `song_list.json` data format:
```jsonc
[
  {
    "id": 1,                        // Unique ID or identifier for the song
    "title": "Test Track 1",        // Song title (displayed in bold in the song list)
    "artist": "Composer A",         // Artist/Composer name (displayed below the title)
    "audio": "music/music1.mp3",    // Relative path to the audio file
    "chart": "music/chart1.json"    // Relative path to the corresponding chart file (JSON format)
  }
]
```

* Example of `chart.json` data format:
```jsonc
{
  "bpmList": [      // BPM change nodes list
    {
      "beat": 0,    // The time this BPM change takes effect (in beats, 0 means the beginning)
      "bpm": 120    // Specific BPM value
    }
  ],
  "notes": [        // Falling notes list
    {
      "beat": 4,    // Exact time the note hits the judgment line (in beats)
      "lane": 0     // Lane index of the note (0~3 represent the four lanes from left to right)
    },
    {
      "beat": 5,    // Start time of a hold note (in beats)
      "lane": 2,
      "endBeat": 7  // End time of a hold note (in beats), only present in hold notes
    }
  ]
}
```