---
name: verify
description: How to build, launch, and drive this app to verify changes at runtime (frontend + backend dev servers, browser flows worth exercising).
---

# Verifying changes in this repo

## Launch

Both servers must run unsandboxed (port binding fails with EPERM in the sandbox):

```bash
# Backend (FastAPI, port 8001) — run in background
cd server && uv run python main.py

# Frontend (Vite, port 3000) — run in background
cd client && npm install && npm run dev
```

Smoke: `curl -s localhost:8001/api/dashboard/summary | head -c 200` and open `http://localhost:3000` in the browser (Claude in Chrome). A production build check is `cd client && npx vite build`.

## Flows worth driving

- **Filter round-trip**: change Location/Category/Order Status in the top bar, confirm a `GET /api/...` request fires with matching query params (read_network_requests) and the tables/KPIs update. Inventory has no month filter.
- **Navigation**: click each of the 6 sidebar links; active state should follow the route.
- **Sidebar collapse** (>1160px): bottom chevron toggles 240px ↔ 64px icon rail with "C" monogram.
- **Language switch**: globe menu → 日本語; all nav labels, page titles, and modals should translate. Filters persist across the switch.
- **Modals**: profile menu (top right) → Profile Details / My Tasks. Modals have a ~0.2s fade; wait 1s after clicking Close before screenshotting or the capture catches the transition and looks stuck open.

## Gotchas

- `client/src/api.js` defines `/api/tasks` and `/api/purchase-orders` methods with no backend routes — 404s there are pre-existing, not your regression. TasksModal works via mock data.
- The Chrome extension cannot actually resize the window; verify <900px drawer and 901–1160px forced-collapse breakpoints by reading the media queries in `client/src/App.vue` or resizing manually.
- Restore state when done: reset-filters icon (right of the filter selects) and switch language back to English.
