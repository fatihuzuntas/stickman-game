# 🎮 Stickman Canvas Demo

A modern, interactive stickman sandbox built with HTML5 Canvas, vanilla JavaScript (ES6+), and the LocalStorage API. The project includes a responsive canvas renderer, multi-character selection and control, per-character customization, multi-page (scene) management, preset/custom backgrounds, and a lightweight UI.

### 🔗 Live Demo
- Hosted on Vercel: [stickman-game-three.vercel.app](https://stickman-game-three.vercel.app/)

![Status](https://img.shields.io/badge/Status-Live%20Demo-brightgreen)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow)
![Canvas](https://img.shields.io/badge/Canvas-HTML5-blue)
![LocalStorage](https://img.shields.io/badge/Storage-LocalStorage-orange)

## ✨ Features

### 🎯 Core Game Mechanics
- **Smooth Movement:** Delta-time based 60 FPS game loop
- **Multi-Selection:** Ctrl/Cmd for multiple character selection and simultaneous control
- **Boundary Control:** Characters cannot exit the screen
- **Responsive:** Works on all screen sizes
- **High DPI:** Sharp rendering on Retina and 4K displays
- **Device Pixel Ratio (DPR) aware rendering** for crisp visuals on HiDPI/Retina
- **Arrow keys movement** with normalized diagonal speed and screen bounds clamping
- **Pause/Resume with P**, soft camera shake on R (reset formation)
- **Multi-select system:** toggle selection on canvas (Ctrl/Cmd click), or via keys (Z+1-9)
- **Input:** keyboard + touch (four-quadrant overlay) without passive mouse tracking

### 🎨 Character Customization
- **Dynamic Colors:** Custom line color for each character
- **Head Images:** Custom head visuals with PNG/JPG files
- **Equipment System:** Emoji or custom image equipment for left/right hand
- **Live Preview:** Real-time visual feedback during editing
- **Animation:** Walking when moving, open-limbed idle pose
- **Per-character color** and optional head image (PNG/JPG)
- **Equipment system** for left/right hand: emoji or custom image
- **Live character previews** in the Characters panel
- **Simple yet expressive animations:** walking when moving, open-limbs idle

### 🖼️ Scene Management
- **Multiple Pages:** Unlimited page creation and management
- **Background System:** Ready 2D patterns + custom image upload
- **LocalStorage:** All data permanently stored in browser
- **Auto-Save:** Changes saved instantly
- **Multiple pages (scenes)**, persisted in LocalStorage
- **Per-page background:** 5 preset 2D themes + custom upload (Data URL)
- **Per-page character roster + positions**, autosaved while editing/playing

### 🎮 Controls
- **Keyboard:** Arrow keys for movement, P (pause), R (center)
- **Touch:** Mobile-compatible four-region touch control
- **Mouse:** Only pressed dragging (no tracking)
- **Multi-Selection:** Multiple character selection in panel and scene
- Movement: Arrow keys
- Pause/Resume: P
- Reset formation (and light shake): R
- Select by number: 1-9 (first 9 characters), 0 = select all
- Multi-select: hold Z + number (toggle), Z + 0 = clear selection, Z alone = clear
- Actions (semantic motions): 
  - A: Cheer
  - S: Work
  - D: Jump
  - F: Wave
  - G: Fear
  - H: Crouch
  - J: Run
  - K: Sleep
  - L: Fight
  - O: Sit

Tip: You can also toggle selection via canvas clicks with Ctrl/Cmd.

## 🚀 Quick Start

### Installation
```bash
# Clone the project
git clone https://github.com/username/stickman-canvas-demo.git
cd stickman-canvas-demo

# Open the file (no web server required)
open index.html
```

### Usage
1. **Game Control:**
   - ⬅️➡️⬆️⬇️ Arrow keys for movement
   - `P` Pause/Resume
   - `R` Center all characters
   - `Ctrl + Click` Multi-selection

2. **Settings Panel:**
   - Click the floating ⚙️ button
   - 4 tabs: Game, Characters, Background, Pages

3. **Character Editing:**
   - Use cards in the Characters tab
   - Color, head image, equipment settings
   - Live preview with real-time visual feedback

### In-App UI
1) Click the floating ⚙️ button to open the Settings modal
2) Tabs: Game, Characters, Background, Pages
3) Characters tab: adjust color, head image, and equipment; live preview included
4) Background tab: choose from 2D presets or upload a custom image
5) Pages tab: add/select/delete scenes, all saved in LocalStorage

## 🏗️ Architecture

### Main Components

```javascript
// Game engine - Main coordinator
class Game {
  constructor(canvas, hudEl) {
    this.stickmen = [];           // Character list
    this.selected = new Set();    // Selected characters
    this.state = null;            // Application state
    this.input = new Input();     // Input manager
  }
}

// Character class - Each stickman
class Stickman {
  constructor(x, y, headImage, color, leftHand, rightHand) {
    this.x = x;                   // Position
    this.headImage = headImage;   // Head image
    this.color = color;           // Color
    this.leftHand = leftHand;     // Left hand equipment
    this.rightHand = rightHand;   // Right hand equipment
  }
}

// Input manager - Keyboard + Touch
class Input {
  getDirection() {
    // Combine keyboard and touch inputs
    // Diagonal speed normalization
  }
}
```

### Game Loop

```javascript
loop(now) {
  const dt = (now - this.lastTime) / 1000;  // Delta time
  
  // 1. Update
  if (!this.paused) {
    for (const s of this.stickmen) {
      s.update(dt, input, width, height);
    }
  }
  
  // 2. Render
  this.drawBackground();
  for (const s of this.stickmen) {
    s.draw(this.ctx);
  }
  
  // 3. Next frame
  requestAnimationFrame(this.loop);
}
```

### Data Structure

```javascript
const state = {
  currentPageIndex: 0,
  pages: [
    {
      id: 'page-abc123',
      name: 'Page 1',
      bgImageDataUrl: 'data:image/png;base64,...',
      stickmen: [
        {
          x: 100, y: 200,
          color: '#f5f5f5',
          headImageDataUrl: 'data:image/png;base64,...',
          leftHand: { type: 'emoji', value: '🔫', size: 22 },
          rightHand: null
        }
      ]
    }
  ]
}
```

## 🧠 Animation System

- Stickman maintains a simple animation phase and movement state
- When moving: walk cycle; when idle: open arms/legs pose
- Actions (A–L, O) temporarily override idle/walk and apply:
  - Torso/head rotations, vertical offset (jump/sit/crouch), limb swings
  - Per-action duration and animation speed
  - Auto-return to idle/walk after the action ends

## 🎨 Customization

### Adding New Backgrounds

```javascript
const NEW_BACKGROUND = {
  name: 'Custom Pattern',
  draw: (ctx, width, height) => {
    // Canvas drawing code
    ctx.fillStyle = '#1a1a1a';
    ctx.fillRect(0, 0, width, height);
    
    // Custom pattern drawing
    ctx.strokeStyle = '#333';
    // ... pattern code
  }
};
```

### Equipment System

```javascript
// Emoji equipment
const emojiEquip = {
  type: 'emoji',
  value: '🔫',
  size: 22
};

// Custom image equipment
const imageEquip = {
  type: 'image',
  value: HTMLImageElement,
  size: 28
};
```

### Background Presets
Canvas-drawn 2D presets (grid, diagonal lines, dots, waves, rings). You can also upload any image; it is stored as a Data URL per page.

## 🔧 Development

### Project Structure
```
stickman-canvas-demo/
├── index.html          # Main application file
├── README.md           # This file
└── assets/             # (Optional) Background images
    └── bg.jpg
```

### Technologies
- **HTML5 Canvas** - 2D graphics rendering
- **ES6+ JavaScript** - Modern JavaScript features
- **LocalStorage API** - Data persistence
- **CSS Grid/Flexbox** - Responsive UI
- **RequestAnimationFrame** - Performance animation

### Performance Optimizations
- ✅ Delta-time based movement
- ✅ DPR (Device Pixel Ratio) support
- ✅ Event delegation
- ✅ Auto-save throttling
- ✅ Canvas clearing optimization

### Tech Highlights
- HTML5 Canvas 2D API
- Vanilla JavaScript (ES6+)
- LocalStorage for persistence (Data URLs for images)
- CSS Grid/Flexbox for UI
- requestAnimationFrame game loop

### Deployment
- The app is static (single HTML file). You can deploy on any static host.
- Example: Vercel
  1. Create a new Vercel project
  2. Set the project root to the repository root
  3. Build command: (none)
  4. Output directory: `.`
  5. Ensure `index.html` is served as the entry point

### Performance Notes
- Delta-time based motion and animation
- DPR scaling for crisp rendering
- Autosave throttling
- Minimal allocations in the hot path

## 📚 Use Cases

### Education
- Programming concepts teaching
- Canvas API examples
- JavaScript game development

### Prototyping
- Quick character design
- UI/UX testing
- Animation prototypes

### Entertainment
- Simple gaming experience
- Character customization
- Creative scene design

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push your branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 🙏 Acknowledgements

- HTML5 Canvas API
- Modern JavaScript ES6+ features
- CSS Grid and Flexbox
- LocalStorage API

---

⭐ If you find this project useful, please give it a star on GitHub!
