# Walldash Home Assistant Add-on

Walldash brings your house to life in live 3D: the touch-first home automation
dashboard for wall-mounted touchscreens and tablets. Trace your floor plan in the
built-in 2D editor (or import it from Sweet Home 3D), drag and drop your devices
onto it, and control everything from an isometric 3D view. No Blender, no YAML.

> 📖 **Full user manual:** <https://jlugagne.github.io/walldash/> — installation,
> onboarding, the 3D views, the 2D editor and the dashboards.

## Installation

1. Add this repository to your add-on store (see the repository README).
2. Install **Walldash** and click **Start**. Keep **Start on boot** enabled.
3. No token setup is required: the add-on talks to Home Assistant through the
   Supervisor API automatically.
4. Open the web UI at `http://<home-assistant-ip>:8080` — on your wall tablets
   no Home Assistant login is needed (direct port access, no Ingress). The first
   device that opens it becomes the **owner** automatically; add further devices
   from **Setup → Access** by approving them or sending an invitation link.

## Configuration

| Option | Default | Description |
| --- | --- | --- |
| `log_level` | `info` | Verbosity of the add-on logs: `debug`, `info`, `warn`, `error`. |
| `domain` | _(empty)_ | Public hostname used for CORS and same-origin checks, with or without `https://` (e.g. `walldash.domain.tld`). Set it when a reverse proxy rewrites the `Host` header. |
| `token_secret` | _(auto-generated)_ | Optional HS256 signing key for access/refresh tokens (at least 32 bytes). Leave empty to auto-generate and persist one in `/data`; keep it stable, changing it invalidates all sessions. |
| `secret_key` | _(empty)_ | Optional 32-byte key-encryption key that seals the stored token signing key. Because the options file and the database share the `/data` volume, this option alone does not protect against full `/data` snapshots; prefer `secret_key_file`. Keep it stable: removing it after use fails closed. |
| `secret_key_file` | _(empty)_ | Path to a file containing the key-encryption key (whitespace-trimmed, exactly 32 bytes), read when `secret_key` is empty. Point it at an operator-mounted path excluded from backups. An unreadable or empty file aborts startup. |
| `allowed_origins` | _(empty)_ | Optional comma-separated trusted origins for CORS and same-origin checks, needed when a reverse proxy rewrites the `Host` header. Accepts `https://host` or a bare `host`. |
| `rescue_mode` | `false` | Recovery switch. When enabled, the next device that opens Walldash claims the owner role, even if other devices already exist. Enable it only to recover from a lost owner device, then set it back to `false`; it is consumed after a single use. |

All data (floor plans, device placements, dashboards) lives in the add-on `/data`
volume and survives updates, reboots, and backups.

## Device access

- **First device**: when no device has enrolled yet, the first one to open Walldash
  becomes the **owner** automatically — no code to read anywhere.
- **More devices**: from **Setup → Access**, either approve a device that is waiting,
  or create a single-use **invitation** (valid for 15 minutes) and share its link with
  the new device.
- **Lost owner device**: enable the `rescue_mode` option, restart the add-on, then
  open Walldash on the device that should become the new owner. Set `rescue_mode`
  back to `false` afterwards; it is consumed after that single use.
- **Revoking or demoting a device**: revoking a device, or lowering its role, signs it out
  immediately — its refresh tokens are cleared, its access token is rejected and its
  open live connections are closed. The rejection is persisted and replayed at startup,
  so it survives an add-on restart. Promoting a device keeps it signed in. Revoked
  devices are hidden in the device list by default — untick **Hide revoked** to show them.

## Usage tips

- **Wall tablets**: point a kiosk browser (e.g. Fully Kiosk) at
  `http://<home-assistant-ip>:8080`. The interface is designed for touch.
- **Import**: use the in-app plan import for Sweet Home 3D (`.sh3d`) files to
  skip manual tracing.
- **Layers**: use Display Layers (e.g. `controls`, `sensors`) to filter what each
  tablet shows in 3D.

## Updating

Update from the add-on store like any other add-on. Your data in `/data` is kept.
If an update is marked breaking, it will require a manual update — read the
changelog first.

## Uninstallation

Uninstalling removes the container; the `/data` volume (and its backups, if you
take them) is managed by the Supervisor. Export anything you need first.

## Troubleshooting

- **UI unreachable**: check the add-on is started and port `8080` is not used by
  another add-on (change the host port in the add-on network settings).
- **Devices missing or actions failing**: check the add-on logs (Supervisor API
  connectivity) and that your entities use supported domains.
- **Verbose logs**: set `log_level` to `debug` and restart the add-on.
- **Still stuck**: open an issue at
  [https://github.com/JLugagne/walldash](https://github.com/JLugagne/walldash)
  with the relevant log lines.
