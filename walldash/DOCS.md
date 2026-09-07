# Walldash Home Assistant Add-on

Walldash brings your house to life in live 3D: the touch-first home automation
dashboard for wall-mounted touchscreens and tablets. Trace your floor plan in the
built-in 2D editor (or import it from Sweet Home 3D), drag and drop your devices
onto it, and control everything from an isometric 3D view. No Blender, no YAML.

## Installation

1. Add this repository to your add-on store (see the repository README).
2. Install **Walldash** and click **Start**. Keep **Start on boot** enabled.
3. No token setup is required: the add-on talks to Home Assistant through the
   Supervisor API automatically.
4. Open the web UI at `http://<home-assistant-ip>:8080` — on your wall tablets
   no Home Assistant login is needed (direct port access, no Ingress).

## Configuration

| Option | Default | Description |
| --- | --- | --- |
| `log_level` | `info` | Verbosity of the add-on logs: `debug`, `info`, `warn`, `error`. |

All data (floor plans, device placements, dashboards) lives in the add-on `/data`
volume and survives updates, reboots, and backups.

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
