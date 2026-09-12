# Walldash

Touch-first 3D home automation dashboard for Home Assistant, built for wall-mounted
touchscreens and tablets. Draw your floor plan (or import it from Sweet Home 3D),
drop your devices onto it, and control your home from a live isometric 3D view —
no Blender, no YAML.

Full documentation: [DOCS.md](DOCS.md). User manual:
[**jlugagne.github.io/walldash**](https://jlugagne.github.io/walldash/).
Application source: [https://github.com/JLugagne/walldash](https://github.com/JLugagne/walldash).

## Configuration

The add-on is configured from the Home Assistant UI (Settings → Add-ons → Walldash →
Configuration):

- `domain` — public hostname used for CORS and same-origin checks, with or without
  `https://` (e.g. `walldash.domain.tld`). Set it when a reverse proxy rewrites the
  `Host` header.
- `token_secret` — optional HS256 signing key for access/refresh tokens. Leave empty to
  auto-generate and persist one in `/data`.
- `secret_key` — optional 32-character key-encryption key. When set, the auto-generated token
  signing key is encrypted at rest (AES-256-GCM), so database copies and Home Assistant snapshots
  do not expose it. Keep it stable and backed up; do not remove it once set.
- `allowed_origins` — optional comma-separated trusted origins for CORS and same-origin
  checks.

See [DOCS.md](DOCS.md) for the full options table.
