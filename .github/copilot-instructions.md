# Copilot Instructions

## Project overview

This repository is a dependency-free, single-page Korean space quiz. The complete application lives in `index.html`; it contains the semantic HTML structure, all CSS, the quiz data, and the client-side state/interaction logic.

The app has two UI states:

- The quiz screen renders one question at a time, four answer buttons, current score, and progress.
- The result screen replaces the quiz after the tenth answer and shows the final score, an emoji reaction, a result message, and a restart action.

There is no server-side code, package manifest, build system, test framework, or lint configuration. The app is intended to open directly as a static HTML file.

## Build, test, and preview commands

There is no build, test, or lint command configured in this repository.

For a local browser preview, run this from the repository root:

```powershell
python -m http.server 8765
```

Then open `http://127.0.0.1:8765/index.html`. This server is only a development preview; do not add a runtime server or dependency for normal app behavior.

When validating changes manually, exercise the full quiz flow: select a correct answer, select a wrong answer, advance through all 10 questions, verify the final score animation/result state, and use the restart button.

## Architecture and data flow

- The static HTML defines the persistent shell (`.topbar`) and the two screen containers: `#quiz-screen` and `#result-screen`.
- The CSS in the `<style>` block owns the visual system, responsive layout, light/dark color tokens, answer feedback animations, result score animation presentation, and reduced-motion behavior.
- The JavaScript in the `<script>` block owns all application state:
  - `questions` is the single source of truth for the 10 question objects. Each object has `text`, `options`, and a zero-based `answer` index.
  - `currentQuestion` tracks the active question; `points` tracks correct answers; `answered` prevents multiple selections for the same question.
  - `renderQuestion()` updates question text, progress, answer buttons, and next-button state.
  - `chooseAnswer()` disables all answers, marks the correct answer, marks an incorrect selection when applicable, updates score, and exposes the next action.
  - `showResults()` switches screens, selects the result emoji/message based on score ratio, and starts the final score animation.
  - `animateFinalScore()` counts from zero to the earned score and bypasses animation when `prefers-reduced-motion: reduce` is active.
- Dynamic answer buttons are generated from the current question rather than hard-coded. Preserve this data-driven flow when changing quiz content.
- Screen switching is handled by toggling the `active` class. Keep both screen containers in the document so focus management and screen-reader labels remain stable.

## Repository-specific conventions

- Keep the deliverable self-contained in `index.html`. Do not introduce frameworks, package dependencies, external fonts, CDN assets, or separate runtime files unless the user explicitly changes the project scope.
- User-facing copy is Korean and the document language is `lang="ko"`. New visible text, feedback, result messages, button labels, and accessibility labels should also be Korean.
- Keep quiz answers in the question object's `options` array and refer to the correct choice with its zero-based `answer` index.
- Use the existing DOM IDs and class names when possible because the script queries those selectors directly. If a selector changes, update every corresponding query and ARIA relationship in the same edit.
- Preserve the existing feedback contract: correct answers use `.is-correct` and `.answer-note.good`; wrong selections use `.is-wrong` and `.answer-note.bad`.
- Preserve the current accessibility behavior: native `<button>` controls, visible `:focus-visible` styles, `aria-live` for score/feedback, progressbar attributes, and focus movement to the next action/result screen.
- Preserve both `prefers-color-scheme` support and the `prefers-reduced-motion` fallback when changing animations or colors.
- Keep the quiz at 10 questions unless the product requirement explicitly changes the question count. If the count changes, update the related copy and rely on `questions.length` for progress/result calculations.
- Prefer small, localized edits within the existing inline `<style>` or `<script>` blocks over restructuring the single-file app.
