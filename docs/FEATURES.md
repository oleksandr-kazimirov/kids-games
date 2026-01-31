# Multiplication Game — Feature Specifications

> BDD-style scenarios for all game modes

---

## Feature: Fixed Tasks Mode

```gherkin
Scenario: Complete fixed session without mistakes
  Given session_type = "fixed"
  And total_tasks = 10
  And max_mistakes = 3
  When user answers all 10 problems correctly
  Then game ends with success
  And results show "10 / 10 mastered"
  And results show "0 mistakes"

Scenario: Wrong answer repeats until correct
  Given session_type = "fixed"
  And total_tasks = 5
  And problem queue = [Q1, Q2, Q3, Q4, Q5]
  When user answers Q1 incorrectly
  Then show correct answer for 5 seconds
  And Q1 is re-queued at position 3-7
  And queue becomes [Q2, Q3, Q4, Q1, Q5] (example)
  When user later answers Q1 correctly
  Then Q1 is marked as mastered
  And game continues until queue is empty

Scenario: Game ends only when all mastered
  Given session_type = "fixed"
  And total_tasks = 5
  And user makes 10 mistakes on various problems
  Then game does NOT end (no fail state)
  When all 5 problems are eventually answered correctly
  Then game ends with success
  And results show "5 / 5 mastered"
  And results show "10 mistakes corrected"
```

---

## Feature: Practice All Tables Mode

```gherkin
Scenario: Practice all tables from 2 to max
  Given session_type = "practice-all"
  And max_number = 5
  Then generate problems for tables 2, 3, 4, 5
  And each table has 10 problems (×1 through ×10)
  And total problems = 4 × 10 = 40

Scenario: Complete all combinations
  Given session_type = "practice-all"
  And max_number = 3
  And problems = [2×1, 2×2, ..., 2×10, 3×1, 3×2, ..., 3×10]
  When user answers all 20 problems correctly
  Then results show "20 / 20 mastered"
  And results show "All tables mastered in {time}"

Scenario: Wrong answers repeat in practice-all
  Given session_type = "practice-all"
  And current_problem = "3 × 7"
  When user answers "18" (incorrect)
  Then show "~~18~~ → 21" for 5 seconds
  And voice says "The answer is 21"
  And "3 × 7" is added back to queue
  When "3 × 7" appears again later
  And user answers "21" (correct)
  Then "3 × 7" is marked as mastered
```

---

## Feature: Practice One Table Mode

```gherkin
Scenario: Practice single table
  Given session_type = "practice-one"
  And practice_number = 7
  Then generate problems: 7×1, 7×2, 7×3, ..., 7×10
  And total problems = 10

Scenario: Master single table
  Given session_type = "practice-one"
  And practice_number = 5
  When user answers all 10 problems correctly
  Then results show "Table of 5 mastered in {time}"
  And results show "10 / 10"
```

---

## Feature: Question Types

```gherkin
Scenario: Result mode (find the answer)
  Given game_mode = "result"
  And problem = { a: 7, b: 8 }
  Then display "7 × 8 = ?"
  And voice says "7 times 8"
  And expected_answer = 56

Scenario: Multiplier mode (find the factor)
  Given game_mode = "multiplier"
  And problem = { a: 7, b: 8, hidden: "a" }
  Then display "? × 8 = 56"
  And voice says "What times 8 equals 56"
  And expected_answer = 7

Scenario: Multiplier mode - other factor hidden
  Given game_mode = "multiplier"
  And problem = { a: 7, b: 8, hidden: "b" }
  Then display "7 × ? = 56"
  And voice says "7 times what equals 56"
  And expected_answer = 8
```

---

## Feature: Auto-Submit on Correct Answer

```gherkin
Scenario: Correct answer auto-submits
  Given current_problem.answer = 56
  When user types "5"
  Then answer_display shows "5"
  And no auto-submit (5 ≠ 56)
  When user types "6"
  Then answer_display shows "56"
  And auto-submit triggers (56 === 56)
  And show "56 ✓" with green checkmark
  And voice says "Correct!"

Scenario: Wrong answer requires CHECK button
  Given current_problem.answer = 56
  When user types "48"
  Then answer_display shows "48"
  And no auto-submit (48 ≠ 56)
  When user clicks CHECK button
  Then show "~~48~~ → 56" for 5 seconds
  And voice says "The answer is 56"
```

---

## Feature: Voice (Optional)

```gherkin
Scenario: Voice disabled by default
  Given checkbox_voice.checked = false
  When problem is displayed
  Then no speech occurs

Scenario: Voice enabled
  Given checkbox_voice.checked = true
  When problem "7 × 8 = ?" is displayed
  Then voice says "7 times 8"
  When user answers correctly
  Then voice says "Correct!"
  When user answers incorrectly
  Then voice says "The answer is 56"

Scenario: Voice works offline
  Given voice_enabled = true
  And device is offline
  When problem is displayed
  Then Web Speech API uses device's built-in TTS
  And voice still works
```

---

## Feature: Progress Display

```gherkin
Scenario: Show mastered count and remaining
  Given total_problems = 40
  And mastered_count = 15
  And queue_length = 28 (includes retries)
  And mistakes = 3
  Then progress shows "Mastered: 15 / 40"
  And status shows "Mistakes: 3 | Left: 28"

Scenario: No mistakes yet
  Given mistakes = 0
  And queue_length = 35
  Then status shows "Left: 35"
  And status color = blue (remaining)
```

---

## Feature: Results Screen

```gherkin
Scenario: Perfect score (no mistakes)
  Given all problems answered correctly on first try
  Then results show "Perfect! No mistakes!"
  And mistake list shows "No mistakes! Perfect!"
  And summary background = green

Scenario: Completed with mistakes
  Given mistakes = 5
  And all problems eventually mastered
  Then results show "Completed with 5 mistakes corrected!"
  And mistake list shows each mistake:
    | Problem     | Display                    |
    | 3 × 5 = 15  | 3 × 5 = ~~22~~ → 15  ✗    |
    | 9 × 3 = 27  | 9 × 3 = ~~18~~ → 27  ✗    |
```

---

## Feature: Timer

```gherkin
Scenario: Timer runs during game
  Given game starts at 00:00
  When 65 seconds pass
  Then timer shows "01:05"
  When game ends
  Then results show "mastered in 01:05"

Scenario: Timer stops on game end
  Given timer is running
  When all problems are mastered
  Then timer stops
  And elapsed_time is saved
  And displayed in results
```

---

## Feature: Congratulations Popup

```gherkin
Scenario: Congrats every 5 correct streak
  Given streak = 4
  When user answers correctly
  Then streak = 5
  And congrats popup appears
  And shows random emoji + message (e.g., "🌟 Great job!")
  And shows "5 correct in a row!"
  And popup auto-dismisses after 1.5 seconds

Scenario: Streak resets on wrong answer
  Given streak = 4
  When user answers incorrectly
  Then streak = 0
  And no congrats popup
```

---

## Feature: Learn Mode

```gherkin
Scenario: View multiplication table
  Given user is on learn screen
  When user clicks number button "7"
  Then display multiplication table:
    | 7 × 1  = 7  |
    | 7 × 2  = 14 |
    | 7 × 3  = 21 |
    | ...         |
    | 7 × 10 = 70 |
  And button "7" is highlighted as selected
```

---

## Test Matrix

| Mode | Tasks | Retry | Auto-Submit | Voice | Timer |
|------|-------|-------|-------------|-------|-------|
| Fixed | User-defined | ✅ | ✅ | Optional | ✅ |
| Practice All | (max-1) × 10 | ✅ | ✅ | Optional | ✅ |
| Practice One | 10 | ✅ | ✅ | Optional | ✅ |

---

*All scenarios should pass before release* ✅
