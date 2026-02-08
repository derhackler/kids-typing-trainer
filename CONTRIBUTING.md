# Contributing

Thank you for your interest in improving the Tipp-Trainer!

## Architecture

The entire application lives in a single `index.html` file. This is intentional — it keeps deployment trivial (just open the file) and avoids build tooling. Please preserve this single-file architecture when contributing.

The file is organized into three embedded sections:

1. **`<style>`** — All CSS, including keyboard colors, screen layouts, and animations
2. **HTML body** — Screen containers (start, tutorial, lesson, result), keyboard markup, overlays
3. **`<script>`** — Application logic: audio, keyboard layout data, lesson definitions, storage, adaptive learning, and the main `app` object

## Guidelines

### Keep it simple

- No frameworks, no build tools, no external dependencies (other than the Giphy API)
- Vanilla JS (ES6+), plain CSS
- The app must work by opening `index.html` directly in a browser

### Audience

The target users are 9-year-old German-speaking children. All UI text must be in German and age-appropriate. Keep the interface simple and encouraging.

### Adding lessons

Lessons are defined in the `defineLessons()` function. Each lesson has:

- `id` — sequential number
- `title` / `description` — shown to the user (German)
- `targetKeys` — array of keys this lesson focuses on
- `sequences` — array of words/sequences to practice
- `duration` — always 180 seconds (3 minutes)
- `minAccuracy` / `minWpm` — success thresholds

To add a new lesson, append to the array and ensure the success criteria match the lesson group (1-5, 6-15, 16-25).

### Testing

Open `index.html` in a browser and manually verify:

- User creation and selection
- Tutorial display for new users
- Typing interaction (correct keys, errors, backspace)
- Keyboard highlighting matches the expected character
- Timer counts down and lesson ends at 3 minutes
- Results screen shows correct stats
- Passed/failed logic works with the threshold values
- GIF loads on success (requires internet)

### Code style

- Use consistent 2-space indentation
- Prefer `const` and `let` over `var`
- Keep functions small and focused
- Comment non-obvious logic, but don't over-document

## Reporting Issues

Please open a GitHub issue with:

- What you expected to happen
- What actually happened
- Browser and OS version
- Steps to reproduce
