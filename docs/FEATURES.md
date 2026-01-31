# Multiplication Game — Feature Specifications

> BDD-style scenarios for all game modes

---

## Feature: Fixed Tasks Mode

```gherkin
Scenario A: Complete fixed session without mistakes
  Given session type is "fixed"
    And total tasks is 10
  When user answers all 10 problems correctly
  Then game ends with success
    And results show "10 / 10 mastered"
    And mistakes shown is 0

Scenario B: Wrong answer repeats until correct
  Given session type is "fixed"
    And total tasks is 5
    And problem queue is [Q1, Q2, Q3, Q4, Q5]
  When user answers Q1 incorrectly
  Then show correct answer for 1 second
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
  When user answers all 20 problems correctly
  Then results show "20 / 20 mastered"
    And message is "Practice All Tables mastered in {time}"

Scenario C: Wrong answers repeat in practice-all
  Given session type is "practice-all"
    And current problem is 3 x 7 = 21
  When user answers 18 (incorrect)
  Then display shows "~~18~~ -> 21"
    And voice says "The answer is twenty-one"
    And problem is requeued
```

---

## Feature: Practice One Table Mode

```gherkin
Scenario A: Practice single table
  Given session type is "practice-one"
    And practice number is 7
  Then problems are [7x1, 7x2, ..., 7x10]
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
    And voice says "seven, times eight"
    And expected answer is 56

Scenario B: Multiplier mode (find factor A)
  Given game mode is "multiplier"
    And problem is 7 x 8 with hidden A
  Then display shows "? x 8 = 56"
    And voice says "What number, times eight, equals fifty-six"
    And expected answer is 7

Scenario C: Multiplier mode (find factor B)
  Given game mode is "multiplier"
    And problem is 7 x 8 with hidden B
  Then display shows "7 x ? = 56"
    And voice says "Seven, times what number, equals fifty-six"
    And expected answer is 8
```

---

## Feature: Auto-Submit

```gherkin
Scenario A: Correct answer auto-submits immediately
  Given current problem answer is 56
  When user types "5"
  Then answer display shows "5"
    And auto-submit is false
  When user types "6"
  Then answer display shows "56"
    And auto-submit triggers immediately
    And display shows "56 ✓"
    And voice says "fifty-six, correct!"
    And wait 500ms
    And proceed to next problem

Scenario B: Wrong answer auto-submits after 2 seconds (anti-cheat)
  Given current problem answer is 56
  When user types "48"
  Then answer display shows "48"
    And auto-submit timer starts (2 seconds)
  When 2 seconds pass without input
  Then auto-submit triggers
    And display shows "~~48~~ -> 56"
    And voice says "The answer is fifty-six"
    And wait 1000ms
    And problem is requeued
    And proceed to next problem

Scenario C: User can click CHECK before auto-submit
  Given current problem answer is 56
    And user typed "48"
    And auto-submit timer is running
  When user clicks CHECK button
  Then timer is cancelled
    And answer is submitted immediately
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
  When user navigates through all screens
  Then all text is in Ukrainian
```

---

## Feature: Voice

```gherkin
Scenario A: Voice toggle in header
  Given game screen is displayed
  Then voice toggle button is in progress bar (top right)
    And shows 🔊 when enabled
    And shows 🔇 when disabled

Scenario B: Toggle voice off
  Given voice is enabled
  When user clicks voice toggle
  Then voice is disabled
    And toggle shows 🔇
    And speech is cancelled

Scenario C: Voice pronunciation
  Given voice is enabled
  When problem is displayed
  Then voice reads the problem
  When user answers correctly
  Then voice says "{answer}, correct!"
  When user answers incorrectly
  Then voice says "The answer is {answer}"

Scenario D: Ukrainian voice not available
  Given language is "Ukrainian"
    And device has no Ukrainian TTS
  Then popup shows installation instructions
    And user can dismiss with OK button
```

---

## Feature: Voice Input (Speech Recognition)

```gherkin
Scenario A: Speak answer instead of typing
  Given game screen is displayed
    And speech recognition is supported
  When user clicks microphone button 🎤
  Then microphone button shows "listening" state (red pulsing)
    And device starts listening for speech
  When user says "fifty-six"
  Then speech is converted to "56"
    And answer is auto-submitted

Scenario B: Voice input in Ukrainian
  Given language is "Ukrainian"
  When user clicks microphone button
    And user says "п'ятдесят шість"
  Then speech is converted to "56"
    And answer is submitted

Scenario C: Speech recognition not supported
  Given device does not support speech recognition
  Then microphone button is disabled (grayed out)
    And user must use numpad

Scenario D: Voice input with numerals
  Given user clicks microphone button
  When user says "56" (as numerals)
  Then answer is recognized as 56
    And answer is submitted

Scenario E: Auto-start microphone after problem is read
  Given speech recognition is supported
    And voice is enabled
  When a new problem is displayed
  Then voice reads the problem aloud
    And after 500ms delay
    And microphone automatically starts listening
    And user can speak their answer immediately
```

---

## Feature: Button Design (UX Standards)

```gherkin
Scenario A: Primary action buttons are green
  Given user is on any screen
  Then START button is green
    And CHECK button is green
    And PLAY AGAIN button is green

Scenario B: Secondary action buttons are blue
  Given user is on menu screen
  Then PLAY button is blue
    And LEARN button is blue

Scenario C: Navigation uses text links
  Given user is on any screen with back option
  Then "Back" is a subtle text link (not a button)
    And "Finish" is a subtle text link
    And "Back to Menu" is a subtle text link

Scenario D: CHECK is the only prominent game button
  Given user is on game screen
  Then CHECK button is large and green
    And "Finish" is small text link below
    And voice toggle is in header area
```

---

## Feature: Progress Display

```gherkin
Scenario A: Show mastered count and remaining
  Given total problems is 40
    And mastered count is 15
    And queue length is 28
    And mistakes is 3
  Then progress shows "Mastered: 15 / 40"
    And status shows "Mistakes: 3 | Left: 28"

Scenario B: Voice toggle in progress bar
  Given game screen is displayed
  Then progress bar contains:
    | Mastered count | Timer | Status | Voice toggle |
```

---

## Feature: Results Screen

```gherkin
Scenario A: Perfect score
  Given all answers correct on first try
  Then message shows "Perfect! No mistakes!"
    And PLAY AGAIN button is green

Scenario B: Completed with mistakes
  Given mistakes is 5
  Then message shows "Completed with 5 mistakes corrected!"
    And mistake list shows each wrong answer
```

---

## Feature: Timer

```gherkin
Scenario A: Timer runs during game
  Given game has started
  When 65 seconds have elapsed
  Then timer shows "01:05"

Scenario B: Timer stops on game end
  When all problems are mastered
  Then timer stops
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
    And auto-dismiss after 1.5 seconds

Scenario B: Streak resets on wrong answer
  Given streak is 4
  When user answers incorrectly
  Then streak becomes 0
```

---

## Feature: Finish Game

```gherkin
Scenario A: Exit game with text link
  Given game is in progress
  When user clicks "Finish" text link
  Then timer stops
    And speech is cancelled
    And user returns to menu
```

---

## Feature: Learn Mode

```gherkin
Scenario A: View multiplication table
  Given screen is "learn"
  When user clicks number 7
  Then table shows 7x1 through 7x10
    And button 7 is highlighted
```

---

## Timing Configuration

| Event | Delay |
|-------|-------|
| Correct answer delay | 500ms |
| Wrong answer delay | 1000ms |
| Auto-submit (anti-cheat) | 2000ms |
| Congrats popup | 1500ms |

---

## Test Matrix

| Feature | Tested |
|---------|--------|
| Auto-submit correct (immediate) | [ ] |
| Auto-submit wrong (2 sec) | [ ] |
| Voice toggle in header | [ ] |
| Voice input (microphone) | [ ] |
| Green primary buttons | [ ] |
| Text link navigation | [ ] |
| Language switching | [ ] |
| All session types | [ ] |

---

*All scenarios should pass before release*
