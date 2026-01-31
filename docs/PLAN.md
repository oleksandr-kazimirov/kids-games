# Multiplication Game — Implementation Plan

> *Made with love for little ones who enjoy learning* ❤️

---

## 1. Overview

A tablet-optimized multiplication learning game for children (ages 7-9) built as a single HTML file with embedded CSS and JavaScript.

**Key Principles:**
- Single file, no dependencies
- Works offline
- Touch-friendly for tablets
- Responsive for all screen sizes
- Optional voice (reads problems aloud in English or Ukrainian)

---

## 2. Game Modes

### 2.1 Question Types

| Type | Display | Child Enters |
|------|---------|--------------|
| **Result** | `7 × 8 = ?` | The answer (56) |
| **Multiplier** | `? × 8 = 56` | The missing factor (7) |

### 2.2 Session Types

| Mode | Description | Best For |
|------|-------------|----------|
| **Fixed Tasks** | Set number of random problems | Quick practice |
| **Practice All** | All tables from 2 to max (e.g., max=5 → 40 problems) | Complete mastery |
| **Practice One** | Single table (e.g., 7×1 through 7×10) | Targeted practice |

**All modes:** Wrong answers repeat until correct! Game ends only when ALL problems are mastered.

---

## 3. Settings

| Setting | Options | Default |
|---------|---------|---------|
| Language | English / Ukrainian | English |
| Max Number | 2-10 | 10 |
| Question Type | Result / Multiplier | Result |
| Session Type | Fixed / Practice All / Practice One | Fixed |
| Tasks (Fixed only) | 5, 10, 15, 20 | 10 |
| Max Mistakes (Fixed only) | 1, 2, 3, Unlimited | 3 |

**Note:** Language is selected on the menu screen. Voice is always enabled and uses the selected language.

---

## 4. Screens

```
MENU ──► SETTINGS ──► GAME ──► RESULTS
  │
  └────► LEARN (view tables)
```

| Screen | Purpose |
|--------|---------|
| **Menu** | Play or Learn buttons |
| **Settings** | Configure game options |
| **Learn** | View multiplication tables |
| **Game** | Solve problems with numpad |
| **Results** | Score, time, mistakes review |

---

## 5. Game Screen Layout

```
┌─────────────────────────────────┐
│  Mastered: 5/40   00:45   ←3    │
├─────────────────────────────────┤
│         7  ×  8  =  ?           │
├─────────────────────────────────┤
│            [ 56 ]               │
├─────────────────────────────────┤
│   [ 1 ]  [ 2 ]  [ 3 ]          │
│   [ 4 ]  [ 5 ]  [ 6 ]          │
│   [ 7 ]  [ 8 ]  [ 9 ]          │
│   [    0    ]   [ C ]          │
├─────────────────────────────────┤
│         [ CHECK ]               │
│         [ FINISH ]              │
└─────────────────────────────────┘
```

---

## 6. Game Flow

```
Child enters digits
       │
       ▼
   Correct? ──► YES ──► Auto-submit (no CHECK needed!)
       │               Show green ✓ checkmark
       │               Green flash → Next problem
       │               Congrats popup (every 5 streak)
       │
       └──► NO ──► Must click CHECK
                   Red shake + show correct (5 sec)
                   Big green correct answer
                   Re-queue (practice modes)
                   Next problem
```

**Auto-submit:** Correct answers submit instantly — no button click needed!

---

## 7. Design

### Colors

| Purpose | Color |
|---------|-------|
| Primary | Blue `#4A90D9` |
| Success | Green `#4CAF50` |
| Error | Red `#E53935` |
| Timer | Orange `#F57C00` |

### Animations

- Screen transitions: fade + slide
- Correct: green glow
- Wrong: shake
- Congrats: pop-in

---

## 8. Code Structure

```javascript
CONFIG           // Settings & constants
Utils            // Helper functions
State            // Application state
ProblemGenerator // Create problems
UI               // DOM updates
Game             // Game logic
```

---

## 9. Files

```
kids-games/
├── README.md
├── LICENSE
├── docs/
│   └── PLAN.md
└── multiplication/
    └── multiplication-game.html
```

---

## 10. Testing Checklist

- [ ] Settings save correctly
- [ ] Result mode: `A × B = ?`
- [ ] Multiplier mode: `? × B = Z`
- [ ] Auto-submit on correct answer
- [ ] Wrong answer shows 5 sec with big green correct
- [ ] Fixed mode ends correctly
- [ ] Practice modes retry wrong answers
- [ ] Timer works in all modes
- [ ] Results show mistakes only
- [ ] Congrats every 5 streak
- [ ] Language selector on menu (English/Ukrainian)
- [ ] UI changes based on selected language
- [ ] Voice pronounces numbers as words
- [ ] Voice pronounces result on correct answer
- [ ] Voice toggle button on game screen (🔊/🔇)
- [ ] Auto-proceed after 3 sec on wrong answer
- [ ] Wait for pronunciation to finish before next task
- [ ] Show install instructions if Ukrainian voice not found
- [ ] FINISH button exits game and returns to menu
- [ ] Works on tablet/phone
- [ ] Works offline

---

*Built with care for curious young minds* 🌟
