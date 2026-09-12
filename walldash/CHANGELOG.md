# Changelog

## 1.1.1
- Plan editor: devices can now be dragged from the palette onto the plan with a finger on an iPad; the crosshair fallback is always visible.

## 1.1.0
- Phone layout: below 640px a dashboard becomes a vertical two-column flow instead of the fixed grid, ordered like the tablet layout; the top bar compacts.
- Per-dashboard grid size: Setup → Dashboards gains a **Grid** editor (columns × rows), stored per dashboard.
- 3D view: drag vertically to tilt the scene, so touch tablets (iPad) can rotate it without a mouse wheel.
- Plan editor: leaving with unsaved changes now asks for confirmation.

## 1.0.0
- Unified switch widgets: device toggles and automation switches now share one design.
- 1×1 switch tiles are a full-tile exclusive ON/OFF button; wider tiles keep the reading and toggle track.
- Removed the sensor sparklines (number, bar and arc widgets) for a cleaner dashboard.
- User manual published at <https://jlugagne.github.io/walldash/>.

## 0.5.0
 - Overviews are now Dashboards — across the UI, the routes (/dashboards), the API (/api/dashboards) and the database (the overview_dashboards table is renamed to dashboards by migration 010). Existing installations migrate automatically.
 - Display-only dashboard view — the everyday dashboard is read-only. A dashboard switcher appears in the top bar as soon as you have more than one, so you can flip between them.
 - Editing lives in Setup — Setup → Dashboards opens the editor for the dashboard you are viewing: rename, delete, add widgets, edit layout, and set a background. Each dashboard keeps its own picture.
 - Contextual Setup button — on a floor it opens that floor's 2D editor; on the Dashboards view it opens the current dashboard's editor. Exit setup returns you to the view, reopening the dashboard you were editing.
 - No more admin mode — editing is a deliberate, labelled mode rather than a hidden toggle.

## 0.4.0
- New 3D house overview: all floors stacked in one scene, opened by default when the app launches.
- Lamps appear in the overview at mid-room height, follow Home Assistant state live, and toggle on tap.
- The overview remembers your camera position and zoom between visits.
- Align floors horizontally/vertically and toggle their visibility from the 2D editor.
- Shared level/house selector across the floor and overview views, draggable zone labels, and an opaque wall option.
- Faster frontend builds (native TypeScript compiler).

## 0.3.3
- Fix some UI issues in the onboarding process

## 0.3.2
- Improve setup process by importing all the floors from sh3d files
- Fix some UI issues in editor mode

## 0.3.1

- Fix device listing via the Supervisor API (duplicated `/api` path segment).
- Log the resolved Home Assistant source/host at startup (no secrets).

## 0.3.0

- First add-on release: pre-built multi-arch images (`amd64`, `aarch64`).
- Supervisor API auto-configuration (no manual token needed).
- Persistent database under `/data`.
