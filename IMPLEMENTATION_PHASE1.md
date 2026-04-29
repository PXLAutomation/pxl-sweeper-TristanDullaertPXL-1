# IMPLEMENTATION_PHASE1.md — Project Bootstrap and Static App Skeleton

**Phase**: 1 of 9  
**Title**: Project bootstrap and static app skeleton  
**Status**: Ready for execution  
**Last updated**: 2026-04-23

---

## 1. Executive Summary

Phase 1 establishes the minimal repository structure and static application shell required to support all future phases. This phase creates three core artifacts:
1. **`index.html`** — A bare-bones HTML scaffold with a centered game window container, no gameplay logic.
2. **`package.json`** — Defines `test`, `lint`, and `build` scripts with sensible defaults.
3. **`TODO.md` and `DONE.md`** — Phase-tracking files to manage work across all 9 phases.

**Critical constraint**: This phase must produce zero gameplay logic. Its sole purpose is to prepare the foundation.

---

## 2. Architectural Design

### 2.1 Document Structure and Scope

The static HTML file shall be structured as a single, self-contained delivery artifact with:
- **Inline CSS** — All styles embedded in a `<style>` block in the `<head>`.
- **Inline JavaScript** — All game logic in a `<script>` block before the closing `</body>` tag.
- **No external dependencies** — No frameworks, no CDN resources, no build-time compilation (though `npm` scripts are placeholders for future use).

### 2.2 HTML Structure (Phase 1 Skeleton)

```
<body>
  ├── #game-window (main container, centered, Win95-styled)
  │   ├── .title-bar (Win95 title bar, display only in Phase 1)
  │   ├── .menu-bar (Win95 menu, display only in Phase 1)
  │   ├── #game-container (game-content area)
  │   │   ├── #header-panel (placeholder, will contain timer/counter/smiley in Phase 6)
  │   │   └── #board-container (placeholder, will contain grid cells in Phase 2)
  └── (desktop background: Win95 teal #008080)
```

### 2.3 CSS Scaffolding (Phase 1 Minimal)

Phase 1 CSS shall define:
1. **Page-level styles**
   - Body background set to Win95 teal (`#008080`).
   - Flexbox container to center the game window vertically and horizontally.
   - Font family and baseline text styling.

2. **Window chrome placeholders**
   - `.title-bar` — Non-functional title bar for visual reference.
   - `.menu-bar` — Non-functional menu bar placeholder.
   - `#game-window` — Outer border and sizing.
   - `#game-container` — Inner content area.

3. **Placeholder regions**
   - `#header-panel` — Empty div, will expand in Phase 6.
   - `#board-container` — Empty div, will expand in Phase 2.

**No bevel styling, no cell styling, no interactive styles in Phase 1.**

### 2.4 JavaScript Scaffolding (Phase 1 Minimal)

Phase 1 JavaScript shall define minimal global state and placeholder functions:

```javascript
// Phase 1: Game state initialization (placeholder)
const gameState = {
  initialized: true,
  phase: 1,
};

// Placeholder function to verify game loads
function initializeGame() {
  console.log('Game initialized (Phase 1 scaffold)');
}

// Invoke on page load
document.addEventListener('DOMContentLoaded', initializeGame);
```

**Rationale**: This establishes the pattern for future phases without adding logic.

### 2.5 State Definitions (Phase 1 Scope)

No game state is required in Phase 1. The `gameState` object exists only to verify the page loads correctly.

---

## 3. File-Level Strategy

### 3.1 Files to Create or Modify

| File | Action | Responsibility |
|------|--------|-----------------|
| `index.html` | **Create** | Root HTML scaffold: title bar, menu bar, header panel placeholder, board container placeholder. Inline CSS and JS. |
| `package.json` | **Create** | Define `test`, `lint`, and `build` npm scripts. Set version to `0.1.0`. Include a minimal `"main"` entry pointing to `index.html`. |
| `TODO.md` | **Create** | Phase-tracking file. Populate with Phase 1 tasks from `IMPLEMENTATION_PLAN.md`. Structure for phases 2–9. |
| `DONE.md` | **Create** | Phase-tracking file. Initially empty. Items move here after review and verification. |
| `REQUIREMENTS.md` | **Read-only** | No changes. Reference for acceptance criteria. |
| `IMPLEMENTATION_PLAN.md` | **Read-only** | No changes. Reference for phase definitions. |

### 3.2 No Files to Delete

No existing files require deletion in Phase 1.

### 3.3 Directory Structure (No Change)

Phase 1 does not introduce new directories. All artifacts remain at the repository root:

```
pxl-sweeper-TristanDullaertPXL-1/
├── index.html (new)
├── package.json (new)
├── TODO.md (new)
├── DONE.md (new)
├── IMPLEMENTATION_PLAN.md (existing)
├── REQUIREMENTS.md (existing)
├── GEMINI.md (existing)
├── CLAUDE.md (existing)
├── AGENTS.md (existing)
└── .git/ (existing)
```

---

## 4. Atomic Execution Steps

Each step below follows the **Plan-Act-Validate** cycle.

### Step 1: Create `package.json`

**Plan**
- Define a minimal `package.json` with project metadata and npm scripts.
- Scripts shall be placeholders that either echo a message or run a no-op command.
- Include `"name"`, `"version"`, `"description"`, and `"scripts"`.

**Act**
1. Create `package.json` at the repository root.
2. Populate with:
   - `name`: "pxl-sweeper"
   - `version`: "0.1.0"
   - `description`: "Win95 Minesweeper clone browser game"
   - `main`: "index.html"
   - `scripts`:
     - `test`: "echo 'No tests yet' && exit 0"
     - `lint`: "echo 'No linting configured' && exit 0"
     - `build`: "echo 'Static build: copy index.html to output' && exit 0"

**Validate**
- [ ] `package.json` exists at the repository root.
- [ ] `npm test`, `npm run lint`, and `npm run build` all succeed without error.
- [ ] Commands return exit code 0.
- [ ] File is valid JSON (no syntax errors).

---

### Step 2: Create `index.html` with Static Scaffold

**Plan**
- Create a single-file HTML document with:
  - Proper DOCTYPE, `<html>`, `<head>`, `<body>` structure.
  - Inline `<style>` block with Phase 1 CSS (centring, background, placeholders).
  - Inline `<script>` block with Phase 1 JavaScript (minimal state and init).
  - Semantic placeholder elements: `.title-bar`, `.menu-bar`, `#header-panel`, `#board-container`.
  - Meta tags for UTF-8 charset and viewport (responsive).

- CSS shall:
  - Set body background to `#008080`.
  - Center the game window using flexbox.
  - Define `.title-bar`, `.menu-bar`, `#game-window`, `#game-container` placeholders.
  - Use baseline font sizing (16px).

- JavaScript shall:
  - Define a global `gameState` object with `{ initialized: true, phase: 1 }`.
  - Define an `initializeGame()` function that logs to console.
  - Invoke `initializeGame()` on `DOMContentLoaded`.

**Act**
1. Create `index.html` at the repository root.
2. Include full HTML boilerplate with proper formatting.
3. Add inline CSS for page-level and placeholder styles (see CSS Scaffolding section above).
4. Add inline JavaScript for game state and init function (see JavaScript Scaffolding section above).
5. Verify no syntax errors or broken references.

**Validate**
- [ ] `index.html` exists at the repository root.
- [ ] File is valid HTML (no parsing errors).
- [ ] Page loads in a browser without console errors.
- [ ] Browser console shows: "Game initialized (Phase 1 scaffold)".
- [ ] Page displays a centered, teal background.
- [ ] Placeholder elements are visible (title bar, menu bar, empty grid region).

---

### Step 3: Create `TODO.md` with Phase-Tracking Structure

**Plan**
- Create a unified to-do file tracking all work across phases 1–9.
- Populate Phase 1 section with exact tasks from `IMPLEMENTATION_PLAN.md`.
- Stub out sections for phases 2–9 as templates to be filled in future cycles.
- Structure: `## Phase [N] — [Title]` followed by a checklist of tasks.

**Act**
1. Create `TODO.md` at the repository root.
2. Add a header explaining the file's purpose and structure.
3. Populate Phase 1 with the exact tasks from `IMPLEMENTATION_PLAN.md` Section 4:
   - `[ ] Add static app skeleton with a root HTML file and empty game container`
   - `[ ] Add package.json with test, lint, and build scripts`
   - `[ ] Add TODO.md and DONE.md phase-tracking structure`
4. Add stub sections for phases 2–9 with placeholder text "Tasks to be populated when phase begins."

**Validate**
- [ ] `TODO.md` exists at the repository root.
- [ ] Phase 1 checklist contains all three tasks.
- [ ] All tasks use `[ ]` (unchecked) format.
- [ ] Phases 2–9 exist as stubs.
- [ ] Markdown formatting is valid.

---

### Step 4: Create `DONE.md` with Phase-Tracking Structure

**Plan**
- Create a unified done file to track completed, reviewed, and verified work.
- Initialize as empty (no items move to DONE until Phase 1 review passes).
- Include a header explaining the file's purpose and format.

**Act**
1. Create `DONE.md` at the repository root.
2. Add a header explaining that items move to DONE only after review and verification.
3. Leave the file empty initially or include a comment like "Phase 1 items will appear here after review."

**Validate**
- [ ] `DONE.md` exists at the repository root.
- [ ] File is initially empty or contains only header comments.
- [ ] Markdown formatting is valid.

---

### Step 5: Verify No Gameplay Logic is Present

**Plan**
- Audit `index.html` to confirm zero gameplay mechanics are implemented.
- Check that no board state, mine placement, cell reveal, input handlers, or game rules are present.

**Act**
1. Search `index.html` for the following keywords; each should NOT be found:
   - "mine"
   - "reveal"
   - "click"
   - "board"
   - "cell"
   - "cascade"
   - "flag"
   - "timer"
   - "counter"
   - (Except in comments or placeholder text, which are acceptable.)

**Validate**
- [ ] No gameplay logic is present in `index.html`.
- [ ] No event listeners for left-click, right-click, or mouse events are defined.
- [ ] `gameState` contains only `{ initialized: true, phase: 1 }` with no game rules.
- [ ] Comments confirm this is a scaffold-only phase.

---

## 5. Edge Cases & Boundary Audit

### 5.1 Bootstrap-Specific Edge Cases

| Edge Case | Risk | Mitigation |
|-----------|------|-----------|
| **Invalid JSON in `package.json`** | High | Validate JSON syntax before commit. Run `npm test` to verify npm can parse the file. |
| **HTML parsing error** | High | Test in browser DevTools to confirm no parse errors appear in console. |
| **Broken Flexbox centering** | Medium | Test in multiple browsers (Chrome, Firefox, Safari, Edge) to confirm centring works on various viewport sizes. Fallback to basic centering if flexbox fails. |
| **Incorrect charset or viewport meta tags** | Low | Include proper `<meta charset="UTF-8">` and `<meta name="viewport" content="width=device-width, initial-scale=1">` to avoid encoding or mobile rendering issues. |
| **Console error on page load** | High | Test page load in browser with DevTools open. Should show exactly one log line: "Game initialized (Phase 1 scaffold)". |
| **npm scripts fail silently** | Medium | All npm scripts must return exit code 0 even if they do nothing. Use `&& exit 0` pattern to ensure success exit. |
| **Empty or missing placeholders** | Low | Confirm `#header-panel` and `#board-container` divs exist in HTML. They should be visibly empty but present. |
| **Inline CSS/JS conflicts with future phases** | Medium | Keep Phase 1 CSS and JS minimal and isolated. Use clear comments to mark Phase 1-specific code so it doesn't interfere with Phase 2+ implementations. |

### 5.2 Scope Creep Risks

| Risk | Prevention |
|------|-----------|
| **Adding early UI styling (bevels, colours, fonts)** | Resist. Phase 1 uses only basic centring and placeholder elements. All Win95 styling happens in Phase 8. |
| **Pre-implementing game state structures** | Resist. The `gameState` object is minimal. The board, mine placement, and game rules are Phase 2+. |
| **Adding test files or build tooling** | Resist. npm scripts are placeholders (`echo` commands). No actual test runner, linter, or bundler is configured in Phase 1. |
| **Modifying existing docs** | Resist. Only create new files (index.html, package.json, TODO.md, DONE.md). Do not edit REQUIREMENTS.md or IMPLEMENTATION_PLAN.md. |

---

## 6. Verification Protocol

### 6.1 Checklist: Pre-Review Validation

Before marking Phase 1 as complete, verify all of the following manually:

- [ ] **File Presence**
  - [ ] `index.html` exists at the root.
  - [ ] `package.json` exists at the root.
  - [ ] `TODO.md` exists at the root with Phase 1 tasks.
  - [ ] `DONE.md` exists at the root (initially empty).

- [ ] **HTML Validation**
  - [ ] Open `index.html` in a browser (Chrome, Firefox, Safari, Edge).
  - [ ] Page loads without console errors.
  - [ ] Browser console displays exactly: "Game initialized (Phase 1 scaffold)".
  - [ ] Page shows a centered, teal background.
  - [ ] Title bar and menu bar placeholders are visible (text or borders).
  - [ ] Header panel and board container divs are present (even if empty).

- [ ] **npm Scripts**
  - [ ] Run `npm test` in terminal; command succeeds with exit code 0.
  - [ ] Run `npm run lint` in terminal; command succeeds with exit code 0.
  - [ ] Run `npm run build` in terminal; command succeeds with exit code 0.
  - [ ] No errors or warnings appear in npm output.

- [ ] **JSON Validity**
  - [ ] Run `npm test` again to verify `package.json` is valid JSON.
  - [ ] `package.json` contains all required fields: `name`, `version`, `description`, `main`, `scripts`.
  - [ ] `scripts` object contains `test`, `lint`, and `build` entries.

- [ ] **Scope Verification**
  - [ ] No gameplay logic is present (no reveal, mine placement, input handlers, cascade logic).
  - [ ] No Win95 styling beyond basic centring (no bevels, colors, or pixel-perfect CSS).
  - [ ] No test framework or build tooling is active (scripts are echo/no-op).
  - [ ] No external dependencies or CDN resources are used.

### 6.2 Checklist: Review Gates (Before Moving to DONE.md)

A reviewer must verify:

- [ ] **Scaffold Integrity**
  - [ ] The HTML scaffold is present and loads cleanly.
  - [ ] `package.json` defines all required scripts.
  - [ ] `TODO.md` and `DONE.md` exist and are structured for phase-by-phase tracking.

- [ ] **No Scope Creep**
  - [ ] Confirm no gameplay code was added prematurely.
  - [ ] Confirm no UI styling beyond placeholders.
  - [ ] Confirm no test or build framework is active (scripts are placeholders).

- [ ] **Documentation Alignment**
  - [ ] Review against `IMPLEMENTATION_PLAN.md` Phase 1 section.
  - [ ] Confirm all exit criteria are met (see section 6.3).

- [ ] **Readiness for Phase 2**
  - [ ] Confirm `TODO.md` can be updated with Phase 2 tasks.
  - [ ] Confirm `index.html` is ready for board model and render (Phase 2).

### 6.3 Exit Criteria (Must All Be True to Move to DONE.md)

- [ ] The HTML scaffold exists and loads cleanly in a browser.
- [ ] `package.json` contains required `test`, `lint`, and `build` scripts.
- [ ] `TODO.md` and `DONE.md` exist and are structured for tracking.
- [ ] No gameplay code is present in `index.html`.
- [ ] npm scripts all succeed with exit code 0.
- [ ] No console errors appear on page load (only "Game initialized" message).
- [ ] Review confirms no scope creep beyond repository bootstrap.

---

## 7. Code Scaffolding

### 7.1 `index.html` Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minesweeper</title>
    <style>
        /* Phase 1: Minimal CSS Scaffold */
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            background-color: #008080;
            font-family: 'MS Sans Serif', Arial, sans-serif;
            font-size: 16px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        
        #game-window {
            background-color: #C0C0C0;
            border: 2px solid;
            border-color: #FFFFFF #000000 #000000 #FFFFFF;
            padding: 2px;
            min-width: 300px;
            min-height: 300px;
        }
        
        .title-bar {
            background: linear-gradient(to right, #000080, #1084D0);
            color: #FFFFFF;
            padding: 2px 4px;
            font-weight: bold;
            margin-bottom: 2px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .title-bar-close {
            cursor: pointer;
            width: 16px;
            height: 14px;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #C0C0C0;
            color: #000000;
            font-size: 12px;
            border: 1px solid;
            border-color: #FFFFFF #000000 #000000 #FFFFFF;
        }
        
        .menu-bar {
            background-color: #C0C0C0;
            padding: 2px 2px;
            margin-bottom: 2px;
            border: 1px solid;
            border-color: #DFDFDF #808080 #808080 #DFDFDF;
            font-size: 14px;
        }
        
        #game-container {
            padding: 4px;
            display: flex;
            flex-direction: column;
            gap: 4px;
        }
        
        #header-panel {
            background-color: #C0C0C0;
            border: 2px solid;
            border-color: #808080 #FFFFFF #FFFFFF #808080;
            padding: 4px;
            min-height: 50px;
        }
        
        #board-container {
            background-color: #C0C0C0;
            border: 2px solid;
            border-color: #808080 #FFFFFF #FFFFFF #808080;
            padding: 4px;
            min-height: 200px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
    </style>
</head>
<body>
    <div id="game-window">
        <div class="title-bar">
            <span>Minesweeper</span>
            <div class="title-bar-close">✕</div>
        </div>
        <div class="menu-bar">
            Game | Help
        </div>
        <div id="game-container">
            <div id="header-panel"></div>
            <div id="board-container"></div>
        </div>
    </div>

    <script>
        // Phase 1: Game State Initialization (Minimal)
        
        const gameState = {
            initialized: true,
            phase: 1,
        };
        
        function initializeGame() {
            console.log('Game initialized (Phase 1 scaffold)');
        }
        
        // Initialize on page load
        document.addEventListener('DOMContentLoaded', initializeGame);
    </script>
</body>
</html>
```

### 7.2 `package.json` Template

```json
{
  "name": "pxl-sweeper",
  "version": "0.1.0",
  "description": "Win95 Minesweeper clone browser game",
  "main": "index.html",
  "scripts": {
    "test": "echo 'No tests yet' && exit 0",
    "lint": "echo 'No linting configured' && exit 0",
    "build": "echo 'Static build: copy index.html to output' && exit 0"
  },
  "keywords": ["minesweeper", "game", "win95"],
  "author": "PXL",
  "license": "MIT"
}
```

### 7.3 `TODO.md` Template

```markdown
# TODO.md — Phased Work Tracking

## Overview

This file tracks work items across all 9 phases of the Minesweeper implementation.
Items move to `DONE.md` only after verification and review.

---

## Phase 1 — Project Bootstrap and Static App Skeleton

- [ ] Add static app skeleton with a root HTML file and empty game container
- [ ] Add package.json with test, lint, and build scripts
- [ ] Add TODO.md and DONE.md phase-tracking structure

---

## Phase 2 — Board Model and Initial Board Render

Tasks to be populated when phase begins.

---

## Phase 3 — First-Click-Safe Mine Placement and Cell Reveal

Tasks to be populated when phase begins.

---

## Phase 4 — Input Handling and Game State Controls

Tasks to be populated when phase begins.

---

## Phase 5 — Cascade Reveal and Win/Loss Detection

Tasks to be populated when phase begins.

---

## Phase 6 — Header Panel, Timer, Mine Counter, Reset Flow

Tasks to be populated when phase begins.

---

## Phase 7 — Best Times Persistence and Dialogs

Tasks to be populated when phase begins.

---

## Phase 8 — Win95 Visual Styling and Compatibility Polish

Tasks to be populated when phase begins.

---

## Phase 9 — Stabilization, Validation, and Release Readiness

Tasks to be populated when phase begins.
```

### 7.4 `DONE.md` Template

```markdown
# DONE.md — Completed and Verified Work

This file tracks work items that have been implemented, reviewed, and verified to meet requirements.
Items move here from `TODO.md` only after review passes and all exit criteria are satisfied.

---

## Phase 1

(No items completed yet.)

---

## Phase 2+

(To be populated as phases complete.)
```

---

## 8. Integration Notes

### 8.1 How Phase 1 Sets Up Future Phases

| Future Phase | Dependency | Why |
|--------------|-----------|-----|
| Phase 2 (Board model) | `index.html` scaffold, `#board-container` div | Phase 2 will populate the empty `#board-container` with a 16×16 grid. |
| Phase 3+ (Gameplay logic) | `index.html` `<script>` block, `gameState` object | Phases 3+ will add game rules, input handlers, and state management to the existing JS scaffold. |
| Phase 6 (Header panel) | `#header-panel` div in HTML | Phase 6 will add timer, counter, and smiley button to the empty `#header-panel`. |
| Phase 8 (Styling) | Phase 1 CSS baseline | Phase 8 will enhance baseline CSS with Win95 bevels, colours, and visual polish. |
| Phase 9 (Validation) | `TODO.md`, `DONE.md`, `package.json` scripts | Phase 9 will use these artifacts to track final verification and run build/lint commands. |

### 8.2 npm Scripts Placeholder Pattern

The Phase 1 `package.json` uses simple `echo` commands for `test`, `lint`, and `build`:
- These are **placeholders** intentionally.
- They verify that npm can run scripts without errors.
- Future phases may introduce real test runners, linters, or bundlers.
- The current setup is a no-op to avoid premature tooling overhead.

---

## 9. Risk Register

### 9.1 Phase 1–Specific Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|-----------|
| **Scope creep: adding gameplay logic** | High | High | Enforce code review to flag any game rules, input handlers, or state logic. Audit for keywords: "mine", "reveal", "cascade". |
| **npm scripts fail on first run** | Medium | High | Test all npm scripts locally before commit. Ensure exit codes are 0. |
| **HTML doesn't load in browser** | Low | High | Test in multiple browsers during development. Validate HTML syntax. |
| **Centering not working across browsers** | Low | Medium | Test flexbox centering in Chrome, Firefox, Safari, and Edge. Have a CSS fallback. |
| **Package.json is invalid JSON** | Low | High | Use a JSON validator. Run `npm test` to verify npm can parse it. |
| **TODO.md and DONE.md are missing** | Low | High | Confirm both files are created before submitting for review. |

---

## 10. Implementation Checklist (Quick Reference)

Use this checklist to track Phase 1 execution:

1. **Create `package.json`**
   - [ ] File created at root.
   - [ ] Includes `test`, `lint`, `build` scripts.
   - [ ] `npm test` runs without error.

2. **Create `index.html`**
   - [ ] File created at root.
   - [ ] Loads in browser without console errors.
   - [ ] Shows "Game initialized (Phase 1 scaffold)" in console.
   - [ ] Displays centered teal background.
   - [ ] Title bar, menu bar, and placeholder divs are visible.

3. **Create `TODO.md`**
   - [ ] File created at root.
   - [ ] Phase 1 section contains all 3 tasks.
   - [ ] Phases 2–9 stubs are present.

4. **Create `DONE.md`**
   - [ ] File created at root.
   - [ ] Initially empty (or header comments only).

5. **Verify No Gameplay Logic**
   - [ ] No "mine", "reveal", "click" handlers, or board logic in `index.html`.
   - [ ] `gameState` is minimal.

6. **Pre-Review Validation**
   - [ ] All 4 files exist.
   - [ ] All npm scripts pass.
   - [ ] HTML loads cleanly.
   - [ ] No scope creep.

---

## 11. Next Steps After Phase 1

Once Phase 1 is reviewed and moved to `DONE.md`:

1. **Update `TODO.md`** — Populate Phase 2 tasks from `IMPLEMENTATION_PLAN.md`.
2. **Review Phase 2 plan** — Create `IMPLEMENTATION_PHASE2.md` with detailed blueprint for board model and render.
3. **Begin Phase 2 execution** — Follow the same cycle (Plan-Act-Validate).

---

## Appendix: Authoritative References

- **`REQUIREMENTS.md`** — Functional and visual requirements. Phase 1 does not implement any requirements; it prepares the foundation.
- **`IMPLEMENTATION_PLAN.md`** — High-level phase definitions and delivery strategy. Phase 1 section is the authoritative source for this phase's scope.
- **`GEMINI.md`** — Project rules. Phase 1 adheres to the rule: "Keep one TODO.md item small enough for one review cycle."

---

**Document Version**: 1.0  
**Last Updated**: 2026-04-23  
**Next Review Gate**: Upon completion of all three Phase 1 tasks and pre-review validation.
