# Multiplication Game — Feature Specifications

> BDD-style scenarios for all game modes

---

## Feature: Fixed Tasks Mode

```gherkin
Scenario A: Complete fixed session without mistakes
  Given session type is "fixed"
    And total tasks is 10
    And max mistakes is 3
  When user answers all 10 problems correctly
  Then game ends with success
    And results show "10 / 10 mastered"
    And mistakes shown is 0

Scenario B: Wrong answer repeats until correct
  Given session type is "fixed"
    And total tasks is 5
    And problem queue is [Q1, Q2, Q3, Q4, Q5]
  When user answers Q1 incorrectly
  Then show correct answer for 5 seconds
    And queue becomes [Q2, Q3, Q4, Q1, Q5]
  When user later answers Q1 correctly
  Then Q1 is mastered
    And game continues

Scenario C: Game ends only when all mastered
  Given session type is "fixed"
    And total tasks is 5
    And user makes 10 mistakes
  Then game over is false (no fail state!)
  When all 5 problems eventually answered correctly
  Then game over is true
    And success is true
    And results show "5 / 5 mastered"
    And message is "Completed with 10 mistakes corrected!"
```

---

## Feature: Practice All Tables Mode

```gherkin
Scenario A: Practice all tables from 2 to max
  Given session type is "practice-all"
    And max number is 5
  Then tables generated are [2, 3, 4, 5]
    And problems per table is 10 (x1 through x10)
    And total problems is 40

Scenario B: Complete all combinations
  Given session type is "practice-all"
    And max number is 3
    And problems are [2x1, 2x2, ..., 2x10, 3x1, 3x2, ..., 3x10]
  When user answers all 20 problems correctly
  Then results show "20 / 20 mastered"
    And message is "All tables mastered in {time}"

Scenario C: Wrong answers repeat in practice-all
  Given session type is "practice-all"
    And current problem is 3 x 7 = 21
  When user answers 18 (incorrect)
  Then display shows "~~18~~ -> 21"
    And display duration is 5 seconds
    And voice says "The answer is twenty-one"
    And problem is requeued
  When problem 3x7 appears again
    And user answers 21 (correct)
  Then problem is mastered
    And display shows "21 checkmark"
```

---

## Feature: Practice One Table Mode

```gherkin
Scenario A: Practice single table
  Given session type is "practice-one"
    And practice number is 7
  Then problems are [7x1, 7x2, 7x3, 7x4, 7x5, 7x6, 7x7, 7x8, 7x9, 7x10]
    And total problems is 10

Scenario B: Master single table
  Given session type is "practice-one"
    And practice number is 5
  When user answers all 10 problems correctly
  Then results show "10 / 10"
    And message is "Table of 5 mastered in {time}"
```

---

## Feature: Question Types

```gherkin
Scenario A: Result mode (find the answer)
  Given game mode is "result"
    And problem is 7 x 8
  Then display shows "7 x 8 = ?"
    And voice says "seven times eight"
    And expected answer is 56

Scenario B: Multiplier mode (find factor A)
  Given game mode is "multiplier"
    And problem is 7 x 8 with hidden A
  Then display shows "? x 8 = 56"
    And voice says "What times eight equals fifty-six"
    And expected answer is 7

Scenario C: Multiplier mode (find factor B)
  Given game mode is "multiplier"
    And problem is 7 x 8 with hidden B
  Then display shows "7 x ? = 56"
    And voice says "Seven times what equals fifty-six"
    And expected answer is 8
```

---

## Feature: Auto-Submit on Correct Answer

```gherkin
Scenario A: Correct answer auto-submits
  Given current problem answer is 56
  When user types "5"
  Then answer display shows "5"
    And auto-submit is false (5 != 56)
  When user types "6"
  Then answer display shows "56"
    And auto-submit is true (56 === 56)
    And display shows "56 checkmark"
    And voice says "fifty-six, correct!"

Scenario B: Wrong answer requires CHECK button
  Given current problem answer is 56
  When user types "48"
  Then answer display shows "48"
    And auto-submit is false (48 != 56)
  When user clicks CHECK button
  Then display shows "~~48~~ -> 56"
    And display duration is 5 seconds
    And voice says "The answer is fifty-six"
```

---

## Feature: Language Selection

```gherkin
Scenario A: Select language on menu
  Given user is on menu screen
  When user clicks "English" button
  Then UI language is English
    And voice language is English
    And English button is highlighted

Scenario B: Switch to Ukrainian
  Given user is on menu screen
  When user clicks "Українська" button
  Then UI language is Ukrainian
    And all buttons show Ukrainian text
    And voice pronounces in Ukrainian

Scenario C: Language persists through screens
  Given user selected Ukrainian
  When user navigates to Settings
  Then all labels are in Ukrainian
  When user starts game
  Then progress text is in Ukrainian
  When user finishes game
  Then results are in Ukrainian
```

---

## Feature: Voice

```gherkin
Scenario A: Voice enabled by default
  Given game starts
  Then voice is enabled
    And voice toggle shows 🔊

Scenario B: Toggle voice off
  Given game is in progress
    And voice is enabled
  When user clicks voice toggle button
  Then voice is disabled
    And voice toggle shows 🔇
    And speech is cancelled

Scenario C: Voice in English
  Given language is "English"
    And voice is enabled
  When problem "7 x 8 = ?" is displayed
  Then voice says "seven, times eight"
  When user answers correct (56)
  Then voice says "fifty-six, correct!"
    And waits for speech to finish
    And then proceeds to next problem
  When user answers incorrect
  Then voice says "The answer is fifty-six"
    And waits for speech to finish
    And auto-proceeds after 3 seconds

Scenario D: Voice in Ukrainian
  Given language is "Ukrainian"
    And voice is enabled
    And device has Ukrainian TTS installed
  When problem "7 x 8 = ?" is displayed
  Then voice says "sim na visim"
  When user answers correct (56)
  Then voice says "piatdesiat shist, pravylno!"

Scenario E: Ukrainian voice not available
  Given language is "Ukrainian"
    And device has no Ukrainian TTS
  Then popup shows "Ukrainian Voice Not Found"
    And shows installation instructions for:
      | Platform | Instructions |
      | Android  | Settings → System → Languages → TTS |
      | iOS      | Settings → Accessibility → Voices |
      | Windows  | Settings → Time & Language → Speech |
    And user can close popup with OK button
```

---

## Feature: Progress Display

```gherkin
Scenario A: Show mastered count and remaining
  Given total problems is 40
    And mastered count is 15
    And queue length is 28 (includes retries)
    And mistakes is 3
  Then progress text shows "Mastered: 15 / 40"
    And status text shows "Mistakes: 3 | Left: 28"

Scenario B: No mistakes yet
  Given mistakes is 0
    And queue length is 35
  Then status text shows "Left: 35"
    And status color is blue
```

---

## Feature: Results Screen

```gherkin
Scenario A: Perfect score (no mistakes)
  Given all answers correct on first try
    And mistakes is 0
  Then message shows "Perfect! No mistakes!"
    And mistake list shows "No mistakes! Perfect!"
    And summary style is success (green)

Scenario B: Completed with mistakes
  Given mistakes is 5
    And all problems mastered
    And mistake history includes:
      | problem | user answer | correct |
      | 3 x 5   | 22          | 15      |
      | 9 x 3   | 18          | 27      |
  Then message shows "Completed with 5 mistakes corrected!"
    And mistake list shows:
      | "3 x 5 = ~~22~~ -> 15  X" |
      | "9 x 3 = ~~18~~ -> 27  X" |
```

---

## Feature: Timer

```gherkin
Scenario A: Timer runs during game
  Given game has started
  When 65 seconds have elapsed
  Then timer display shows "01:05"
  When game ends
  Then results show "mastered in 01:05"

Scenario B: Timer stops on game end
  Given timer is running
  When all problems are mastered
  Then timer is stopped
    And elapsed time is saved
    And time is displayed in results
```

---

## Feature: Congratulations Popup

```gherkin
Scenario A: Congrats every 5 correct streak
  Given streak is 4
  When user answers correctly
  Then streak becomes 5
    And congrats popup appears
    And emoji is random (star, etc.)
    And message is random ("Great job!", etc.)
    And subtext shows "5 correct in a row!"
    And auto-dismiss after 1.5 seconds

Scenario B: Streak resets on wrong answer
  Given streak is 4
  When user answers incorrectly
  Then streak becomes 0
    And congrats popup does not appear
```

---

## Feature: Finish Game Early

```gherkin
Scenario A: Exit game before completion
  Given game is in progress
    And timer is running
  When user clicks FINISH button
  Then timer stops
    And speech is cancelled
    And user returns to menu screen
    And no results are saved
```

---

## Feature: Learn Mode

```gherkin
Scenario A: View multiplication table
  Given screen is "learn"
  When user clicks number button 7
  Then table is displayed:
    | 7 x 1  = 7  |
    | 7 x 2  = 14 |
    | 7 x 3  = 21 |
    | 7 x 4  = 28 |
    | 7 x 5  = 35 |
    | 7 x 6  = 42 |
    | 7 x 7  = 49 |
    | 7 x 8  = 56 |
    | 7 x 9  = 63 |
    | 7 x 10 = 70 |
    And button 7 is highlighted
```

---

## Test Matrix

| Mode | Tasks | Retry | Auto-Submit | Voice | Timer | Congrats | Language |
|------|-------|-------|-------------|-------|-------|----------|----------|
| Fixed | User-defined | Yes | Yes | Always | Yes | Yes | EN/UK |
| Practice All | (max-1) x 10 | Yes | Yes | Always | Yes | Yes | EN/UK |
| Practice One | 10 | Yes | Yes | Always | Yes | Yes | EN/UK |

---

*All scenarios should pass before release*
