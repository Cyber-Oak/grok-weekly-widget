# grok-weekly-widget

A tiny Conky-style desktop widget that shows your Grok weekly usage:
no title bar, pinned above the taskbar, under everything else.

<!-- Add a screenshot: save it as screenshot.png in the repo root and uncomment the line below -->
<!-- ![Grok weekly usage widget](screenshot.png) -->

## What it does

- Reads your Grok CLI login (`~/.grok/auth.json`) and queries the same
  billing endpoint the CLI's `/usage` view uses
- Shows percent used plus the reset date, with a color bar
  (green → amber → red)
- Refreshes every 10 minutes; retries quickly on failure
- `grok-weekly-widget --once` prints one line and exits (handy for scripts)

## Variants

| File | For |
|---|---|
| `grok-weekly-widget` | Full version. KDE/KWin aware: pins itself via KWin scripting, positions above the taskbar using EWMH struts, and auto-refreshes an expired OIDC token. |
| `grok-weekly-widget-pi` | Slim version. Generic EWMH window hints, no KWin scripting, no token auto-refresh (it tells you to re-run `grok`). Tested on Raspberry Pi OS. |

Pick the one that matches your setup. Both are single Python files with
no dependencies beyond the standard library and Tkinter.

## Requirements

- Python 3 (standard library only — no pip packages)
- Tkinter (`python3-tk` on Debian/Raspberry Pi OS, usually bundled elsewhere)
- Grok CLI installed and signed in (`grok` login)
- X11 or XWayland. The full variant targets KDE/KWin; the slim variant
  works on any EWMH-compliant window manager.

## Install

```sh
cp grok-weekly-widget ~/.local/bin/
chmod +x ~/.local/bin/grok-weekly-widget
cp grok-weekly-widget.desktop ~/.config/autostart/
```

Log out and back in, or just run `grok-weekly-widget` to try it now.
On KDE, the optional [KWin window rule](kwin-window-rule.md) makes KWin
enforce the widget behavior persistently.

## Security notes

- Your token never leaves `~/.grok/auth.json` (mode `0600`). The widget
  reads it at runtime and never logs or prints it.
- HTTP redirects are refused outright, so the bearer token can't leak to
  a redirected host.
- Token refreshes (full variant) are written back atomically with `0600`
  permissions.
- The only network destinations are Grok's own API endpoints.

## Caveat

This uses xAI's unofficial billing endpoint
(`cli-chat-proxy.grok.com/v1/billing`). If xAI changes it, the widget
breaks until updated. It also costs no weekly usage itself — it's just
a handful of HTTPS requests.

## License

MIT — see [LICENSE](LICENSE).
