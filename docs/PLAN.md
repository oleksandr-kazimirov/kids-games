# Multiplication Learning Game for Kids - Implementation Plan

## Overview
A tablet-optimized (10-inch) HTML/JavaScript multiplication game for children with configurable difficulty and game modes.

## Core Requirements

### Game Configuration (with defaults)
| Setting | Options | Default | Description |
|---------|---------|---------|-------------|
| Practice number (max) | 2-10 | 10 | One multiplier limited to [2, max], other always [2, 10] |
| Game mode | Result / Multiplier | Result | Find answer (Z) or find missing factor (A or B) |
| Session type | Fixed / Practice All / Practice One | Fixed | Fixed = set tasks; Practice All = all tables; Practice One = single table |
| Tasks per session | 5, 10, 15, 20 | 10 | Number of problems (only for Fixed mode) |
| Allowed mistakes | 1, 2, 3, unlimited | 3 | Game ends when exceeded (only for Fixed mode) |

### Session Types

**Fixed Mode (original):**
- Set number of tasks (5/10/15/20)
- Game ends when tasks completed OR mistake limit reached
- Shows results at end

**Practice All Mode (NEW):**
- Plays ALL tables from 2 to maxNumber (e.g., max=5 → tables 2,3,4,5)
- Each table has 10 problems (×1 through ×10)
- Total = (maxNumber - 1) × 10 tasks (e.g., max=5 → 4 × 10 = 40 tasks)
- Wrong answers: problem goes back into queue, repeats later
- **Show correct answer**: When wrong, display correct answer before moving on
- No game over from mistakes - just keep practicing
- Shows congratulations message every 5 correct answers in a row
- Ends only when all combinations mastered
- **Results show only mistakes**: Only display problems that were answered incorrectly
- **Time tracking**: Show elapsed time during game and total time in results

**Practice One Mode (NEW):**
- Select a single number (2-10) to practice
- Plays all 10 combinations for that table (e.g., 5×1 through 5×10)
- Same retry logic as Practice All - wrong answers repeat later
- Show correct answer on mistake before moving on
- Results show only mistakes
- Time tracking included

### Game Mechanics
- **Result Mode**: Show `A × B = ?` → child enters Z
- **Multiplier Mode**: Show `? × B = Z` or `A × ? = Z` → child enters missing multiplier
- **Multiplication range**:
  - One multiplier: [2, selected_max] (the one being practiced)
  - Other multiplier: [2, 10] (always full range, randomly chosen)
  - Position (A or B) is randomized each problem
  - Example: If max=5, problems could be: `3 × 8`, `7 × 4`, `2 × 9`, etc.
- **No duplicate pairs**: Treat A×B and B×A as same pair (if 2×7 shown, 7×2 won't appear in session)
- Track score, mistakes, and progress
- **Store all answers**: Keep record of each problem with user's answer for results screen

## Project Cleanup

Remove existing Java/Maven files:
```
DELETE:
├── pom.xml
├── src/                        # Remove entire src directory
│   └── main/java/...
└── .idea/                      # Optional: remove IDE config if desired
```

## File Structure (after cleanup)
```
/Users/iuade0ez/IdeaProjects/custom/multiplying-kids-game/
├── .gitignore
└── multiplication-game.html    # Single self-contained game file
```

**Single file contains:**
- Embedded `<style>` tag with all CSS
- Embedded `<script>` tag with all JavaScript
- No external dependencies (no CDN, no frameworks)
- Can be opened directly on tablet browser or shared via file transfer

## Implementation Steps

### Step 0: Clean Up Project
1. Delete `pom.xml`
2. Delete `src/` directory (contains Java files)
3. Update `.gitignore` for HTML project (remove Java-specific entries)

### Step 1: Create Single HTML File (`multiplication-game.html`)

**HTML Structure:**
- Settings screen with:
  - Max multiplier slider (2-10)
  - Game mode toggle (Result/Multiplier)
  - Tasks per session dropdown
  - Allowed mistakes dropdown
  - Start button
- Game screen with:
  - Problem display (large, readable)
  - Number pad (0-9 buttons + clear)
  - Submit button
  - Progress indicator (e.g., "3/10")
  - Mistakes counter
- Results screen with:
  - Final score and accuracy percentage
  - **Full problem history list**:
    - Each problem shown: `3 × 7 = 21 ✓` or `5 × 8 = 35 ✗ (correct: 40)`
    - Color coded: green for correct, red for wrong
    - Scrollable if many problems
  - Play again button

**Embedded CSS (`<style>` tag):**
- Tablet-optimized (10-inch, 768×1024 viewport)
- Large touch targets (min 48px height)
- **Modern game style (clean but engaging):**
  - Rounded corners on buttons and cards
  - Subtle shadows for depth
  - Smooth transitions/animations
  - Modern color palette (soft blues, greens, warm accents)
  - Gradient backgrounds (subtle, not distracting)
  - Card-style containers
- Large readable fonts (problem: 48px+, buttons: 24px+)
- High contrast for accessibility
- CSS Grid for number pad layout
- Visual feedback: green glow (correct), red shake (wrong)

**Embedded JavaScript (`<script>` tag):**
- Configuration management with defaults
- Problem generation:
  - One multiplier: random from [2, selectedMax]
  - Other multiplier: random from [2, 10]
  - Randomly swap positions (so practiced number appears as A or B)
  - For multiplier mode: randomly hide A or B
- Answer validation
- Score/mistakes tracking
- Session state management (settings → game → results)
- Screen transitions

## UI Wireframe (Text)

```
┌─────────────────────────────────────┐
│           SETTINGS SCREEN            │
│                                      │
│  Max Number: [====●====] 10          │
│                                      │
│  Mode:  [Result ▼]                   │
│                                      │
│  Tasks: [10 ▼]    Mistakes: [3 ▼]   │
│                                      │
│         ┌──────────────┐            │
│         │    START     │            │
│         └──────────────┘            │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│            GAME SCREEN               │
│                                      │
│      Progress: 3/10    ❌ 1/3        │
│                                      │
│           7  ×  8  =  ?              │
│                                      │
│         ┌───────────────┐           │
│         │      56       │           │
│         └───────────────┘           │
│                                      │
│         ┌──────────────┐            │
│         │    CHECK     │            │
│         └──────────────┘            │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│           RESULTS SCREEN             │
│                                      │
│         Score: 8/10 (80%)            │
│                                      │
│  ┌─────────────────────────────┐    │
│  │ ✓  3 × 7 = 21               │    │
│  │ ✓  5 × 4 = 20               │    │
│  │ ✗  6 × 8 = 42 (correct: 48) │    │
│  │ ✓  2 × 9 = 18               │    │
│  │ ...                         │    │
│  └─────────────────────────────┘    │
│                                      │
│         ┌──────────────┐            │
│         │  PLAY AGAIN  │            │
│         └──────────────┘            │
└─────────────────────────────────────┘
```

## Visual Design

**Modern Game Style (straightforward but polished):**
- Clean white/light gray background with subtle gradient
- Card-based UI with soft shadows
- Rounded buttons with hover/press effects
- Color scheme:
  - Primary: Soft blue (#4A90D9) for buttons
  - Success: Green (#4CAF50) for correct answers
  - Error: Red (#E53935) for mistakes
  - Neutral: Gray tones for text
- Animations:
  - Button press effect (scale down slightly)
  - Correct answer: green glow + checkmark
  - Wrong answer: red flash + subtle shake
  - Smooth screen transitions
- Typography: System fonts (clean, fast loading)

## Technical Details

### Number Input Approach
- Use large number buttons (0-9) for tablet-friendly input
- Display entered number in large font
- Clear/backspace button

### Answer Feedback
- Correct: Green flash + brief celebration
- Wrong: Red flash + show correct answer briefly
- Sound effects optional (can be disabled)

### Congratulations Messages (Practice All Mode)
Every 5 correct answers, show an encouraging popup:
- "Great job! Keep going!"
- "You're doing amazing!"
- "Fantastic! 5 more correct!"
- "Super star! Keep it up!"
- "Excellent work!"
- "You're on fire!"
- "Math champion!"
- "Brilliant!"

Message appears briefly (1.5s) then auto-dismisses.

## Verification
1. Open `multiplication-game.html` in browser
2. Test settings: change all options, verify they persist to game
3. Test Result mode: verify random problems, correct answer validation
4. Test Multiplier mode: verify missing factor problems work
5. Test no duplicate pairs: verify A×B and B×A don't both appear
6. Test mistake limit: verify game ends when exceeded
7. Test session completion: verify results screen shows all problems
8. Test on tablet viewport (768×1024 or similar)

## Files to Delete
1. `pom.xml` - Maven config (not needed)
2. `src/` directory - Java source files (not needed)

## Files to Create
1. `multiplication-game.html` - Single self-contained game file (in project root)

**Benefits of single file:**
- Easy to share (email, USB, cloud)
- Works offline
- No server needed - just open in browser
- No dependencies to install
