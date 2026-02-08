# Tipp-Trainer - 10-Finger Typing Trainer for Children

A gamified typing trainer that teaches 9-year-old children the 10-finger system on a German QWERTZ keyboard layout. The entire application runs as a single HTML file with no server or build process required.

## Features

- **25 progressive lessons** — from home row basics to numbers and punctuation
- **German QWERTZ keyboard** — accurate layout with visual finger-color mapping
- **Adaptive learning** — tracks error patterns and adjusts future lessons to focus on weak keys
- **Multiple user profiles** — each child gets independent progress tracking via localStorage
- **Gamified rewards** — confetti animations, cute animal GIFs (via Giphy API), and sound effects
- **Audio feedback** — Web Audio API tones for correct/incorrect keystrokes and lesson completion
- **Child-friendly German UI** — age-appropriate language, animal theme, large fonts

## Getting Started

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, or Edge)
- A physical keyboard (German QWERTZ layout recommended)

### Running the App

Simply open `index.html` in your browser:

```bash
# Option 1: Open directly
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows

# Option 2: Use a local server (optional)
python3 -m http.server 8000
# Then visit http://localhost:8000
```

No installation, no dependencies, no build step.

## How It Works

### Lesson Progression

| Lessons | Focus | Accuracy | WPM |
|---------|-------|----------|-----|
| 1–5 | Home row (ASDF JKLÖ + GH) | ≥ 90% | ≥ 8 |
| 6–10 | Upper row (QWERTZUIOPÜ) | ≥ 92% | ≥ 10 |
| 11–15 | Lower row (YXCVBNM) | ≥ 92% | ≥ 10 |
| 16–20 | Capital letters (Shift key) | ≥ 94% | ≥ 12 |
| 21–25 | Numbers and punctuation | ≥ 94% | ≥ 12 |

### Keyboard Color Coding

Each finger is assigned a color on the visual keyboard:

| Finger | Color | Keys (home row) |
|--------|-------|-----------------|
| Left pinky | Pink | A |
| Left ring | Purple | S |
| Left middle | Blue | D |
| Left index | Green | F, G |
| Right index | Yellow | H, J |
| Right middle | Orange | K |
| Right ring | Red | L |
| Right pinky | Brown | Ö, Ä |
| Thumbs | Gray | Space |

### Adaptive Learning

The system tracks which keys cause the most errors over the last 5 completed lessons. Future lesson content is automatically biased toward those problematic keys, giving the child more practice where they need it most.

### Error Correction

Typing errors block progression — the child must press Backspace to correct mistakes before continuing. This encourages accuracy over speed.

## Data Storage

All data is stored locally in the browser via `localStorage`:

- `typingTrainer_users` — array of user profiles with progress data
- `typingTrainer_currentUser` — ID of the currently selected user

No data is sent to any server. Clearing browser data will reset all progress.

## Technology

- **HTML/CSS/JS** — single-file, zero dependencies
- **Web Audio API** — synthesized sound effects (no audio files)
- **Giphy API** — random animal GIFs as rewards (falls back to emoji if offline)
- **localStorage** — persistent user data across sessions

## Project Structure

```
kids-typing-trainer/
├── index.html              # Complete application (HTML + CSS + JS)
├── docs/
│   └── SPECIFICATION.md    # Detailed project specification
├── README.md
├── LICENSE
├── CONTRIBUTING.md
└── .gitignore
```

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
