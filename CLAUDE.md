# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Pokémon Set Tracker — a single-page web app for tracking owned Pokémon TCG cards. Hosted via GitHub Pages at `fkt3.github.io`. The entire application lives in a single `index.html` file with inline CSS and JavaScript (no build step, no dependencies, no package manager).

## Development

Open `index.html` directly in a browser. There is no build, lint, or test pipeline. To deploy, push to `main` — GitHub Pages serves the file automatically.

## Architecture

Everything is in `index.html` (~900 lines): `<style>` block → HTML markup → `<script>` block.

### Data Layer
- **API**: TCGdex REST API (`https://api.tcgdex.net/v2/en`) — fetches set lists and individual set card data
- **Storage**: `localStorage` with key `pokemon_tcg_collection` — stores `{ cardId: timestamp }` map
- **Caching**: `setCardsCache` object holds fetched set details in memory to avoid repeat API calls

### Key Globals & Functions
- `allSets` — array of all sets from API, loaded once at init
- `render()` — rebuilds the set grid (applies search, sort, grid filter)
- `openSet(id)` / `renderModal(set)` — modal showing individual cards in a set
- `toggleCard(cardId)` — marks/unmarks a card as owned, updates UI incrementally
- `updateGlobalStats()` — refreshes the top-level stats banner (cards owned, sets started, sets complete)
- `updateSetCard(setId)` — updates a single set card in the grid without full re-render
- `exportCollection()` / `importCollection()` — JSON export/import of collection data
- `exportProgressImage()` / `exportMissingImage(setId)` — canvas-based image generation for sharing

### UI Structure
- **Header**: logo, title, stats banner (cards owned, sets started, sets complete)
- **Controls**: search input, filter buttons (All/Started/Complete), sort toggle (A-Z)
- **Grid**: set cards with progress bars, click to open modal
- **Modal**: card grid for a set, toggle ownership by clicking, filter (All/Owned/Missing), Check All, Share Missing
- **Theme**: dark/light toggle stored in `localStorage` as `pokemon_tcg_theme`, applied via `.light` class on `<html>`

### CSS Theming
CSS variables on `:root` (dark) and `:root.light` (light) control all colors. Key variables: `--bg`, `--surface`, `--border`, `--text`, `--accent`, `--green`.
