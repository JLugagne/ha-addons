# Changelog

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
