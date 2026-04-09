# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-file Korean shopping list app (`index.html`) backed by Supabase for cloud persistence.

## Running the App

The app must be served over HTTP — opening `index.html` directly via `file://` will cause CORS errors from Supabase.

```bash
python -m http.server 8080
# then open http://localhost:8080
```

## Architecture

Everything lives in `index.html` as a self-contained file with inline CSS and a `<script type="module">` block.

**Supabase integration** uses the REST API directly via `fetch` — no SDK. All DB operations are in four functions at the bottom of the script:

| Function | HTTP Method | Purpose |
|---|---|---|
| `fetchItems()` | GET | Load all items, ordered by `created_at desc` |
| `addItem()` | POST | Insert new row `{ text, done: false }` |
| `toggle(id, done)` | PATCH | Update `done` field for a single row |
| `remove(id)` | DELETE | Delete a single row by `id` |
| `clearChecked()` | DELETE | Delete all rows where `done = true` |

**Supabase project:** `copnojhwbjwcxxxdzkzi` (region: ap-northeast-1)
**Table:** `shopping_items` — columns: `id` (uuid PK), `text` (text), `done` (boolean), `created_at` (timestamptz)
**RLS:** enabled with a permissive "Allow all" policy (no auth required)

## GitHub

Repository: https://github.com/HotGran/shopping-list
