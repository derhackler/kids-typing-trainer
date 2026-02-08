# 10-Finger Typing Trainer for Children - Project Specification

## Project Overview

A gamified typing trainer for 9-year-old children to learn the 10-finger system on a German keyboard layout. The entire application must be a single HTML file with no server requirements.

## Target Audience

- Primary users: 9-year-old children
- German-speaking
- Learning German QWERTZ keyboard layout

## Technical Requirements

### Architecture

- **Single HTML file** containing all HTML, CSS, and JavaScript
- Vanilla JavaScript (ES6+), no frameworks
- LocalStorage API for data persistence
- Giphy API for reward GIFs
- Modern browsers only (Chrome, Firefox, Safari, Edge - latest versions)
- No mobile/touch support required (physical keyboard needed)

### Data Storage

All data stored in browser's LocalStorage:

- User profiles
- Progress data per user
- Lesson completion statistics
- Error patterns for adaptive learning

## Functional Requirements

### 1. User Management

- Multiple user profiles supported
- User selection screen on app start
- Create new user with name input
- Each user has independent progress tracking
- No user deletion required

### 2. Success Metrics (Progressive Difficulty)

Lessons have progressive success criteria:

**Lessons 1-5:**

- Accuracy: ≥90%
- WPM (Words Per Minute): ≥8

**Lessons 6-15:**

- Accuracy: ≥92%
- WPM: ≥10

**Lessons 16-25:**

- Accuracy: ≥94%
- WPM: ≥12

### 3. Error Handling

- **Immediate correction required**: User cannot continue typing until error is corrected (blocking behavior)
- Backspace deletes last character
- Visual feedback for correct/incorrect input
- Sound feedback for success and errors

### 4. Adaptive Learning System

- System tracks which keys/sequences cause most errors
- Next lesson is automatically selected based on:
  - If previous lesson passed: new lesson or repetition based on error patterns
  - If previous lesson failed: repeat same lesson
  - Adapt lesson content to focus on problematic key combinations
- No manual lesson selection or repetition by user

### 5. Lesson Structure

- **Total lessons:** 25
- **Duration:** Maximum 3 minutes of continuous typing per lesson
- **Content:** Time-based (user types for up to 3 minutes, not a fixed text)
- **Language:** German words and common German letter sequences

**Lesson Progression:**

- Lessons 1-5: Home row (asdf jklö) + simple combinations
- Lessons 6-10: + upper row (common letters: e, n, r, t, u, i, o, p, ü)
- Lessons 11-15: + lower row (y, x, c, v, b, n, m)
- Lessons 16-20: + Shift key + capital letters
- Lessons 21-25: + numbers and special characters

**German letter sequences to practice:**

- Common combinations: en, er, ch, sch, ei, ie, st, nd, ge, te, de, es, in, an, un
- Common words should be age-appropriate for 9-year-olds

### 6. Tutorial/Onboarding

- Simple first-time tutorial showing:
  - Finger placement on home row (asdf jklö)
  - Which fingers control which keys
  - Basic explanation kept very simple
- Shown only once per new user

### 7. Keyboard Visualization

- Visual keyboard displayed during lessons
- Color-coded finger assignment:
  - Left pinky: a, q, y, 1, <, ^
  - Left ring: s, w, x, 2
  - Left middle: d, e, c, 3
  - Left index: f, r, v, 4, g, t, b, 5
  - Right index: h, z, n, 6, j, u, m, 7
  - Right middle: k, i, ,, 8
  - Right ring: l, o, ., 9
  - Right pinky: ö, p, ü, 0, -, ä, ß, etc.
  - Thumbs: Space
- Highlight current key to press
- Highlight corresponding finger

### 8. Reward System

- After successfully completing a lesson: Show animal GIF from Giphy API
- GIF categories: cute animals, funny animals, baby animals
- Success sound plays
- Display lesson statistics (accuracy, WPM)
- Automatic progression to next lesson selection (no manual choice)

### 9. Audio Feedback

- Success sound: pleasant/positive tone when correct key pressed
- Error sound: gentle/neutral sound when wrong key pressed
- Not toggleable (always on)
- Use Web Audio API or simple HTML5 audio

### 10. UI/UX Requirements

**Theme:** Animals

- Colorful, playful design appealing to 9-year-olds
- Animal illustrations/icons throughout
- Friendly, encouraging language
- Large, readable fonts
- Simple navigation

**Language:** Complete German interface

- All text in German
- Age-appropriate language (for 9-year-olds)

**Screens:**

1. **Start Screen**
- User selection/creation
- Simple, colorful design with animal theme
1. **Lesson Screen**
- Keyboard visualization (top or center)
- Text to type (clear, large font)
- Live stats: current accuracy, WPM, time remaining
- Visual feedback for correct/incorrect typing
- Timer (counts up to 3 minutes)
1. **Success Screen**
- Celebration message
- Lesson statistics
- Animal GIF from Giphy
- Automatic progression after 5-10 seconds (or "Continue" button)
1. **Tutorial Screen**
- Simple finger position explanation
- Visual keyboard with finger colors
- Start practice button

## Data Models

### User Object

```javascript
{
  id: string (UUID),
  name: string,
  createdAt: timestamp,
  currentLesson: number,
  completedLessons: [
    {
      lessonId: number,
      accuracy: number,
      wpm: number,
      timestamp: timestamp,
      errorPatterns: {
        'key': count, // e.g., 'k': 5, 'l': 3
      }
    }
  ],
  tutorialCompleted: boolean
}
```

### Lesson Object

```javascript
{
  id: number,
  title: string, // e.g., "Grundreihe 1"
  description: string,
  targetKeys: [string], // keys to practice
  sequences: [string], // words/sequences to type
  duration: 180, // seconds (3 minutes)
  minAccuracy: number,
  minWpm: number,
  level: number (1-5, 6-15, 16-25)
}
```

## Technical Implementation Details

### Lesson Content Generation

- Pre-defined 25 lesson templates
- Adaptive layer: Based on error patterns, inject more practice for problematic keys
- Generate random but weighted sequences focusing on weak areas

### Statistics Calculation

- **WPM:** (characters typed / 5) / (time in minutes)
  - Standard: 1 word = 5 characters
- **Accuracy:** (correct characters / total characters attempted) * 100
- Track in real-time during lesson

### Giphy API Integration

- API Key: Free tier (no key required for basic usage)
- Endpoint: `https://api.giphy.com/v1/gifs/random`
- Parameters: `api_key=`, `tag=cute+animals` or `tag=funny+animals`
- Display random animal GIF on success

### LocalStorage Structure

```javascript
localStorage.setItem('typingTrainer_users', JSON.stringify([user1, user2, ...]));
localStorage.setItem('typingTrainer_currentUser', userId);
```

## UI Flow

1. **App Start**
- Load users from LocalStorage
- Show user selection screen
- "New User" button
1. **User Selection**
- Click user → Check if tutorial completed
- If not: Show tutorial
- If yes: Determine next lesson (adaptive algorithm)
1. **Tutorial** (first time only)
- Show finger position guide
- Simple explanation
- "Start Training" button → First lesson
1. **Lesson Flow**
- Show lesson title/description briefly (2 seconds)
- Start lesson:
  - Timer starts
  - User types displayed text
  - Real-time feedback (visual + audio)
  - Immediate error correction required
- After 3 minutes OR if user completes enough sequences:
  - Calculate statistics
  - Check if passed (accuracy + WPM thresholds)
1. **Success Screen**
- If passed: Show celebration + GIF + stats
- If failed: Show encouragement + stats
- Save results to LocalStorage
- Automatically select next lesson (after delay or button click)
1. **Next Lesson Selection (Adaptive Algorithm)**
- If failed: Repeat same lesson
- If passed but with many errors in specific keys:
  - Generate adapted version of next lesson focusing on those keys
- If passed well: Continue to next new lesson

## Design Guidelines

### Color Scheme (Animal Theme)

- Primary: Warm, friendly colors (orange, yellow, green)
- Accent: Bright, playful colors
- Keyboard finger colors: Rainbow scheme (distinguishable)
  - Left pinky: Pink
  - Left ring: Purple
  - Left middle: Blue
  - Left index: Green
  - Right index: Yellow
  - Right middle: Orange
  - Right ring: Red
  - Right pinky: Brown
  - Thumbs: Gray

### Typography

- Large, sans-serif font for typing text (minimum 24px)
- Readable font for UI elements
- High contrast for accessibility

### Animations

- Subtle, non-distracting
- Smooth transitions between screens
- Celebratory animations on success (confetti effect optional)

## Error Handling

### Edge Cases

- No internet for Giphy: Show text-based celebration instead
- LocalStorage full: Alert user (unlikely with this app size)
- Invalid input: Ignore non-alphabet keys during lessons (except backspace)

### Validation

- User name: Required, non-empty
- Lesson completion: Must meet both accuracy AND WPM requirements

## Performance Requirements

- App must load in <2 seconds
- No lag during typing (immediate key feedback)
- Smooth animations (60fps)

## Future Expansion Considerations

- More lessons can be added easily (lesson array is extensible)
- No hardcoded lesson count in UI
- System should handle 25+ lessons without modification

## Deliverable

Single `index.html` file containing:

- Complete HTML structure
- Embedded CSS (in `<style>` tag)
- Embedded JavaScript (in `<script>` tag)
- All functionality described above
- Ready to run by opening in browser (no build process)

## Testing Checklist

- [ ] Multiple users can be created
- [ ] User progress persists across sessions
- [ ] Tutorial shows for new users only
- [ ] Lessons progress adaptively
- [ ] Error correction blocks progression
- [ ] Sounds play correctly
- [ ] Giphy GIFs load and display
- [ ] Statistics calculate correctly
- [ ] Keyboard visualization shows correct keys/fingers
- [ ] German keyboard layout is accurate
- [ ] All text is in German
- [ ] Responsive to window size (desktop sizes)
- [ ] Works offline (except Giphy GIFs)

## Success Criteria

A 9-year-old child should be able to:

1. Create their profile independently
1. Understand the tutorial without adult help
1. Complete lessons without confusion
1. Stay motivated through gamification
1. Learn proper finger placement through visual guides
1. Progress through lessons at their own pace
1. See improvement over time through adaptive learning
