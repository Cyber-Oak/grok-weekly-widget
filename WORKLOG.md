# WORKLOG — grok-weekly-widget

Shared log for Claude sessions on **infinity** (desktop) and **forge** (ThinkPad). Read before starting work; append an entry before finishing. Newest entries at the bottom.

---

## 2026-10-16 08:53 EDT — infinity

**What this project is:** Small borderless desktop widgets showing usage quotas: `grok-weekly-widget` (full, KDE/KWin-aware), `grok-weekly-widget-pi` (slim, generic EWMH, tested on Raspberry Pi OS), and `claude-usage-widget` (Claude Code 5-hour/7-day usage card that stacks above the Grok widget). Includes .desktop autostart entries and a KWin window-rule guide.

**What I did:** Created this WORKLOG.md as part of the cross-machine setup. No code changes.

**Why:** Shared memory for Claude on both machines under the worklog protocol in ~/.claude/CLAUDE.md (synced via claudedot → Cyber-Oak/claude-sync).

**Current state (as found):** Freshly cloned to ~/Projects/grok-weekly-widget today; clean on `main`, in sync with origin; 8 commits. Latest: rename CUn() → split_scopes(). Not installed or run on infinity.

**Cross-machine setup today:** Repo syncs via GitHub. ~/Workbench syncs via Syncthing. Claude config via claude-sync; history via ccsync (being set up).

**Loose ends / next steps:** The widgets read local CLI login files; never commit credentials or sample auth files. Omarchy uses Hyprland, not KWin — the KWin-specific variant/positioning would need adapting if run on infinity or forge.
