# Copilot Instructions for Space-Quiz

## Architecture & Project Structure
- **Self-contained static application**: The entire application lives in a single `index.html` file without external build dependencies, package managers, bundlers, external fonts, or external CDN scripts.
- **Narrow column centered container**: Maximum width constrained via `.quiz-container` (`max-width: 480px`).
- **State management**: Pure client-side state managed via minimal top-level variables (`currentIdx`, `score`, `hasAnswered`, and `questions`).
- **Views**:
  - `#questionView`: Displays current question category, prompt, mission-control option buttons, animated feedback panel, and the `#nextBtn` trigger.
  - `#resultsScreen`: Hidden during the quiz, revealed upon mission completion with dynamic performance tiers, accuracy breakdown, emoji reactions, and restart button.
  - `.progress-section`: Displays progress bar and counter (`Question X of 10`), hidden on the results screen and restored on restart.

## Key Conventions & Styling
- **Theme support (`prefers-color-scheme`)**:
  - CSS variables define colors for dark theme (default `:root`) and light theme (`@media (prefers-color-scheme: light)`).
  - All UI elements must derive background, border, text, and glow colors from CSS variables (`--bg`, `--card-bg`, `--accent`, `--opt-bg`, `--correct-bg`, `--wrong-bg`, `--mc-cyan`, etc.) to guarantee consistent contrast across themes.
- **Mission-Control HUD aesthetic**:
  - Option buttons feature pseudo-element corner reticles (`::before` / `::after`), monospace telemetry channel tags (`.option-telemetry` with `[CH-A]` through `[CH-D]`), and HUD state transitions (`[LOCK // CONFIRMED]`, `[ERR // ANOMALY]`, `[SYS // TARGET]`).
  - Monospace font stack referenced through `--font-mono`.
- **Keyboard navigation & accessibility**:
  - Native buttons with `aria-pressed`, `aria-label`, and `role="progressbar"`.
  - Global `keydown` listeners support option hotkeys (`1`–`4` and `A`–`D`), as well as `Enter` and `Space` to advance questions or restart.
- **Animation patterns**:
  - Correct answers trigger `.correct` (green glow and scale pulse `pulseGreen`).
  - Wrong answers trigger `.wrong` (red glow and shake `redShake`), while revealing the correct answer with `.reveal-correct`.
  - Keep animations hardware-accelerated (`transform`, `opacity`, `box-shadow`).

## Testing & Verification
- **Execution environment**: File can be served locally or inspected directly via `file:///` URL in a browser or browser canvas (`space-quiz-preview`).
- **Browser verification**: When verifying functionality or DOM states, use browser canvas actions (`read_page`, `evaluate_javascript`, `click_element`) or inspect the DOM directly.
