# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- [x] **Describe the game's purpose.**
  A simple number-guessing game built with Streamlit. The app picks a secret number within a range that depends on the chosen difficulty (Easy 1–20, Normal 1–100, Hard 1–50), and the player tries to guess it within a limited number of attempts. After each guess the game gives a "Go HIGHER" / "Go LOWER" hint, tracks a score, and ends on a correct guess or when attempts run out.

- [x] **Detail which bugs you found.**
  - **State bug:** the secret number was regenerated on every "Submit," so the game was unwinnable.
  - **Backwards/inconsistent hints:** the Higher/Lower feedback didn't match the secret, and the secret's type was being switched every other attempt, which made hints flip-flop.
  - **New Game didn't fully reset:** it refreshed the secret but left the old history list (and could leave the game stuck in a non-playing state).
  - **Difficulty restrictions ignored:** switching difficulty didn't reset the range/attempt limit, so the secret and guesses could fall outside the new difficulty's rules.
  - **Score behaved oddly** due to the type-switching logic and inconsistent attempt handling.

- [x] **Explain what fixes you applied.**
  - Persisted the secret in `st.session_state` so it stays fixed across reruns and only changes on a new game or difficulty change.
  - Corrected `check_guess` so the hints honestly reflect whether the guess is above or below the secret, and removed the every-2-attempts type-switching that corrupted comparisons and scoring.
  - Added a single `reset_game(difficulty)` helper that clears attempts and history, picks a fresh secret in the correct range, and sets the status back to "playing" — used by both the **New Game** button and the difficulty-change reset so they can't drift apart.
  - Made a difficulty change auto-reset the game to that difficulty's range and attempt limit.
  - Refactored the core logic (`get_range_for_difficulty`, `parse_guess`, `check_guess`, `update_score`) into `logic_utils.py` and verified the behavior with `pytest`.

## 📸 Demo Walkthrough
*(Sample game on Normal difficulty, range 1–100, secret = 42.)*
1. User enters a guess of 70 → hint reads "📉 Go LOWER!"
2. User enters a guess of 25 → hint reads "📈 Go HIGHER!"
3. The secret stays fixed across guesses, so each hint narrows the range honestly
4. Score updates after each guess and history records every attempt
5. User guesses 42 → "🎉 Correct!", balloons, and the game locks until a new game is started

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```
# Paste your pytest output here, e.g.:
# pytest tests/
# ========================= X passed in 0.XXs =========================
```

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]
