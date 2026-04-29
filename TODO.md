# TODO.md — Phased Work Tracking

## Overview

This file tracks work items across all 9 phases of the Minesweeper implementation.
Items move to `DONE.md` only after verification and review.

---

## Phase 1 — Project Bootstrap and Static App Skeleton

- [x] Add static app skeleton with a root HTML file and empty game container
- [x] Add package.json with test, lint, and build scripts
- [x] Add TODO.md and DONE.md phase-tracking structure

---

## Phase 2 — Board Model and Initial Board Render

- [x] Add 16×16 board state representation
- [x] Render the unrevealed 256-cell board in the game window
- [x] Add header panel placeholders for counter, smiley, and timer

---

## Phase 3 — First-Click-Safe Mine Placement and Cell Reveal

- [x] Implement first-click-safe mine placement
- [x] Implement cell value assignment after mine placement
- [x] Implement reveal of individual clicked cells

---

## Phase 4 — Input Handling and Game State Controls

- [x] Add left-click reveal and right-click marker cycle handling
- [x] Suppress browser context menus on game UI elements
- [x] Add revealed-cell chord click behaviour and inactive-state input guard

---

## Phase 5 — Cascade Reveal and Win/Loss Detection

- [x] Add recursive cascade reveal for zero-value cells
- [x] Add win detection and end-game win board reveal
- [x] Add loss detection and end-game loss board reveal

---

## Phase 6 — Header Panel, Timer, Mine Counter, Reset Flow

- [x] Add 7-segment mine counter and timer display logic
- [x] Add smiley button states and reset functionality
- [x] Add timer start/stop/reset lifecycle and counter update logic

---

## Phase 7 — Best Times Persistence and Dialogs

- [x] Add Best Times modal dialog and new-record name entry
- [x] Persist best-time record in localStorage with graceful fallback
- [x] Add Reset Scores button and Best Times dialog display

---

## Phase 8 — Win95 Visual Styling and Compatibility Polish

- [x] Apply Win95 chrome styling to the window, menu, and controls
- [x] Render pixel-style smiley faces and 7-segment displays per spec
- [x] Confirm layout and styling are consistent across target desktop browsers

---

## Phase 9 — Stabilization, Validation, and Release Readiness

- [x] End-to-end validation across all gameplay and UI requirements
- [x] Run available automated tests, lint, and build checks
- [x] Update TODO.md and DONE.md to reflect completed work
- [x] Confirm final static file is deliverable and meets project definition of done
