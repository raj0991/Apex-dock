# Apex Dock for Rainmeter

A modern, highly optimized macOS-inspired interactive application dock and taskbar skin for [Rainmeter](https://www.rainmeter.net/).

Featuring genuine cosine bell-curve magnification, interactive dock ball physics, running application indicators, drag-and-drop shortcut management, and an intelligent **zero-CPU sleep engine**.

---

## ✨ Key Features

- **macOS-Style Wave Magnification**: Smooth cosine bell-curve icon expansion on hover with customizable responsiveness and scale factors.
- **Interactive Dock Ball**:
  - **Hover Mode**: Follows the active cursor and continuously bounces rhythmically on top of whichever icon is currently hovered.
  - **Idle Patrol Mode**: Smoothly patrols and bounces back and forth across the dock when inactive (can be toggled on/off).
- **Zero-CPU Sleep Mode**:
  - When the cursor leaves the dock, all icons smoothly collapse back to rest.
  - Once resting, the skin pauses its update loop (`UpdateDivider=-1`), resulting in **0% CPU** and zero unnecessary redraws.
  - Instantly wakes up the millisecond your cursor touches the dock.
- **Independent Animation Controls**:
  - Toggle **Ball Animation** and **Icon Hover Animation** separately.
  - Independent custom speeds for hover zoom responsiveness and ball bounce rates.
- **Running Application Indicators**:
  - Integrated process measures (`MeasureProcess_*`) detect when applications like Steam, VS Code, Android Studio, etc., are running.
  - Highlights running apps with an elegant rounded neon-cyan glowing border.
  - Windows UWP app detection (`WinCheck.exe`) for System Settings, Recycle Bin, and Explorer.
- **Drag & Drop Reordering**:
  - Click and drag icons left or right along the dock to reorder them in real time.
  - Order changes automatically persist into `Taskbar.ini`.
- **Drag & Drop Shortcut Adding**:
  - Drag any `.exe`, `.lnk`, or file directly from your desktop or File Explorer onto the dock.
  - Automatically extracts high-resolution icons and configures launch paths via `AddShortcut.exe`.
- **Integrated Menus & Utilities**:
  - **App Menu** (`Right-Click on any icon`): Launch, change icon, edit properties, or delete app from dock.
  - **Power Menu**: Built-in access to Shutdown, Restart, Sleep, and Lock.
  - **Windows Search**: Instant trigger for Windows Search overlay.

---

## 🎛️ Context Menu (Right-Click Skin)

Right-click anywhere on the dock background to open the main settings menu:

| Option | Action / Description |
| :--- | :--- |
| **Add App / Shortcut...** | Opens the shortcut creation wizard to browse and add an executable. |
| **Help** | Opens the documentation. |
| **Ball Animation: [ON] / [OFF]** | Toggles the dock ball indicator on or off. |
| **Set Ball Speed... [#BallSpeed#x]** | Prompts for a multiplier (e.g. `1.0`, `1.5`, `2.0`) controlling ball patrol & bounce speed. |
| **Icon Hover Animation: [ON] / [OFF]** | Toggles the icon magnification effect on hover. |
| **Set Hover Speed... [#HoverSpeed#x]** | Prompts for a multiplier controlling how fast icons expand and collapse on hover. |

---

## ⚙️ Configuration & Variables (`Variables.inc`)

All user preferences are stored in `@Resources\Variables.inc`:

```ini
[Variables]
; Dimensions
IconSize=90               ; Base size of dock icons in pixels
RotationAngle=0           ; Icon rotation angle (0 for standard)

; Spacing & Layout
HorizontalGap=50          ; Spacing between icons horizontally
VerticalGap=30            ; Spacing between icons vertically
Orientation=1             ; 1 = Horizontal, 0 = Vertical
Horizontal=1
Vertical=0

; Margins & Padding
PadTop=80                 ; Top padding for dock expansion space
PadRight=20
PadBottom=15
PadLeft=20

; Animations
BallAnimation=1           ; 1 = Ball enabled, 0 = Ball disabled
BallAnimationState=[ON]
BallSpeed=1.5             ; Ball bounce & patrol speed multiplier

IconZoomAnimation=1       ; 1 = Icon hover magnification enabled, 0 = disabled
IconZoomState=[ON]
HoverSpeed=1.5            ; Icon hover lerp speed multiplier
```

---

## 📁 Directory Structure & Architecture

```
Apex Dock/
├── Taskbar.ini                 # Primary Rainmeter skin definition
├── README.md                   # This documentation file
├── HELP.txt                    # Shortcut to documentation for context menu
└── @Resources/
    ├── Variables.inc           # User-configurable variables & states
    ├── Style.inc               # Common icon styles and placement meters
    ├── DockAnimation.lua       # High-performance Lua animation & state engine
    ├── Addons/                 # Native helper utilities (.NET / C#)
    │   ├── AddShortcut.exe     # Icon extractor & shortcut installer
    │   ├── AppMenu.exe         # Per-icon right-click context menu
    │   ├── PowerMenu.exe       # Quick power options menu
    │   ├── WinCheck.exe        # UWP / Explorer running window detector
    │   └── WinSearch.exe       # Windows search trigger
    └── Images/                 # Icon graphic assets
        ├── System/             # Search, Computer, Trash, Power icons
        ├── Coding/             # VS Code, Android Studio icons
        ├── Misc/               # Steam and other app icons
        └── Custom/             # Icons created for user-added apps
```

---

## 🚀 How It Works Under The Hood

### 1. High-Efficiency Animation Engine (`DockAnimation.lua`)
- **Dirty Checking**: Instead of sending redundant `!SetOption` and `!UpdateMeter` bangs across all meters every tick, the script caches previous `(x, y, w, h)` coordinates and only issues bangs to Rainmeter when values visually change.
- **Sleep / Wake State Machine**:
  - When idle with ball disabled, `UpdateDivider` is set to `-1` and execution halts completely.
  - Any mouse interaction immediately restores `UpdateDivider=1` for butter-smooth 60 FPS animation.
- **Mathematical Cosine Wave**: Uses a bell-curve cosine distribution ($\cos^{1.25}$) across adjacent neighbors within radius $R=2.4$ for realistic macOS magnification.

### 2. Add Shortcut & App Management
- Dropping an executable on the dock invokes `AddShortcut.exe "$File$"`.
- Extracts 32-bit transparent icons directly from the file's icon table and creates both normal and hover brightness variants in `@Resources\Images\Custom\`.
- Automatically appends the new `[Meter]` and `[Border_Meter]` sections into `Taskbar.ini`.

---

## 💡 Tips & Customization

- **Adjust Dock Position**: Click and drag while holding `Ctrl` in Rainmeter, or set coordinates in Rainmeter Manage tool (default is centered at bottom of screen).
- **Faster Hover Zoom**: Right-click dock -> `Set Hover Speed...` -> type `2.0` or `2.5` for instantaneous, ultra-snappy icon growth.
- **Change an App's Icon**: Right-click the app's icon -> choose `Edit Icon` to assign any `.png` image of your choice.
- **Remove an App**: Right-click the app's icon -> choose `Delete` to remove it from the dock.
