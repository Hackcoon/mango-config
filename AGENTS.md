# AGENTS.md — instructions for any AI working on fury's mango setup

Follow this file every time. Short version: any bind change = live config +
`mango-ultimate-hotkeys.md` + commit + push. Never leave one behind.

## Repos and paths

| What | Path | GitHub |
|---|---|---|
| This repo (mango config + docs) | `~/mango-config` | `Hackcoon/mango-config` |
| Live mango config (source of truth) | `~/.config/mango/config.conf` (+ `media.conf`, `dms/`) | — (synced into this repo at `mango/`) |
| Living hotkey sheet (canonical) | `~/nixos-backups/mango-ultimate-hotkeys.md` | mirrored in this repo as `mango-ultimate-hotkeys.md` |
| NixOS system config | `/etc/nixos` | `Hackcoon/fury-nixOS` |

`~/mango-config/mango/config.conf` must always match `~/.config/mango/config.conf`.
`~/mango-config/mango-ultimate-hotkeys.md` must always match
`~/nixos-backups/mango-ultimate-hotkeys.md`. Sync with `cp`, never hand-edit
two copies independently.

## Workflow for any keybind change

1. Edit the LIVE config first: `~/.config/mango/config.conf`.
2. Validate: `mango -p -c ~/.config/mango/config.conf` → exit 0, no output.
   Mango hot-reloads, so a valid edit is live immediately.
3. Update the living sheet `~/nixos-backups/mango-ultimate-hotkeys.md`:
   - Tables (DMS shell / Apps / Layouts / Windows / Screenshots / Tags / Monitors).
   - `Last updated` line at top: `YYYY-MM-DD (what changed)`.
   - `Changelog` entry at bottom.
   - `Appendix` snapshot: paste the changed bind block so it mirrors the config.
4. Sync into this repo:
   ```bash
   cp ~/.config/mango/config.conf mango/config.conf
   cp ~/nixos-backups/mango-ultimate-hotkeys.md mango-ultimate-hotkeys.md
   ```
5. Commit and push — do this unprompted, fury wants GitHub kept in sync:
   ```bash
   git add mango/config.conf mango-ultimate-hotkeys.md
   git commit -m "<imperative summary, e.g. Add SUPER+SHIFT+V neovim in Kitty>"
   git push
   ```
6. If a NixOS package or module changed too (`/etc/nixos`), verify with
   `nixos-rebuild build --flake /etc/nixos#nixos`, then commit + push that
   repo the same way. Never run `sudo nixos-rebuild switch` — no sudo in AI
   sessions; fury runs it himself, then reports back.

## Hard rules (learned the painful way)

- `#` comments start at column 1. NEVER inline `#` on a bind line — mango
  glues it onto the last arg and breaks the bind.
- Keep the desc comment above each bind under ~30 chars — DMS renders that
  last comment line on `SUPER+H`. Details: `CHEATSHEET-AI-GUIDE.md`.
- Media keys stay indented (one space) in `media.conf` — hidden from the
  sheet by design, still functional. Never use `dms/binds.conf` (empty slot).
- Installed mango is 0.16.2; the docs site tracks dev main. Verify every new
  dispatcher against the binary (`mango -p` rejects unknowns), not the web.
- Before picking a key, grep the live config for conflicts across ALL
  modifiers (`SUPER`, `SUPER+SHIFT`, `SUPER+CTRL`, `SUPER+ALT`).
- Use `git mv` for renames/moves so history is preserved.
- Commit style: short imperative (`Add ...`, `Fix ...`, `Move ...`, `Rename ...`).
- No spaces-free renames needed, but prefer clear names; archive stale docs
  under `old markdown files/` instead of deleting.
- This shell has no sudo TTY. Any command needing sudo must be handed to
  fury as a copy-paste block, not executed.
