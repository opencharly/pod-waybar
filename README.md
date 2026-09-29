# pod-waybar

The `waybar` candy of the OpenCharly candy library, as a standalone repo (the
candy de-submodule cutover, kind-prefixed naming). It provides the Waybar status
bar and Sway auto-tiling for the Sway desktop.

## What it provides

Installs the `waybar` package, stages the `waybar-wrapper` launcher and the
`sway-autotile` helper into the user's `~/.local/bin`, and stages the shared
Catppuccin Mocha waybar config + stylesheet under `~/.config/waybar`. Two
supervisord services then run the bar and the auto-tiler against the Sway Wayland
session.

| Property | Value |
|---|---|
| Requires | `pod-sway` |
| Services | `waybar` (priority 15), `sway-autotile` (priority 16) |
| Install files | `waybar-wrapper`, `sway-autotile`, `config.json`, `style.css` |
| Package | `waybar` (RPM) |

The `waybar` service uses the `wait_for:` lowering to block on the compositor
socket before exec — a supervised wait with a 30s budget that fails the service
on timeout instead of looking like a waybar crash.

## How to use it

Part of the `sway-desktop` composition:

```yaml
my-desktop:
  candy:
    - '@github.com/opencharly/pod-waybar:<tag>'
```

## Verification

The candy's `check:` plan asserts `/usr/bin/waybar`, the `waybar` package, the
`waybar-wrapper` and `sway-autotile` launchers, the staged config and stylesheet,
and the `wait_for:` lowering in `/etc/supervisord.conf` (with the wrapper's
hand-rolled wait removed).

## Layout

- `charly.yml` — the `waybar:` candy entity (description, `require`, `distro`,
  `service`, `plan`) plus its `skill:` entity.
- `waybar-wrapper`, `sway-autotile`, `config.json`, `style.css` — the staged
  launcher, helper, config, and style.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:waybar` — the candy properties, the module
  layout, and the unified config shared with `waybar-labwc`.
- `/charly-selkies:sway` — the compositor dependency.
- `/charly-selkies:swaync` — the notification daemon (notification bell module).
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
