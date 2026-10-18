# Copilot instructions

## Project shape

This is a dependency-free, single-page browser quiz. The complete application lives in `index.html`:

- The `<style>` block contains all layout, responsive behavior, light/dark theme tokens, answer states, and animations.
- The quiz data is the `questions` array in the `<script>` block. Each item has a `question`, four `answers`, and a zero-based `correct` answer index.
- The script renders one question at a time into the existing `#answers` container, tracks `currentQuestion`, `score`, and an answer-lock flag, and updates the DOM directly.
- `chooseAnswer` applies correct/wrong feedback, updates the score, disables the current answer buttons, and advances after a short feedback delay.
- `showResults` switches from `#quiz` to `#results` and selects the result emoji and copy from the final score ratio. `restart` resets state and renders Question 1.

Keep the app runnable directly from a local `file://` URL. Do not introduce a server, bundler, framework, package manager, external fonts, or runtime dependency unless the project requirements explicitly change.

## Build, test, and lint

There are no configured build, test, or lint commands and no dependency manifest. Open `index.html` directly in a browser for manual validation.

For a focused smoke test, verify:

1. The initial screen shows Question 1 of 10, score 0, and four answer buttons.
2. Answer buttons can be reached and activated with the keyboard; focus-visible styling remains clear.
3. A correct answer turns green, increments the score, and advances after feedback.
4. A wrong answer shakes/red-marks the selected option, highlights the correct option, leaves the score unchanged, and advances.
5. The progress bar advances and reaches 100% on completion.
6. The results screen shows the final score, an emoji reaction, score-specific copy, and a working Play again button.
7. Play again restores Question 1 and score 0.

## Implementation conventions

- Preserve the narrow centered card layout and the ivory/red visual language. Add new colors through the existing CSS custom properties where possible.
- Maintain both `prefers-color-scheme: light/dark` behavior and responsive rules for screens at or below 480px.
- Use the existing DOM IDs and class names as the JavaScript/CSS contract; update all related surfaces together when changing quiz state or markup.
- Keep answer choices as native `<button type="button">` elements so keyboard activation and disabled states work without custom event handling.
- Use text updates through `textContent` for dynamic question, feedback, score, and result copy. The current answer rendering uses a small fixed template for answer markup; preserve that pattern and do not insert untrusted content.
- Keep answer feedback mutually understandable: `.correct` marks the correct option, `.wrong` marks an incorrect selection, and `.correct-text`/`.wrong-text` style the status message.
- When changing timing or progression behavior, preserve the `locked` guard so multiple clicks cannot score or advance a question more than once.
- Keep the 10-question contract consistent across the questions array, progress labels, progress calculations, and final score copy. Prefer deriving counts from `questions.length` rather than duplicating a total.
