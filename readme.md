[简体中文](#节奏史莱姆) | [English](#Slime-Rhythm)

# 节奏史莱姆

一个基于 HTML5 Canvas 和 Web Audio API 开发的轻量级、节拍驱动下落式音乐游戏。本项目自带一个功能完善的 Web 端可视化谱面编辑器。

## 游戏本体 (`game.html`)
- **史莱姆音符**：参照了Rift of the NecroDancer的节拍式下落方式，但没有过多复杂的移动机制，更接近传统的下落式音游。
- **判定系统**：包含 PERFECT, GREAT, GOOD, MISS 四种判定，支持长按音符。
- **双平台适配**：
  - PC端使用键盘输入（S, D, J, K）。
  - 移动端直接点击屏幕，支持多点触控。
- **个性化设置**：
  - 支持亮色 / 暗色模式一键切换。
  - 游戏 UI 主题色调全色相自定义。
  - 支持为 4 个轨道分别上传自定义音符图案。
- **辅助功能**：内置自动打歌模式，方便预览谱面。
- **多音频格式支持**：游戏本身支持大部分主流音频格式，包括MP3、OGG、WAV、FLAC等，但部分浏览器可能对部分音频格式有兼容性问题。

## 谱面编辑器 (`editor.html`)
- **可视化编辑**：直观的瀑布流轨道界面。
- **灵活的节拍控制**：支持 1/1、1/2、1/4 三种节拍吸附精度。（建议只使用 1/1 吸附精度，否则可能造成读谱困难）
- **多重音符模式**：支持短按和长按音符的绘制。
- **多 BPM 变速支持**：支持在当前拍随时添加或移除 BPM 变速点，适配变速乐曲。
- **本地导入导出**：支持本地音频文件导入，支持大部分主流音频格式。支持 JSON 格式的谱面一键导入与导出。

### 编辑器操作指南

在浏览器中打开 `editor.html` 后：
- **添加音符**：鼠标左键点击网格。
- **添加长按音符**：在左侧面板将输入模式切换为“长按 (Drag)”，然后在网格上按住左键向上拖动。
- **删除音符**：鼠标右键点击已有音符。
- **移动时间轴**：鼠标滚轮向上或向下。
- **缩放轨道**：按住 `Ctrl` 键 + 鼠标滚轮。
- **播放/暂停**：空格键 (Space)。

## 如何运行

由于现代浏览器的安全策略限制（跨域 CORS 以及 Web Audio API 需要用户交互），**请勿直接双击打开 HTML 文件**。必须通过本地 HTTP 服务器运行此项目。

### 方法一：使用 VS Code (推荐)
1. 在 VS Code 中打开本项目的文件夹。
2. 安装扩展插件 **Live Server**。
3. 右键点击 `game.html` 或 `editor.html`，选择 **"Open with Live Server"**。

### 方法二：使用 Python
如果安装了 Python，可以在项目根目录下打开命令行/终端，输入：
```bash
# Python 3.x
python -m http.server 8000
```
然后在浏览器中访问 `http://localhost:8000/game.html`。

## 项目结构

```text
slime-rhythm-game/
│
├── game.html          # 游戏主程序
├── editor.html        # 谱面编辑器
├── song_list.json     # 游戏主页加载的歌单配置清单
│
├── sfx/               # 游戏音效文件夹
│   ├── perfect.mp3    # Perfect 判定音效
│   ├── great.mp3      # Great 判定音效
│   ├── good.mp3       # Good 判定音效
│   ├── empty.mp3      # 空挥/非判定区敲击音效
│   └── long.mp3       # 长按期间的循环音效
└── music/             # 音乐盒谱面文件夹
    ├── music1.mp3     # 音乐
    └── chart1.json    # 谱面文件
```

* `song_list.json` 数据格式示例：
```jsonc
[
  {
    "id": 1,                        // 歌曲的唯一编号或标识符
    "title": "测试曲目 1",           // 歌曲名称（将加粗显示在选歌界面列表中）
    "artist": "作曲家 A",            // 曲师/艺术家名称（显示在标题下方）
    "audio": "music/music1.mp3",    // 音乐音频文件的相对路径（支持绝大多数主流音频格式）
    "chart": "music/chart1.json"    // 对应的游戏谱面文件（JSON格式）的相对路径
  }
]
```

* `chart.json` 数据格式示例：
```jsonc
{
  "bpmList": [      // BPM 变速节点列表
    {
      "beat": 0,    // 该变速点生效的时间（以拍为单位，0 代表开局）
      "bpm": 120    // 具体的 BPM 值
    }
  ],
  "notes": [        // 游戏下落音符列表
    {
      "beat": 4,    // 音符准确落到判定线的时间（以拍为单位）
      "lane": 0     // 音符所在的轨道编号（0~3 分别代表从左到右的四条轨道）
    },
    {
      "beat": 5,    // 长按音符（Hold）的起始时间（拍）
      "lane": 2,
      "endBeat": 7  // 长按音符的结束时间（拍），仅长按音符带有此参数
    }
  ]
}
```

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