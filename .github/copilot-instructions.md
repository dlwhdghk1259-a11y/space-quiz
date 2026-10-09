# Copilot Instructions

This repository is a dependency-free, single-page Korean space quiz. The complete app lives in `index.html` with inline HTML, CSS, quiz data, state, and interaction logic.

## Preview

From the repository root, run:

```powershell
python -m http.server 8765
```

Open `http://127.0.0.1:8765/index.html` and manually verify correct/wrong feedback, progress through all 10 questions, score count-up, results, restart, keyboard navigation, dark mode, and reduced motion.

## Architecture

- `questions` is the source of truth; each item has `text`, `options`, and a zero-based `answer` index.
- `renderQuestion()` updates the question, progress, answer buttons, and next action.
- `chooseAnswer()` prevents duplicate selections, disables answers, marks feedback, and updates the score.
- `showResults()` switches screens, chooses the emoji/message, and starts the final score animation.
- `animateFinalScore()` counts from zero to the earned score and respects `prefers-reduced-motion`.

## Conventions

- Keep the app self-contained in `index.html`; do not add frameworks, external assets, fonts, CDN resources, or runtime dependencies.
- User-facing text and accessibility labels are Korean; keep `lang="ko"`.
- Preserve existing DOM IDs, ARIA relationships, native buttons, focus styles, live regions, progressbar attributes, and focus movement.
- Preserve `.is-correct`, `.is-wrong`, `.answer-note.good`, `.answer-note.bad`, `prefers-color-scheme`, and `prefers-reduced-motion` behavior.
- Keep the quiz at 10 questions unless requirements change, and use `questions.length` for calculations.
