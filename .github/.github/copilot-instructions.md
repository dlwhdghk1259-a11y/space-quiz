# Copilot Instructions

## Project overview

This is a dependency-free, single-page Korean space quiz. The complete app lives in `index.html`, which contains semantic HTML, inline CSS, quiz data, state, rendering, and interaction logic. The quiz has a question screen and a result screen with score, progress, feedback, emoji reaction, and restart behavior.

There is no package manifest, build system, test framework, or lint configuration.

## Preview and validation

Run a local preview from the repository root:

```powershell
python -m http.server 8765
```

Open `http://127.0.0.1:8765/index.html`. Manually verify correct and incorrect answers, progress, all 10 questions, score count-up, result screen, restart, keyboard navigation, dark mode, and reduced-motion behavior.

## Architecture

- `questions` in `index.html` is the source of truth. Each item has `text`, `options`, and a zero-based `answer` index.
- `renderQuestion()` updates the active question, progress, answer buttons, and next action.
- `chooseAnswer()` prevents duplicate selections, disables answers, marks correct/wrong feedback, and updates the score.
- `showResults()` switches to the result screen, selects the emoji/message by score ratio, and starts the final score animation.
- `animateFinalScore()` counts from zero to the earned score and skips animation for `prefers-reduced-motion`.

## Project conventions

- Keep the deliverable self-contained in `index.html`; do not add frameworks, external assets, fonts, CDN resources, or runtime dependencies.
- User-facing text and accessibility labels are Korean; keep `lang="ko"`.
- Preserve existing DOM IDs, class names, ARIA relationships, native buttons, focus-visible styles, live regions, progressbar attributes, and focus movement unless updating every dependent selector and relationship together.
- Preserve `.is-correct`, `.is-wrong`, `.answer-note.good`, `.answer-note.bad`, `prefers-color-scheme`, and `prefers-reduced-motion` behavior.
- Keep the quiz at 10 questions unless the requirement changes; use `questions.length` for calculations.
- Prefer small localized edits inside the existing inline style/script blocks.
