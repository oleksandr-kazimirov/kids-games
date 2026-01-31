# Claude Code Instructions

## Project Overview
Kids educational games - single HTML files, no dependencies, offline-capable.

## Current Game
**Multiplication Game** (`multiplication/multiplication-game.html`)
- Tablet-optimized for children ages 7-9
- Single HTML file with embedded CSS/JS
- Bilingual: English and Ukrainian
- Voice pronunciation using Web Speech API
- Voice input: kids can speak answers using Speech Recognition API

## IMPORTANT: Keep Files in Sync
When making ANY changes, ALWAYS update these files together:
1. `docs/FEATURES.md` - BDD scenarios (Gherkin format, labeled A, B, C...)
2. `docs/PLAN.md` - Implementation plan and specifications
3. `multiplication/multiplication-game.html` - The actual code

**Never change code without updating FEATURES.md and PLAN.md accordingly!**

## Documentation Files
- `docs/FEATURES.md` - BDD scenarios in Gherkin format for all features
- `docs/PLAN.md` - Implementation plan, UI wireframes, design specs

## Architecture
OOP with ES6 classes:
- `Utility` - static helpers
- `SpeechService` - voice output (text-to-speech)
- `SpeechRecognitionService` - voice input (speech-to-text)
- `NumberWords` - number-to-word and word-to-number conversion
- `Problem` - problem data and display
- `SessionStrategy` pattern - `FixedSession`, `PracticeAllSession`, `PracticeOneSession`
- `SessionFactory` - creates session by type
- `GameState` - centralized state
- `UIController` - DOM operations
- `GameController` - game logic + voice input
- `Application` - main entry

## Key Files
```
kids-games/
├── CLAUDE.md
├── README.md
├── LICENSE
├── docs/
│   ├── PLAN.md        <- Update with any spec changes
│   └── FEATURES.md    <- Update BDD scenarios
└── multiplication/
    └── multiplication-game.html
```

## Coding Standards
- Pure ES6+ JavaScript, no frameworks
- Single file per game (HTML + embedded CSS/JS)
- OOP with classes, Strategy pattern for variants
- Minimize if statements - use object lookups, ternaries
- No empty lines between code blocks
- Compact but readable

## Commit Guidelines
- Short, descriptive messages
- No Co-Authored-By line

## Testing
Open HTML file directly in browser. Test on:
- Desktop browser
- Tablet (primary target)
- Mobile phone

## Design Principles
- Large touch targets (48px minimum)
- Green buttons for primary actions (START, CHECK)
- Text links for navigation (Back, Finish)
- Voice toggle in header area
- High contrast, kid-friendly colors
