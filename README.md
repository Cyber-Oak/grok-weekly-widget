# grok-weekly-widget

A tiny Conky-style desktop widget that shows your Grok weekly usage, with
an optional companion card for Claude Code usage stacked right above it:
no title bar, pinned above the taskbar, under everything else.

<!-- Add a screenshot: save it as screenshot.png in the repo root and uncomment the line below -->
<!-- ![Grok and Claude usage widgets](screenshot.png) -->

## What it does

- Reads your Grok CLI login (`~/.grok/auth.json`) and queries the same
  billing endpoint the CLI's `/usage` view uses
- Shows percent used plus the reset date, with a color bar
  (green → amber → red)
- **Claude usage (new):** `claude-usage-widget` reads your Claude Code login
  (`~/.claude/.credentials.json`) and shows both the 5-hour session window
  and the 7-day window, each with its own bar and reset time. It finds the
  Grok window and sits directly above it, same width and right edge, so
  the two read as one column.
- Refreshes every 10 minutes; retries quickly on failure (and backs off
  on rate limits)
- `--once` prints the usage text and exits (handy for scripts)

## The look

The slim Grok card and the Claude card share one design: a dark,
slightly translucent card with a hairline border, a small colored dot and
all-caps title (blue for Grok, orange for Claude), a "last updated" time
in the corner, and thin rounded bars. Errors show inline in a soft red
in place of the bars. Nunito Sans is used if installed, otherwise
DejaVu Sans or your default font.

## Variants

| File | For |
|---|---|
| `grok-weekly-widget` | Full version. KDE/KWin aware: pins itself via KWin scripting, positions above the taskbar using EWMH struts, and auto-refreshes an expired OIDC token. |
| `grok-weekly-widget-pi` | Slim version, with the new look. Generic EWMH window hints, no KWin scripting. Auto-refreshes an expired token under the same lock the Grok CLI uses. Tested on Raspberry Pi OS. |
| `claude-usage-widget` | Claude Code usage card. Same generic EWMH approach and look as the slim version; stacks above whichever Grok widget is running (or reserves its spot if none is). |

Pick the Grok variant that matches your setup, and add the Claude card if
you use Claude Code. All are single Python files with no dependencies
beyond the standard library and Tkinter.

## Requirements

- Python 3 (standard library only — no pip packages)
- Tkinter (`python3-tk` on Debian/Raspberry Pi OS, usually bundled elsewhere)
- Grok CLI installed and signed in (`grok` login)
- For the Claude card: Claude Code installed and signed in with a
  Claude.ai account (`claude` login)
- X11 or XWayland. The full variant targets KDE/KWin; the slim variant
  and the Claude card work on any EWMH-compliant window manager.

## Install

```sh
cp grok-weekly-widget ~/.local/bin/
chmod +x ~/.local/bin/grok-weekly-widget
cp grok-weekly-widget.desktop ~/.config/autostart/
```

Using the slim variant? Install it under the same name:
`cp grok-weekly-widget-pi ~/.local/bin/grok-weekly-widget`.

For the Claude card:

```sh
cp claude-usage-widget ~/.local/bin/
chmod +x ~/.local/bin/claude-usage-widget
cp claude-usage-widget.desktop ~/.config/autostart/
```

Log out and back in, or just run `grok-weekly-widget` (and
`claude-usage-widget`) to try it now. On KDE, the optional
[KWin window rule](kwin-window-rule.md) makes KWin enforce the widget
behavior persistently.

## Security notes

- Your tokens never leave their login files (`~/.grok/auth.json`,
  `~/.claude/.credentials.json`, both mode `0600`). The widgets read
  them at runtime and never log or print them.
- HTTP redirects are refused outright, so a bearer token can't leak to
  a redirected host.
- Token refreshes are done under the same lock the matching CLI uses (so
  a one-time refresh token is never spent twice) and written back
  atomically with `0600` permissions.
- The only network destinations are Grok's own API and auth endpoints,
  and Anthropic's (`api.anthropic.com`, `platform.claude.com`) for the
  Claude card.

## Caveat

This uses xAI's unofficial billing endpoint
(`cli-chat-proxy.grok.com/v1/billing`), and the Claude card uses the
undocumented usage endpoint Claude Code's `/usage` view calls
(`api.anthropic.com/api/oauth/usage`). If either changes, that widget
breaks until updated. Neither costs any usage itself — it's just a
handful of HTTPS requests.

## License

MIT — see [LICENSE](LICENSE).
