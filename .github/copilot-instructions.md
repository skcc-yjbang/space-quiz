# Copilot instructions

## Architecture

- The project is a client-only quiz in `index.html`; its HTML, CSS, question data, and JavaScript live together in one file. It has no package manifest, build step, server requirement, or external dependencies.
- The UI has start, quiz, and results screens. Quiz state (`current`, `score`, `answered`, and `history`) is managed inside one JavaScript IIFE. `choose()` records an answer, `next()` advances the quiz, and `renderResults()` builds the final summary and answer review.
- English questions in `QUESTIONS` are the base data, including each correct `answer` index. `KOREAN_QUESTIONS` overlays translated topic, prompt, options, and fact at the same index; it intentionally omits the answer index. Keep both arrays in the same order and ensure translated options preserve the correct answer's index.
- Shared UI translations and language-specific formatters are in `TEXT.en` and `TEXT.ko`. Static markup uses `data-i18n`; `applyLanguage()` updates the document language/title and rerenders the active quiz or results view while preserving quiz state. Dynamic question and feedback rendering is handled by `renderQuestion()` and `renderAnswer()`.

## Build and test

- There are no build, lint, or automated test commands, and no individual test runner. Open `index.html` directly in a browser to run the app.
- For a manual smoke test, check both languages, answer selection and feedback, score/progress updates, the results review, and restarting the quiz. Keyboard shortcuts are handled in the `keydown` listener: `1`–`4` or `A`–`D` select answers, and `N` advances after an answer.

## Project conventions

- Keep the app dependency-free and self-contained in `index.html`; there is no separate source or generated output.
- When changing quiz content, update the English and Korean question entries at matching indexes. When adding interface copy, add it to both language dictionaries and use the existing translation/rendering flow rather than leaving visible copy in only one language.
- Preserve the existing accessibility behavior: update `document.documentElement.lang` on language changes, keep labels and live-region text localized, retain focus movement between quiz states, and respect reduced-motion and forced-colors styles.
- Keep user-provided or translated text assigned through `textContent`; use `innerHTML` only for fixed, trusted markup already used by the interface.
- The intro currently advertises Enter to advance, while the key handler advances with `N`; keep the help copy and actual shortcut behavior synchronized when editing keyboard controls.
