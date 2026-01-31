<div align="center">

# Kids Games

**A collection of educational games for children**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Made with JavaScript](https://img.shields.io/badge/Made%20with-JavaScript-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Offline Ready](https://img.shields.io/badge/Offline-Ready-green.svg)](/)
[![No Dependencies](https://img.shields.io/badge/Dependencies-None-brightgreen.svg)](/)

*Simple, fun, and educational games that work anywhere — no internet required*

[Features](#-features) • [Games](#-games) • [Getting Started](#-getting-started) • [Development](#-development) • [Contributing](#-contributing)

</div>

---

## ✨ Features

- **Zero Dependencies** — Pure HTML, CSS, and JavaScript
- **Offline Ready** — Works without internet connection
- **Single File** — Each game is self-contained in one HTML file
- **Responsive Design** — Optimized for tablets, phones, and desktops
- **Touch Friendly** — Large buttons designed for young children
- **Modern UI** — Clean, engaging interface with smooth animations
- **No Installation** — Just open the HTML file in any browser

---

## 🎮 Games

### Multiplication Game

A tablet-optimized multiplication learning game for kids aged 7-9.

📍 **Location:** [`multiplication/multiplication-game.html`](multiplication/multiplication-game.html)

#### Game Modes

| Mode | Description |
|------|-------------|
| **Learn** | View multiplication tables (2-10) before practicing |
| **Fixed Tasks** | Complete a set number of problems with mistake limit |
| **Practice All** | Master all tables from 2 to selected max number |
| **Practice One** | Focus on mastering a single multiplication table |

#### Question Types

| Type | Example |
|------|---------|
| **Find Result** | `7 × 8 = ?` → Enter `56` |
| **Find Multiplier** | `? × 8 = 56` → Enter `7` |

#### Key Features

- ⏱️ **Time Tracking** — See how long each session takes
- ❌ **Mistake Review** — Review all incorrect answers at the end
- 🔄 **Retry System** — Wrong answers repeat later in practice modes
- 🎉 **Encouragement** — Congratulations popup every 5 correct in a row
- 📊 **Results Screen** — Shows mistakes with correct answers and total time

#### Configuration Options

| Setting | Options | Default |
|---------|---------|---------|
| Max Number | 2-10 | 10 |
| Question Type | Result / Multiplier | Result |
| Session Type | Fixed / Practice All / Practice One | Fixed |
| Tasks (Fixed mode) | 5, 10, 15, 20 | 10 |
| Max Mistakes (Fixed mode) | 1, 2, 3, Unlimited | 3 |

---

## 🚀 Getting Started

### Quick Start

1. **Download** the game file:
   ```
   multiplication/multiplication-game.html
   ```

2. **Open** in any web browser (Chrome, Safari, Firefox, Edge)

3. **Play!** No installation or internet required

### On Tablet/Phone

Transfer the HTML file via:
- 📧 Email attachment
- ☁️ Cloud storage (iCloud, Google Drive, Dropbox)
- 🔌 USB cable
- 📱 AirDrop (Apple devices)

Then open with any browser app.

### Recommended Devices

| Device | Experience |
|--------|------------|
| 10" Tablets (iPad, Galaxy Tab) | ⭐ Best |
| Phones | ✅ Good |
| Laptops/Desktops | ✅ Good |

*Minimum: Any device with a modern web browser (2017+)*

---

## 🛠️ Development

### Project Structure

```
kids-games/
├── README.md
├── .gitignore
└── multiplication/
    └── multiplication-game.html    # Self-contained game
```

### Architecture

Each game follows a clean, modular architecture:

```javascript
(() => {
    'use strict';

    // Configuration — All settings and constants
    const CONFIG = { ... };

    // Utilities — Helper functions (DOM, random, formatting)
    const Utils = { ... };

    // State — Centralized state management
    const State = { ... };

    // Domain Logic — Game-specific logic
    const ProblemGenerator = { ... };

    // UI Controller — View layer and DOM manipulation
    const UI = { ... };

    // Game Controller — Game flow and event handling
    const Game = { ... };

    // Initialization
    const init = () => { ... };
    init();
})();
```

### Design Principles

| Principle | Description |
|-----------|-------------|
| **Single File** | No build tools, no bundlers |
| **No Frameworks** | Vanilla JS for maximum portability |
| **ES6+ Syntax** | Modern JavaScript features |
| **CSS Variables** | Easy theming via custom properties |
| **Responsive First** | Media queries for all screen sizes |
| **Accessibility** | High contrast, large touch targets |

### Browser Support

| Browser | Status |
|---------|--------|
| Chrome | ✅ Supported |
| Safari | ✅ Supported |
| Firefox | ✅ Supported |
| Edge | ✅ Supported |
| Opera | ✅ Supported |
| Samsung Internet | ✅ Supported |

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### Ideas for Future Games

- [ ] ➕ Addition & Subtraction
- [ ] ➗ Division
- [ ] 🔢 Number Patterns
- [ ] 🔷 Shape Recognition
- [ ] 📝 Word Spelling
- [ ] 🧠 Memory Match

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 👥 Credits

<div align="center">

**Powered by Claude AI (Anthropic) and Oleksandr Kazimirov**

🤖 Game Design & Development: [Claude AI](https://claude.ai)
👨‍💻 Project Owner: Oleksandr Kazimirov

---

Made with ❤️ for kids who love learning

</div>
