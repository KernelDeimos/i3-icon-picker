# i3-icon-chooser

A floating emoji picker for i3wm. Press a hotkey, pick an emoji, and the current workspace gets labelled — `8:🤖`, `4:🌍`, etc. The number is preserved so `Alt+number` switching still works.

![i3 bar showing workspaces 4:🌍, 7:🧬, 8:🤖](screenshot.png)

## Prerequisites

- i3wm
- Python 3.10+
- PyQt6: `sudo pacman -S python-pyqt6` on Arch

## Install

Place the script somewhere stable and make it executable:

```bash
chmod +x i3-icon-chooser
```

Symlink it onto your PATH:

```bash
ln -s /path/to/i3-icon-chooser ~/bin/i3-icon-chooser
```

## Configure i3

Add to `~/.config/i3/config`:

```
# float the picker window
for_window [title="i3-icon-chooser"] floating enable

# bind a hotkey — use the full path, not just the command name,
# since i3's exec environment may not include ~/bin
bindsym $mod+i exec --no-startup-id /full/path/to/i3-icon-chooser
```

### Keep Alt+number working after renaming

If your workspace bindings use variables (`workspace $ws4`), they'll break after a rename because i3 matches by exact name. Replace them with number-based bindings written out literally:

```
bindcode $mod+10    workspace number 1
bindcode $mod+11    workspace number 2
...
bindcode $mod+19    workspace number 10

bindcode $mod+Shift+10    move container to workspace number 1
...
bindcode $mod+Shift+19    move container to workspace number 10
```

The `number` keyword must appear literally in the config — it doesn't work through variable substitution.

### Bar display

Inside `bar {}`, `strip_workspace_numbers no` shows `8:🤖`; `strip_workspace_numbers yes` shows just `🤖`.

Reload i3 with `Alt+Shift+c`.

## Usage

Press `Alt+i` (or whatever you bound). Type to filter, click or press Enter to select. Escape to cancel.

## How it works

On launch the script queries `i3-msg -t get_workspaces` to get the focused workspace number. Selecting an emoji runs `i3-msg 'rename workspace to "N:emoji"'`. The window is centred on the monitor containing the active workspace, so multi-monitor setups work without any extra config.
