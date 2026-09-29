# AGENTS.md — pod-waybar

Standalone candy repo for the `waybar` candy — the Waybar status bar and Sway
auto-tiling for the Sway desktop. The candy lives in `charly.yml` at the repo
root plus its launcher, helper, config, and style.

Canonical files:

- `charly.yml` — the `waybar:` candy entity (description, `require`, `distro`,
  `service`, `plan`) and its `skill:` entity.
- `waybar-wrapper`, `sway-autotile` — the launcher and helper copied into
  `~/.local/bin`.
- `config.json`, `style.css` — the staged `~/.config/waybar` config and theme.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:waybar` — the owning skill: the candy properties, the module
  layout, and the unified config shared with `waybar-labwc`. Load before editing,
  building, deploying, or troubleshooting this candy.
- `/charly-selkies:sway` — the compositor dependency.
- `/charly-selkies:swaync` — the notification daemon (notification bell module).
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `copy:` / `check:`, service declarations, the
  `wait_for:` lowering).
- `/charly-check:check` — the check/R10 framework (`charly check box`,
  `charly check run <bed>`).
- `/charly-pod:pod` — the `kind: pod` / deploy schema reference (this candy is
  composed into a box; services).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The candy's own `check:` steps assert `/usr/bin/waybar`, the `waybar` package,
  the two launchers, the staged config and style, and the `wait_for:` lowering in
  `/etc/supervisord.conf`.

## Modify this repo

- Edit the `waybar:` candy entity in `charly.yml`; the `skill:` entity in the
  same file is the owning skill's source — a candy change and its skill change
  land together.
- The `waybar` service uses the `wait_for:` socket lowering; do not reintroduce a
  hand-rolled wait loop in `waybar-wrapper` — the `ELAPSED` `not_contains` check
  asserts the loop is gone.
- `config.json` / `style.css` are the shared unified config (also used by
  `waybar-labwc`); keep them in step across the two candies.
- The `skill:` entity is the source for `/charly-selkies:waybar`; never edit the
  generated `SKILL.md` — regenerate it.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
