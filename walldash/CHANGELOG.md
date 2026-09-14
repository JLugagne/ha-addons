# Changelog

## 1.4.7
- Fixed the floor view on touch devices: switching floors could leave a blank canvas with the device badges piled in a corner, because the 3D scene only re-rendered on camera changes. The scene now re-renders whenever the floor, devices or layer change, and the camera fit always includes device placements even when the plan has no walls or zones.
- If the browser refuses or later drops the WebGL context (for example Brave Shields fingerprinting protection or a phone GPU over its limits), the floor view now shows an explanatory message with a reload button instead of a blank canvas.

## 1.4.6
- Fixed a case where promoting a device (for example **device → admin**) could sign the promoted device out: a promotion no longer revokes the access token, so the device keeps its session and picks up the new permissions on its next action. Lowering a role, revoking a device and logging out still sign out immediately.

## 1.4.5
- Security hardening across sessions, snapshots and imports.
- Revocations now survive restarts: a logout, a demoted or revoked device stays signed out after the add-on restarts, instead of its unexpired access token being accepted again for up to 15 minutes.
- Privileged actions (approving a device, creating or revoking an invitation, revoking a device, changing a role) re-check the account in the database, so a device whose access was revoked or lowered can no longer use an old token to perform them.
- Cross-site request protection compares the whole origin (scheme, host and port), not just the host, so a page served over plain HTTP on the same host cannot act on an HTTPS deployment.
- Restored backups are validated as a whole: a snapshot that references missing or mismatched levels or dashboards is refused instead of being imported into a broken state.
- Level and dashboard names are length-limited, and background images must be one of the bundled backgrounds.
- Fixed concurrent plan imports (Sweet Home 3D and AI-JSON): two simultaneous imports can no longer share wall, opening or zone identifiers.
- API responses are no longer cacheable, security headers also cover the add-on's own redirects, and websocket errors no longer expose the Home Assistant URL.
- A placement can no longer be saved against another level.

## 1.4.4
- Promoting a device (for example **device → admin**) no longer signs it out: the session and its live connection stay, and only the access token is re-issued so the new permissions take effect.
- Lowering a device's role still signs it out immediately, so reduced access can never be outlived by an already-issued token.

## 1.4.3
- Keep devices signed in: an automatic token refresh no longer downgrades the 60-day "remember me" cookie to a session cookie. Kiosk browsers and WebViews that clear session cookies when they are recycled no longer log the device out after a period of inactivity.

## 1.4.2
- Refresh-token resilience: the token store now honors the configured reuse-grace window instead of a hardcoded 10 seconds, so a device replaying a just-rotated token after waking is no longer mistaken for token theft and signed out.
- Access panel: revoked devices are hidden by default; untick **Hide revoked** to list them.
- Sign-in screen: removed the manual invitation-token field. Approvals need no code, and invitations open from the link an owner shares.
- Refreshed user-manual screenshots and wording.

## 1.4.1
- Security hardening: admins can no longer revoke, rename or change an owner, and the last owner is protected.
- Revoking a device (or changing its role) now invalidates its refresh and access tokens and closes its live WebSockets immediately; the WebSocket hub re-validates the account before every action.
- JSON request bodies are capped at 1 MiB (32/64 MiB for restore and plan imports) and over-limit bodies return 413.
- Removed the unused CSRF-token endpoint and its in-memory store.
- The auto-generated signing key is sealed at rest with the `secret_key` option, or (when unset) a generated key-encryption key file next to the database (`/data/walldash.db.kek`, mode `0600`); full `/data` snapshots are credential-grade material. For protection against snapshots that leave the host, set `token_secret` or point the new `secret_key_file` option at a path excluded from backups.
- Overflow-safe widget layout validation, strict explicit-scheme CORS matching, and govulncheck pinned in CI.

## 1.4.0
- New device access model: the first device to open Walldash becomes the owner automatically (no one-time code to read from the logs), additional devices are approved from Setup → Access or enrolled with a single-use 15-minute invitation link, and a `rescue_mode` option recovers access when the owner device is lost.
- One-time codes are no longer written to the add-on log.

## 1.3.4
- Fix spurious logouts: a refresh token replayed shortly after a burst of reconnects (phone waking, background retry) is treated as benign concurrency instead of token theft. The reuse grace window is now 30 seconds.

## 1.3.3
- The Access panel device list and the top-bar pending indicator refresh the session automatically when the access token has expired, instead of showing an empty list.
- Documentation: the user manual now documents every setting in detail.

## 1.3.2
- The one-time enrollment code is written to the add-on log again (`otp_issued`), restoring the bootstrap channel for the first device.

## 1.3.1
- Security: the auto-generated token signing key can be encrypted at rest with the new optional `secret_key` option (AES-256-GCM), so database copies and Home Assistant snapshots no longer expose it. Keep it stable and backed up; removing it after it has been set fails closed.
- The container runs as an unprivileged user; base images are pinned by digest.

## 1.3.0
- Security hardening: every state-changing management endpoint now requires the owner/admin scope; a standard device keeps read access and the action route only.
- Logout and device revocation now invalidate already-issued access tokens immediately.
- Strict same-origin checks, security headers (CSP, X-Frame-Options, HSTS and more), and CORS no longer reflects arbitrary origins.
- Request bodies and `.sh3d` import are size-bounded; Home Assistant entity IDs are validated.

## 1.2.2
- Bump the `egauth` dependency to v0.13.0. No user-visible change.

## 1.2.1
- A shield button in the top bar (owner/admin) opens **Setup → Access** directly and badges the number of pending enrollment requests.
- Devices can be renamed from the Access table (the default label is the User-Agent).

## 1.2.0
- Per-device sign-in: each device is an anonymous account enrolled with a single-use one-time code (OTP, 15 minutes) shown in the add-on log (`otp_issued`) and in **Setup → Access**. No Home Assistant login on tablets.
- The first device becomes `owner`; owner/admin can list devices, change roles and revoke any device. Access tokens live 15 minutes, refresh tokens 60 days with rotation.
- New options: `domain` (public hostname used for CORS/same-origin), `allowed_origins`, and `token_secret`.
- HTTPS is required for the authentication cookies; reach the add-on through a TLS reverse proxy.

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
