# IMPLEMENTATION_PLAN.md

## Overview

This plan covers implementation of a browser-based Win95 Minesweeper clone as a single static HTML application. The objective is to deliver a faithful, single-page playable game with a fixed 16×16 Intermediate board, Win95 chrome styling, accurate input handling, and persistent best-time storage.

## Assumptions

- The project is a static browser game delivered as one standalone HTML file with inline CSS and JavaScript.
- Deployment target is local/static hosting only; no backend or app store release is required.
- The repository currently has no formal build or test framework; `npm test`, `npm run lint`, and `npm run build` are placeholders for the expected workflow.
- Risk tolerance is medium because the game combines a moderately complex logic model with pixel-accurate UI requirements.
- The current authoritative scope is defined by `REQUIREMENTS.md` and there is no separate `DESIGN.md` or `ARCHITECTURE.md` available.

## Delivery strategy

This plan uses a hybrid strategy with vertical slices for core gameplay and layered polish for UI and persistence.

- Core gameplay features are delivered in thin vertical slices so each phase yields a reviewable, playable increment.
- Visual styling and platform compatibility are layered later, after gameplay logic is complete, to avoid UI polish masking logic regressions.
- This strategy fits the static browser game because it separates the deterministic game engine from the presentation layer and keeps each review cycle focused and verifiable.

## Phase list

- Phase 1 — Project bootstrap and static app skeleton
- Phase 2 — Board model and initial board render
- Phase 3 — First-click-safe mine placement and cell reveal
- Phase 4 — Input handling and game state controls
- Phase 5 — Cascade reveal and win/loss detection
- Phase 6 — Header panel, timer, mine counter, reset flow
- Phase 7 — Best Times persistence and dialogs
- Phase 8 — Win95 visual styling and compatibility polish
- Phase 9 — Stabilization, validation, and release readiness

## Detailed phases

### Phase 1 — Project bootstrap and static app skeleton

Goal

Create the baseline repository structure and static application shell so future work can be built on a known starting point.

Scope

- Add or verify a single-page HTML scaffold for the game window.
- Add `package.json` with placeholder/sensible scripts for test, lint, and build.
- Add `IMPLEMENTATION_PLAN.md`, `TODO.md`, and `DONE.md` structure if missing.
- Keep actual gameplay logic out of this phase.

Expected files to change

- `IMPLEMENTATION_PLAN.md`
- `index.html` or equivalent root HTML file
- `package.json`
- `TODO.md`
- `DONE.md`
- `REQUIREMENTS.md` (read-only reference)

Dependencies

- No prior implementation phases required.

Risks

- Low. The main risk is overscoping by adding gameplay logic in a scaffold phase.

Tests and checks to run

- Validate that `index.html` loads in a browser with no console errors.
- Run `npm test`, `npm run lint`, or `npm run build` if scripts exist.

Review check before moving work to `DONE.md`

- Confirm the scaffold is present without any gameplay implementation.
- Confirm `package.json` defines at least `test`, `lint`, and `build` scripts.
- Confirm `TODO.md` and `DONE.md` are present and structured for phase-by-phase tracking.
- Confirm no scope creep beyond repository bootstrap.

Exact `TODO.md` entries to refresh from this phase

- [ ] Add static app skeleton with a root HTML file and empty game container
- [ ] Add `package.json` with `test`, `lint`, and `build` scripts
- [ ] Add `TODO.md` and `DONE.md` phase-tracking structure

Exit criteria for moving items to `DONE.md`

- The HTML scaffold exists and loads cleanly in a browser.
- `package.json` contains required scripts.
- `TODO.md` and `DONE.md` exist and are ready for the next phase.
- Review confirms no gameplay code was added prematurely.

### Phase 2 — Board model and initial board render

Goal

Implement the fixed 16×16 board data model and render the unrevealed cell grid in the page.

Scope

- Add a 16×16 board state representation.
- Render 256 unrevealed cells in the game window.
- Add fixed header placeholders for the mine counter, smiley button, and timer.

Expected files to change

- `index.html`
- `TODO.md`
- `DONE.md`
- Any test file if introduced (e.g. `test/*.js`)

Dependencies

- Phase 1 must be complete.

Risks

- Low. The primary risk is incorrect board sizing or page layout causing the grid to render wrong.

Tests and checks to run

- Open the page and confirm a 16×16 grid of unrevealed cells renders.
- Confirm there are no JavaScript console errors.
- Run unit tests if board-model tests are added.

Review check before moving work to `DONE.md`

- Confirm board model supports a 16×16 grid and can be referenced by cell coordinates.
- Confirm the UI renders 256 distinct unrevealed cell elements.
- Confirm game header placeholders exist.
- Confirm no functional gameplay beyond rendering.

Exact `TODO.md` entries to refresh from this phase

- [ ] Add 16×16 board state representation
- [ ] Render the unrevealed 256-cell board in the game window
- [ ] Add header panel placeholders for counter, smiley, and timer

Exit criteria for moving items to `DONE.md`

- The board model and UI render are present and visually correct.
- The page loads without script errors.
- Review confirms the implementation matches the phase goal.

### Phase 3 — First-click-safe mine placement and cell reveal

Goal

Implement mine placement on the first click and reveal individual cells with numeric values.

Scope

- Delay mine placement until the first left click.
- Guarantee the first clicked cell and its adjacent neighbours contain no mines.
- Assign cell values 0–8 after mine placement.
- Reveal single cells on click with the correct value displayed.

Expected files to change

- `index.html`
- `TODO.md`
- `DONE.md`
- Any test file for board logic

Dependencies

- Phase 2 must be complete.

Risks

- Medium. The main risk is incorrect safe-zone logic or misassigned cell values.

Tests and checks to run

- Confirm no mines exist before the first click.
- Confirm the first clicked cell and its neighbours are safe after mine placement.
- Confirm revealed cell values correspond to surrounding mine counts.
- Confirm there are no console errors.

Review check before moving work to `DONE.md`

- Confirm mine placement occurs only after the first click.
- Confirm the safety zone around the first click is enforced correctly.
- Confirm revealed cells show the correct value and state.
- Confirm no extra behaviour beyond the phase scope was introduced.

Exact `TODO.md` entries to refresh from this phase

- [ ] Implement first-click-safe mine placement
- [ ] Implement cell value assignment after mine placement
- [ ] Implement reveal of individual clicked cells

Exit criteria for moving items to `DONE.md`

- First-click-safe placement and single-cell reveal both work correctly.
- Mines are not placed until the first left click.
- Review confirms behaviour matches requirements.

### Phase 4 — Input handling and game state controls

Goal

Add full cell input handling, context menu suppression, marker cycling, and game-state guards.

Scope

- Implement left-click reveal and right-click marker cycling on unrevealed cells.
- Cycle unrevealed → flagged → question mark → unrevealed.
- Prevent left-click on flagged cells.
- Reveal question-marked cells on left-click.
- Implement chord click on a revealed number cell when flagged neighbours equal the number.
- Block all cell input when the game is won or lost, except reset via smiley button.
- Suppress the browser context menu on game elements.

Expected files to change

- `index.html`
- `TODO.md`
- `DONE.md`
- Any test files for input handling

Dependencies

- Phase 3 must be complete.

Risks

- Medium. Input complexity and chord-click semantics can cause regression if not isolated.

Tests and checks to run

- Confirm right-click cycles cell markers appropriately.
- Confirm flagged cells ignore left-clicks.
- Confirm question-marked cells reveal on left-click.
- Confirm chord click only triggers when flagged neighbour count equals the number.
- Confirm no context menu appears on game elements.

Review check before moving work to `DONE.md`

- Confirm all input paths behave per the requirements.
- Confirm the game locks input after win or loss.
- Confirm no broader game state logic was added outside this phase.

Exact `TODO.md` entries to refresh from this phase

- [ ] Add left-click reveal and right-click marker cycle handling
- [ ] Suppress browser context menus on game UI elements
- [ ] Add revealed-cell chord click behaviour and inactive-state input guard

Exit criteria for moving items to `DONE.md`

- Input handling works for unrevealed, flagged, and question-marked cells.
- Browser context menu is suppressed on all game elements.
- Review confirms correct state guarding and no scope creep.

### Phase 5 — Cascade reveal and win/loss detection

Goal

Implement recursive zero-value reveals and the end-game win/loss detection and board reveal states.

Scope

- Add recursive cascade reveal for value-0 cells.
- Ensure flagged/question-marked neighbours are not revealed by a cascade.
- Implement win detection when all non-mine cells are revealed.
- Implement loss detection when a mine is revealed directly or via chord click.
- Render win and loss states per requirements, including flagged mines and incorrect flag indicators.

Expected files to change

- `index.html`
- `TODO.md`
- `DONE.md`
- Any test files for game state logic

Dependencies

- Phase 4 must be complete.

Risks

- High. This phase contains the most complex game logic and the greatest regression risk.

Tests and checks to run

- Confirm cascade reveal opens all connected zero regions and correctly reveals border numbers.
- Confirm flagged and question-marked cells are preserved as unrevealed by cascades.
- Confirm win triggers only when every non-mine cell is revealed.
- Confirm loss reveals mines and incorrectly flagged cells correctly.
- Confirm no input is accepted after end of game.

Review check before moving work to `DONE.md`

- Confirm cascade reveal semantics match the requirements.
- Confirm win and loss board states are correct and stable.
- Confirm the game enters a final state and blocks further cell input.

Exact `TODO.md` entries to refresh from this phase

- [ ] Add recursive cascade reveal for zero-value cells
- [ ] Add win detection and end-game win board reveal
- [ ] Add loss detection and end-game loss board reveal

Exit criteria for moving items to `DONE.md`

- Cascade, win, and loss behaviour function correctly in manual test scenarios.
- End-game states are rendered per requirements.
- Review confirms all win/loss rules are implemented.

### Phase 6 — Header panel, timer, mine counter, reset flow

Goal

Implement the header panel, 7-segment counter displays, smiley button states, timer lifecycle, and new-game reset.

Scope

- Render the mine counter and timer as three-digit 7-segment displays.
- Start the timer on the first left click and stop it on win or loss.
- Clamp the timer at 999 seconds and reset to 000 on new game.
- Update the mine counter when flags are placed or removed, including negative display values.
- Implement the smiley button with default, nervous, won, and dead states.
- Implement game reset via the smiley button and the Game → New Game menu item.

Expected files to change

- `index.html`
- `TODO.md`
- `DONE.md`
- Any test files for header/timer logic

Dependencies

- Phase 5 must be complete.

Risks

- Medium. Timer and counter state must remain synchronized with game events.

Tests and checks to run

- Confirm timer starts on the first click and stops on win/loss.
- Confirm the timer clamps at 999.
- Confirm mine counter updates correctly as flags are placed and removed.
- Confirm smiley button states change for default, press, win, and loss.
- Confirm reset returns the full game to initial state.

Review check before moving work to `DONE.md`

- Confirm timer and counter both work correctly and match requirements.
- Confirm reset behavior fully restores initial state.
- Confirm smiley states are displayed and updated appropriately.

Exact `TODO.md` entries to refresh from this phase

- [ ] Add 7-segment mine counter and timer display logic
- [ ] Add smiley button states and reset functionality
- [ ] Add timer start/stop/reset lifecycle and counter update logic

Exit criteria for moving items to `DONE.md`

- Header panel functionality is complete and behaves correctly.
- Timer and counter updates are validated.
- Review confirms reset workflow is implemented.

### Phase 7 — Best Times persistence and dialogs

Goal

Implement persistent best-time recording, Win95-style modals for Best Times and new-record entry, and fallback handling for storage failures.

Scope

- Store best time and player name under `minesweeper_best_times` in `localStorage`.
- Display the Best Times dialog via Game → Best Times.
- Prompt for the player name on a new record win, pre-filled with "Anonymous".
- Support Reset Scores and OK actions in dialogs.
- Handle unavailable `localStorage` gracefully without console errors.

Expected files to change

- `index.html`
- `TODO.md`
- `DONE.md`
- Any dialog-specific test files

Dependencies

- Phase 6 must be complete.

Risks

- Medium. Persistence and UX interactions require attention to edge cases and browser storage availability.

Tests and checks to run

- Confirm best-time storage and retrieval works in normal browser mode.
- Confirm new-record dialog appears and saves the record.
- Confirm Best Times dialog displays the current record or defaults when none exists.
- Confirm Reset Scores clears stored data and resets the display.
- Confirm there are no errors when `localStorage` is unavailable.

Review check before moving work to `DONE.md`

- Confirm persistent best-time storage works and is visible in the UI.
- Confirm dialogs appear and function as required.
- Confirm fallback handling is implemented cleanly.

Exact `TODO.md` entries to refresh from this phase

- [ ] Add Best Times modal dialog and new-record name entry
- [ ] Persist best-time record in `localStorage` with graceful fallback
- [ ] Add Reset Scores button and Best Times dialog display

Exit criteria for moving items to `DONE.md`

- Best Times persistence and dialog flows are implemented and tested.
- `localStorage` failure does not break the app.
- Review confirms no scope creep into unrelated UI polish.

### Phase 8 — Win95 visual styling and compatibility polish

Goal

Apply the final Win95 visual styling and ensure the app renders correctly in target desktop browsers.

Scope

- Apply the Win95 desktop teal background and centred window layout.
- Implement all required bevel styles, title bar, menu bar, header panel, and cell visuals.
- Render 7-segment displays and pixel-style smiley faces in accordance with the requirements.
- Ensure dialogs use Win95-style borders and buttons.
- Confirm the window does not show scrollbars in a normal desktop viewport.

Expected files to change

- `index.html`
- `TODO.md`
- `DONE.md`

Dependencies

- Phase 7 must be complete.

Risks

- Medium. Visual fidelity and browser compatibility can cause regressions if styling is applied late.

Tests and checks to run

- Visually compare the app to the Win95 aesthetic requirements.
- Test in Chrome, Firefox, Edge, and Safari for layout and styling consistency.
- Confirm the game remains playable and there are no new console errors.

Review check before moving work to `DONE.md`

- Confirm the page uses the required Win95 colour palette and bevel system.
- Confirm the application window is centred and non-scrollable in the normal desktop viewport.
- Confirm dialogs and controls match the visual requirements.

Exact `TODO.md` entries to refresh from this phase

- [ ] Apply Win95 chrome styling to the window, menu, and controls
- [ ] Render pixel-style smiley faces and 7-segment displays per spec
- [ ] Confirm layout and styling are consistent across target desktop browsers

Exit criteria for moving items to `DONE.md`

- Visual styling is complete and validated.
- The UI matches the requirements without introducing regressions.
- Review confirms the app is visually finished.

### Phase 9 — Stabilization, validation, and release readiness

Goal

Verify the final application is complete, documented, and ready for handoff as a static browser game.

Scope

- Run end-to-end validation across all gameplay and UI requirements.
- Run any available automated tests, lint, and build checks.
- Update `TODO.md` and `DONE.md` to reflect completed work.
- Confirm the final static file(s) are deliverable and meet the project definition of done.

Expected files to change

- `index.html`
- `package.json`
- `TODO.md`
- `DONE.md`
- `IMPLEMENTATION_PLAN.md`

Dependencies

- Phases 1 through 8 must be complete.

Risks

- Low. This phase is validation-only, but it can reveal regressions from prior phases.

Tests and checks to run

- Run `npm test` (or project equivalent).
- Run `npm run lint` and `npm run build` if configured.
- Perform a full manual gameplay regression test from new game through win and loss.
- Confirm no console errors in all target browsers.

Review check before moving work to `DONE.md`

- Confirm all requirements from `REQUIREMENTS.md` are satisfied.
- Confirm `TODO.md` has been updated to reflect completed items and any remaining issues.
- Confirm `DONE.md` accurately tracks delivered work.
- Confirm the static app is deliverable as a single-page browser game.

Exact `TODO.md` entries to refresh from this phase

- [ ] Run final verification of gameplay, UI, and persistence behaviour
- [ ] Update `TODO.md` and `DONE.md` to reflect the final delivered state
- [ ] Run build, lint, and acceptance checks before delivery

Exit criteria for moving items to `DONE.md`

- All validation tasks are complete and pass.
- The final app is documented and ready for delivery.
- Review confirms the project meets the definition of done.

## Dependency notes

- Each phase depends on completion of the previous phase because the game build is sequential: scaffold → board → placement → input → end-state → header → persistence → styling → validation.
- Phase 5 is the first high-risk phase and should be isolated from styling and persistence work.
- Phase 8 depends on all prior gameplay features being stable so that visual polish does not mask logic bugs.
- Phase 9 is a final validation gate and must not begin until all previous phases are complete.

## Review policy

- Review size is limited to one phase per cycle. No phase should be accepted if it contains more than one major deliverable.
- If a phase grows beyond its stated scope, split it before implementation continues.
- Reviews must verify requirement traceability, regression risk, and documentation alignment.
- Reviewers must confirm `TODO.md` contains any remaining follow-up work before moving phase work to `DONE.md`.

## Definition of done for the plan

The project is complete when:

- The game is fully implemented in a static browser-deliverable package.
- All `REQUIREMENTS.md` functional, visual, and non-functional requirements are met.
- The app runs in target desktop browsers with no console errors.
- All required scripts and docs are present (`package.json`, `TODO.md`, `DONE.md`, `IMPLEMENTATION_PLAN.md`).
- Delivery readiness is verified by build/lint/test commands and manual acceptance checks.

## Open questions

- Which exact root HTML filename should be used for the final app (`index.html`, `minesweeper.html`, or another name)?
- Which chord-click input method should be implemented to match the intended behaviour most closely? (simultaneous left+right press vs. alternate modern input)
- Should the nervous smiley face appear only while the mouse is held over a revealable cell or anywhere on the board?
- Should the New Record dialog appear immediately on win or after a short delay?
- Which technique should be used for pixel-style faces and 7-segment displays: inline SVG, CSS-only, or another approach?
- Should the app explicitly clamp the mine counter at -99 if the player places more than 99 extra flags?
