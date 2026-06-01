# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Single-file HTML project lead tracker app. All HTML, CSS, and JavaScript is in `lead_tracker_v4_6.html`. No build tools, no frameworks, no package.json. Open directly in a browser.

## Run

Open `lead_tracker_v4_6.html` in any modern browser. Data persists in localStorage and IndexedDB.

## Architecture

- **Single self-contained file** (`lead_tracker_v4_6.html`):
  - `<style>`: Design system using CSS custom properties (--surface-1, --accent, --border, etc.)
  - 5 embedded library blocks: XLSX (SheetJS), Chart.js v4.4.1, ECharts, plus init + app JS
  - Everything is driven by `window.addEventListener('DOMContentLoaded', ...)`
- **Views**: Projects (card), Table, Dashboard (charts), Heatmap (map), Kanban (drag/drop pipeline)
- **Data**: `let projects = []` — in-memory, synced to IndexedDB + localStorage
- **No backend**, no API calls — fully offline capable with OneDrive file sync option

## Key libraries (embedded inline)

- **Chart.js v4.4.1** — dashboard charts (pie, doughnut, line, bar)
- **ECharts** — China heatmap (embedded GeoJSON for provinces)
- **XLSX (SheetJS)** — Excel import/export (no FileSaver needed)

## Event delegation pattern used for action buttons

Projects and Table views use event delegation on parent containers (`#projectsList`, `#tableBody`) rather than inline `onclick`. Buttons carry `data-project-id` attributes and class names like `edit-btn`, `delete-btn`, `history-btn`.

- `handleProjectListClick(e)` — delegated handler for Projects view
- `handleTableButtonClick(e)` — delegated handler for Table view
- `openEditModal(projectId)` — always opens the edit modal; switches to Projects tab best-effort via `safeSwitchTab('projects')`
- `deleteProject(projectId)` — confirms via `confirmModal()`, then deletes
- `showFullHistory(projectId)` — opens history modal
- `togglePriority(projectId)` — toggles the `isPriority` boolean on the project

No inline `onclick` on action buttons — relies solely on delegation + `target.closest(...)`.

## Design system (CSS custom properties)

Defined in `:root` and overridden in `body.dark-mode`:
- `--surface-1/2/3` — surface hierarchy
- `--accent` — EDB corporate blue `#132b66`
- `--border`, `--text-primary`, `--text-muted`, `--text-faint`
- `--radius-sm/md/lg`, `--font`, `--font-mono`
- Status colors: `--success`, `--warning`, `--danger`, `--info`, `--teal`, `--orange`
