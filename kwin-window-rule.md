# KWin window rule for the Grok usage widget

The widget already sets most of this itself at runtime (below other
windows, off the taskbar/pager/Alt-Tab switcher, no decorations), but a
KWin window rule makes KWin enforce it persistently. Do **not** copy a
`kwinrulesrc` file over yours — that would wipe your existing rules.
Add it through the GUI instead.

## Add via System Settings

1. Open **System Settings → Window Management → Window Rules**.
2. Click **Add New…**.
3. Under **Window matching**, set:
   - **Window title**: `Grok usage` — *Substring match*
4. Under **Appearance & Fixes**, enable and force these:
   - **No titlebar and frame**: Force Yes
   - **Keep below other windows**: Force Yes
   - **Skip taskbar**: Force Yes
   - **Skip pager**: Force Yes
   - **Skip switcher**: Force Yes
   - **Accept focus**: Force No
   - **Minimizable**: Force No
   - **Closable**: Force No
5. Click **OK**, then **Apply**.

## Reference values

These are the raw rule properties, for anyone who prefers to hand-edit
`~/.config/kwinrulesrc` (back it up first — KWin generates a unique ID
per rule, so do not reuse someone else's):

- title = `Grok usage` (substring match)
- wmclass substring = `grok` (the widget sets `grok-weekly-widget` /
  `GrokWeeklyWidget` as its class hints)
- skiptaskbar, skippager, skipswitcher, below, noborder: forced on
- acceptfocus, minimize, closeable: forced off
