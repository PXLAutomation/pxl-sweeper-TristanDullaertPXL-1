# REQUIREMENTS.md — Win95 Minesweeper Clone

---

## 1. Overview

This project is a faithful browser-based clone of the classic Windows 95 Minesweeper game, delivered as a single static HTML page with no external frameworks or backend dependencies. The game replicates the original rules in full — including cascade reveal, chord clicking, and flag/question-mark cycling — while presenting a pixel-accurate Win95 visual aesthetic: beveled chrome, the system grey palette, 7-segment LED counters, and the iconic smiley button. The board is fixed at Intermediate difficulty (16×16 grid, 40 mines). Mine placement is deferred until after the player's first click, guaranteeing a safe opening move. Best times are persisted in the browser's `localStorage`.

---

## 2. Functional Requirements

### Game Initialisation

**FR-01** — On page load, the game shall initialise in a ready state: the board is fully unrevealed, the mine counter displays 40, the timer displays 000, and the smiley button shows the default happy face (😊).

**FR-02** — The board shall be fixed at 16 columns × 16 rows (256 cells total) with exactly 40 mines.

**FR-03** — Mines shall **not** be placed at game start. Mine placement shall occur immediately after the player's first left-click, ensuring the clicked cell and all 8 of its neighbours (where they exist) are mine-free.

**FR-04** — After mine placement, each non-mine cell shall be assigned a numeric value (0–8) equal to the count of mines in its Moore neighbourhood (the 8 surrounding cells, clamped to grid boundaries).

### Input Handling

**FR-05** — Left-clicking an unrevealed, unflagged cell shall reveal it.

**FR-06** — Right-clicking an unrevealed cell shall cycle its marker state in order: **unrevealed → flagged → question mark → unrevealed**. Each right-click advances the state by one step.

**FR-07** — Left-clicking a flagged cell shall have no effect.

**FR-08** — Left-clicking a question-marked cell shall reveal it (question marks do not block reveal).

**FR-09** — Left-clicking an already-revealed cell that displays a number N shall perform a **chord click**: if the count of flagged neighbours equals N, all non-flagged, unrevealed neighbours shall be revealed simultaneously. If the flagged count does not equal N, the chord click shall have no effect.

**FR-10** — No input (left-click, right-click, or chord) shall be processed on any cell while the game is in a won or lost state, except clicking the smiley button to start a new game.

**FR-11** — Right-click context menus from the browser shall be suppressed on all game elements (the grid, the smiley button, the header panel).

### Cascade Reveal

**FR-12** — When a cell with value 0 is revealed, all 8 of its neighbours shall be revealed automatically and recursively. This cascade shall propagate until all reachable cells with value 0 and their numbered border cells are revealed. Flagged and question-marked cells shall **not** be revealed by the cascade.

**FR-13** — The cascade shall be implemented without visible animation delay; all cells in a cascade group shall appear revealed simultaneously.

### Timer

**FR-14** — The timer shall start counting upward (in whole seconds) on the player's first left-click.

**FR-15** — The timer shall stop when the game is won or lost.

**FR-16** — The timer shall display a maximum value of 999. If elapsed time exceeds 999 seconds, it shall remain clamped at 999.

**FR-17** — The timer shall reset to 000 when a new game begins.

### Mine Counter

**FR-18** — The mine counter shall initialise at 40 (the total mine count).

**FR-19** — Each time the player places a flag, the mine counter shall decrement by 1. Each time a flag is removed (cycled to question mark or unrevealed), the counter shall increment by 1.

**FR-20** — The mine counter shall display negative values (e.g. -01, -09) if the player places more flags than there are mines, using a leading minus sign in place of the hundreds digit. The minimum displayable value is -99.

**FR-21** — The mine counter shall reset to 40 when a new game begins.

### Win Condition

**FR-22** — The game is won when every non-mine cell has been revealed. Flagging all mines is **not** required to win.

**FR-23** — On win: the timer shall stop, the smiley button shall switch to the sunglasses face (😎), all unflagged mines shall automatically receive a flag marker, and the Best Times dialog shall be triggered if the elapsed time is a new record (see FR-30 to FR-34).

### Loss Condition

**FR-24** — The game is lost when the player reveals a mine cell (directly or via chord click).

**FR-25** — On loss: the timer shall stop, the smiley button shall switch to the dead face (😵), and the board shall enter a revealed-loss state as follows:
- The triggering mine cell shall be displayed with a red background.
- All other un-flagged mines shall be revealed (shown as mines, not flagged).
- All incorrectly flagged cells (flagged but containing no mine) shall be displayed with an X-mine icon.
- Correctly placed flags shall remain as flags.
- All other unrevealed cells shall remain unrevealed.

**FR-26** — Question-marked cells that contain mines shall be revealed as mines on loss (not as question marks).

### Smiley Button

**FR-27** — The smiley button shall display four distinct states:
- **Default** (game in progress, no mouse held): standard happy face.
- **Nervous** (player holds left mouse button down over any cell): surprised/nervous face. This state shall revert to default if the mouse is released without revealing a cell (e.g. released over a flagged cell).
- **Won**: sunglasses face.
- **Dead**: dead face.

**FR-28** — Clicking the smiley button at any time shall start a new game: reset the board, timer, mine counter, and smiley state to their initial values.

### Menu Bar

**FR-29** — The menu bar shall contain a single menu: **Game**. Clicking "Game" shall open a dropdown with two items:
- **New Game** — equivalent to clicking the smiley button; resets the game.
- **Best Times…** — opens the Best Times dialog (see FR-30 to FR-34).

The menu shall close if the player clicks outside it or presses Escape.

### Best Times

**FR-30** — Best Times shall be stored in `localStorage` under the key `minesweeper_best_times`. The stored value shall be a JSON object with a single field: `intermediate` (an object with fields `time` (integer seconds) and `name` (string)).

**FR-31** — On a win, if no best time exists, or if the elapsed time is strictly less than the stored best time, the player shall be prompted to enter their name via an in-game Win95-styled dialog (not a browser `prompt()`). The dialog shall contain a text input pre-filled with "Anonymous" and an OK button.

**FR-32** — On confirming the dialog, the new best time and name shall be saved to `localStorage`.

**FR-33** — The Best Times dialog (accessible via Game → Best Times…) shall display one row: **Intermediate: [time]s — [name]**. If no best time has been recorded, the time shall display as "999" and the name as "Anonymous".

**FR-34** — The Best Times dialog shall have a single **Reset Scores** button that clears the stored entry and resets the display to its defaults, and an **OK** button to close the dialog.

---

## 3. Visual & UI Requirements

### Overall Chrome & Layout

**VR-01** — The game shall render as a Win95-style application window, horizontally and vertically centred on a page background coloured `#008080` (Win95 teal desktop).

**VR-02** — The window shall be non-resizable and sized to exactly fit its contents (title bar + menu bar + header panel + board). No scrollbars shall appear within the window.

**VR-03** — The Win95 colour palette used throughout shall be:

| Name | Hex | Usage |
|---|---|---|
| Window grey | `#C0C0C0` | Default background of all surfaces |
| Dark shadow | `#808080` | Bottom-right bevel (outer) |
| Darkest shadow | `#000000` | Bottom-right bevel (inner) |
| Highlight | `#FFFFFF` | Top-left bevel (outer) |
| Light highlight | `#DFDFDF` | Top-left bevel (inner) |
| Desktop teal | `#008080` | Page background |
| LED background | `#000000` | 7-segment display background |
| LED active | `#FF0000` | Lit 7-segment digit |
| LED inactive | `#8B0000` | Unlit (ghost) 7-segment digit |
| Mine red | `#FF0000` | Background of triggering mine cell on loss |
| Number colours | See VR-12 | Per-number colouring of revealed cells |

### Bevel System

**VR-04** — All raised surfaces (unrevealed cells, buttons, outer window border, header panel border) shall use a 2px bevel: top and left edges `#FFFFFF` (outer) and `#DFDFDF` (inner); bottom and right edges `#000000` (outer) and `#808080` (inner).

**VR-05** — All sunken surfaces (the header panel inset, the grid inset, revealed cells, pressed buttons) shall use a 2px bevel: top and left edges `#808080` (outer) and `#000000` (inner); bottom and right edges `#DFDFDF` (outer) and `#FFFFFF` (inner).

**VR-06** — The outer window border shall be raised (VR-04). The header panel and grid container shall each be individually inset (VR-05) within the window body.

### Title Bar

**VR-07** — The title bar shall span the full window width. Background: the Win95 active title bar gradient from `#000080` (left) to `#1084D0` (right), or solid `#000080` as a fallback. Text: "Minesweeper" in white, bold, left-aligned with standard padding. The title bar shall include a right-aligned close button (`✕`) rendered in Win95 style (raised bevel, grey background) — clicking it shall have no effect (the window cannot be closed).

### Menu Bar

**VR-08** — The menu bar shall sit immediately below the title bar. Background `#C0C0C0`. Text in the system font (see VR-09). The "Game" item shall appear highlighted (inverted: white text on `#000080` background) when its dropdown is open.

**VR-09** — The system font throughout the entire UI shall be `"MS Sans Serif", "Microsoft Sans Serif", Arial, sans-serif` at 11px (or 8pt), non-antialiased where possible (use `image-rendering: pixelated` or `-webkit-font-smoothing: none` where applicable).

### Header Panel

**VR-10** — The header panel shall contain three elements horizontally arranged: the mine counter (left), the smiley button (centre), and the timer (right). The panel background is `#C0C0C0` and it shall be inset (VR-05) within the window.

### 7-Segment LED Display

**VR-11** — Both the mine counter and timer shall be rendered as 7-segment LED displays showing exactly 3 digits. Each display background is `#000000`. Lit segments: `#FF0000`. Unlit (ghost) segments: `#8B0000`. Each digit shall be drawn using CSS or SVG faithfully replicating the 7-segment layout (segments: top, top-left, top-right, middle, bottom-left, bottom-right, bottom). Digits 0–9 and the minus sign (−) shall be supported.

### Smiley Button

**VR-12** — The smiley button shall be a raised square button (VR-04), approximately 26×26px. It shall display one of four emoji-style faces rendered in pixel art or as styled HTML/CSS (not system emoji, to ensure cross-platform consistency):
- Default: yellow face, eyes open, smile.
- Nervous: yellow face, eyes open, flat/straight mouth (O-shaped open mouth).
- Won: yellow face, sunglasses, smile.
- Dead: yellow face, X-eyes, flat mouth.

When the button is held down (mousedown), it shall appear pressed (VR-05 bevel). On mouseup it returns to raised.

### Grid Cells

**VR-13** — Each cell shall be exactly 16×16px. The grid shall therefore be 256×256px (16 columns × 16 rows, no gaps between cells).

**VR-14** — **Unrevealed cell**: raised bevel (VR-04), background `#C0C0C0`. No text or icon.

**VR-15** — **Revealed empty cell (value 0)**: sunken bevel (VR-05), background `#C0C0C0`. No text or icon. The border shall be 1px sunken rather than the full 2px, to match the original appearance.

**VR-16** — **Revealed numbered cell**: sunken bevel (VR-05), background `#C0C0C0`. Number centred in the cell, bold, 13px, in the following colours:

| Number | Colour | Hex |
|---|---|---|
| 1 | Blue | `#0000FF` |
| 2 | Green | `#008000` |
| 3 | Red | `#FF0000` |
| 4 | Dark blue | `#000080` |
| 5 | Dark red | `#800000` |
| 6 | Cyan | `#008080` |
| 7 | Black | `#000000` |
| 8 | Grey | `#808080` |

**VR-17** — **Flagged cell**: raised bevel (VR-04), background `#C0C0C0`. Displays a flag icon: a red (`#FF0000`) rectangular flag on a black pole, above a black baseline, centred in the cell. The flag shall be pixel-art style, approximately 7×7px flag body.

**VR-18** — **Question-marked cell**: raised bevel (VR-04), background `#C0C0C0`. Displays a grey (`#808080`) or black `?` character centred in the cell.

**VR-19** — **Revealed mine (non-triggering, on loss)**: sunken bevel (VR-05), background `#C0C0C0`. Displays a mine icon: black circular body with 8 spikes radiating outward and a small highlight dot, centred in the cell. The mine shall be pixel-art style.

**VR-20** — **Triggering mine (on loss)**: same as VR-19 but background is `#FF0000` (red).

**VR-21** — **Incorrectly flagged cell (on loss)**: reveals the mine icon (VR-19) with a red `✕` overlaid.

**VR-22** — When the player holds left mouse button down over a non-flagged, unrevealed cell, that cell shall appear in a pressed/sunken state (VR-05) while the button is held, reverting if the button is released without confirming.

### Dialogs

**VR-23** — All dialogs (Best Times, New Record name entry) shall be rendered as Win95-style modal windows: raised outer border (VR-04), `#C0C0C0` background, title bar (VR-07 style), and standard Win95 button styling for all buttons. Dialogs shall appear centred over the game window. The page background shall not be dimmed.

**VR-24** — Buttons within dialogs shall be raised (VR-04), minimum 75px wide, 23px tall, with the system font (VR-09). On mousedown they shall appear pressed (VR-05).

---

## 4. Non-Functional Requirements

**NR-01** — The entire application shall be deliverable as a **single `.html` file**. All CSS and JavaScript shall be inline (within `<style>` and `<script>` tags). No external files, CDN links, or network requests are required to run the game.

**NR-02** — The application shall use **no JavaScript frameworks or libraries** (no React, Vue, jQuery, etc.). Vanilla HTML, CSS, and JavaScript only.

**NR-03** — The application shall use **no CSS frameworks** (no Bootstrap, Tailwind, etc.).

**NR-04** — Best times shall be persisted using the browser's `localStorage` API under the key `minesweeper_best_times`. The application shall handle the case where `localStorage` is unavailable (e.g. private browsing mode) by disabling persistence gracefully without throwing errors.

**NR-05** — The game shall function correctly in the current stable releases of Chrome, Firefox, Edge, and Safari at the time of development. No polyfills for legacy browsers (IE, pre-Chromium Edge) are required.

**NR-06** — The game shall be fully playable using a standard mouse. Keyboard-only play is not required. Touch/mobile support is not required.

**NR-07** — The application shall produce no JavaScript console errors during normal gameplay.

**NR-08** — The game logic (mine placement, cascade, win/loss detection) shall execute synchronously and complete within a single event loop tick (no perceptible lag on modern hardware).

---

## 5. Out of Scope

The following features are explicitly **not** part of this project:

- **Difficulty selection** — no Beginner, Expert, or Custom board sizes; only Intermediate (16×16, 40 mines).
- **Mobile / touch support** — no tap-to-reveal, no long-press-to-flag, no responsive layout.
- **Keyboard accessibility** — no keyboard navigation of cells or menus.
- **Window dragging** — the game window is static and cannot be moved.
- **Window resizing or minimising** — the close button has no effect; min/max buttons are not present.
- **Multiplayer** — single-player only.
- **Sound effects** — no audio of any kind.
- **Animations** — no reveal animations, transitions, or particle effects.
- **High DPI / Retina scaling** — pixel art assets are rendered at 1x; scaling is left to the browser default.
- **Server-side persistence** — best times are local to the browser only.
- **Multiple best-time entries** — only one record (fastest time + name) is stored per the single difficulty.
- **The "Help" menu** — the menu bar contains only the "Game" menu.
- **Marks menu option** — the original Win95 game had a "Marks (?)" toggle to enable/disable question marks; this toggle is not included. Question marks are always enabled.

---

## 6. Open Questions

**OQ-01 — Chord click trigger mechanism.** The original Win95 Minesweeper used simultaneous left+right mouse button press (not a second left-click) to trigger chord clicks. Some modern clones use double-left-click or middle-click instead. **Decision needed:** which input method to implement? *Recommendation: simultaneous left+right press, as it is most faithful; but this requires tracking both button states and may feel unnatural to users unfamiliar with the original.*

**OQ-02 — Nervous face trigger scope.** Should the nervous face appear only when holding the mouse down over a revealable cell, or any time the left mouse button is held down anywhere on the board? *The original showed nervous any time the mouse was held on the board, regardless of cell state.*

**OQ-03 — Question mark on revealed cells after cascade.** If a question-marked cell is adjacent to a cascade, should it be revealed by the cascade? FR-12 specifies it should not — confirm this is the desired behaviour.

**OQ-04 — Mine counter display for values below -99.** FR-20 specifies a minimum of -99 for the mine counter. If the player places more than 99 extra flags (109 flags on a 40-mine board), the display will overflow. *Recommendation: clamp at -99 and do not worry about this edge case.*

**OQ-05 — New Record dialog timing.** Should the New Record dialog appear immediately on win, or after a short delay? *The original appeared immediately. A brief delay (e.g. 300ms) may feel more natural in a browser context.*

**OQ-06 — Pixel-art face rendering approach.** VR-12 specifies the smiley faces should not use system emoji. The implementation could use: (a) inline SVG, (b) Canvas-drawn pixel art, or (c) CSS-only faces. *Decision needed before implementation: inline SVG is recommended for simplicity and sharpness.*

**OQ-07 — 7-segment display rendering approach.** VR-11 requires faithful 7-segment rendering. Options: (a) CSS `div`-based segments, (b) inline SVG per digit, (c) a small Canvas. *Recommendation: CSS `div`-based segments for simplicity and pure-HTML compliance.*

**OQ-08 — Behaviour of chord click that hits a mine.** If the player chord-clicks and a neighbour is a mine but was not flagged (i.e. the flagged count equals N but a flag is misplaced), the game should end. FR-09 covers this implicitly via FR-24 (revealing a mine triggers loss), but it should be confirmed that the loss sequence handles simultaneous multi-cell reveal correctly — specifically, the triggering cell should be the first mine revealed, shown with the red background.

**OQ-09 — Window centring behaviour on small viewports.** If the browser viewport is smaller than the game window (unlikely on desktop but possible), should the window scroll or overflow? *Recommendation: allow the page to scroll; do not scale the game window.*

## Project Setup Requirements

- The project shall include a `package.json` file in the repository root.
- The `package.json` file shall define the project's runnable commands in a consistent way.
- The `package.json` file shall include at least a `test` script so the same test command can be run every time.
- If the project uses ES module imports in JavaScript, `package.json` shall set `"type": "module"`.
- The project shall remain compatible with a plain JavaScript, static-site workflow.