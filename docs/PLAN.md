# Multiplication Game — Implementation Plan

> *Made with love for little ones who enjoy learning*

---

## 1. Overview

A tablet-optimized multiplication learning game for children (ages 7-9) built as a single HTML file with embedded CSS and JavaScript.

**Key Principles:**
- Single file, no dependencies
- Works offline
- Touch-friendly for tablets
- Responsive for all screen sizes
- Voice reads problems aloud (English or Ukrainian)
- Voice input: kids can speak their answers

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
| **Practice All** | All tables from 2 to max | Complete mastery |
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

---

## 4. Screens

```
MENU ──► SETTINGS ──► GAME ──► RESULTS
  │
  └────► LEARN (view tables)
```

| Screen | Purpose |
|--------|---------|
| **Menu** | Language selector, Play/Learn buttons |
| **Settings** | Configure game options |
| **Learn** | View multiplication tables |
| **Game** | Solve problems with numpad |
| **Results** | Score, time, mistakes review |

---

## 5. Game Screen Layout

```
┌─────────────────────────────────────┐
│   ←        00:45        🔊          │  <- back, timer, voice toggle
│   Mastered: 5/40      Left: 35      │  <- progress info
├─────────────────────────────────────┤
│           7  ×  8  =  ?             │
├─────────────────────────────────────┤
│              [ 56 ]                 │
├─────────────────────────────────────┤
│       [ 1 ]  [ 2 ]  [ 3 ]          │
│       [ 4 ]  [ 5 ]  [ 6 ]          │
│       [ 7 ]  [ 8 ]  [ 9 ]          │
│         [   0   ]   [ C ]          │
├─────────────────────────────────────┤
│  🎤  ┌─────────────────────┐       │  <- mic for voice input
│      │       CHECK         │ GREEN  │
│      └─────────────────────┘       │
└─────────────────────────────────────┘
```

---

## 6. Game Flow

```
Child enters digits
       │
       ▼
   Correct? ──► YES ──► Auto-submit immediately
       │               Show green ✓ checkmark
       │               Voice: "fifty-six, correct!"
       │               Wait 500ms → Next problem
       │               Congrats popup (every 5 streak)
       │
       └──► NO ──► Auto-submit after 2 seconds (anti-cheat)
                   OR click CHECK button
                   Red shake + show correct answer
                   Voice: "The answer is fifty-six"
                   Wait 1000ms → Re-queue problem
                   Next problem
```

**Auto-submit:**
- Correct answers submit instantly
- Wrong answers auto-submit after 2 seconds (prevents cheating)

---

## 7. Design

### Button Hierarchy (UX Standards)

| Type | Style | Examples |
|------|-------|----------|
| Primary Action | Green button | START, CHECK, PLAY AGAIN |
| Secondary Action | Blue button | PLAY, LEARN |
| Navigation/Cancel | Text link | Back, Finish |

### Colors

| Purpose | Color |
|---------|-------|
| Primary | Blue `#4A90D9` |
| Success | Green `#4CAF50` |
| Error | Red `#E53935` |
| Timer | Orange `#F57C00` |

### Animations

- Screen transitions: fade + slide
- Correct: green glow + checkmark
- Wrong: shake
- Congrats: pop-in

---

## 8. Code Architecture (OOP)

```javascript
// Classes
Utility                   // Static helpers ($, randomInt, shuffle, formatTime)
SpeechService             // Voice output (speak, hasVoice, cancel)
SpeechRecognitionService  // Voice input (start, stop, handleResult)
NumberWords               // Number-to-word conversion + word-to-number parsing
Problem                   // Problem data, display HTML, speech text
SessionStrategy           // Base class for session types
  ├─ FixedSession
  ├─ PracticeAllSession
  └─ PracticeOneSession
SessionFactory            // Creates session by type
GameState                 // Centralized state management
UIController              // All DOM operations
GameController            // Game logic + voice input handling
Application               // Main entry point
```

---

## 9. Files

```
kids-games/
├── CLAUDE.md
├── README.md
├── LICENSE
├── docs/
│   ├── PLAN.md
│   └── FEATURES.md
└── multiplication/
    └── multiplication-game.html
```

---

## 10. Testing Checklist

- [ ] Settings save correctly
- [ ] Result mode: `A × B = ?`
- [ ] Multiplier mode: `? × B = Z`
- [ ] Auto-submit on correct answer (immediate)
- [ ] Auto-submit on wrong answer (2 sec delay)
- [ ] Wrong answer shows correct with green text
- [ ] Practice modes retry wrong answers
- [ ] Timer works in all modes
- [ ] Results show mistakes only
- [ ] Congrats every 5 streak
- [ ] Language selector on menu
- [ ] Voice pronounces numbers as words
- [ ] Voice toggle in header (🔊/🔇)
- [ ] Voice input with microphone button
- [ ] Green CHECK button prominent
- [ ] "Finish" as subtle text link
- [ ] Works on tablet/phone
- [ ] Works offline

---

## 11. Timing Configuration

| Event | Delay |
|-------|-------|
| Correct answer | 500ms |
| Wrong answer | 1000ms |
| Auto-submit (anti-cheat) | 2000ms |
| Congrats popup | 1500ms |

---

*Built with care for curious young minds*
