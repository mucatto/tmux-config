# Portable tmux configuration

A compact tmux setup with:

- a dark blue status theme and clear active-pane borders;
- mouse support, vi copy mode, and X11 clipboard integration;
- convenient splits that inherit the current working directory;
- automatic session persistence with tmux-resurrect and tmux-continuum;
- generic restoration of nested tmux sessions;
- `Prefix + Shift+n` to create an outer window attached to `<name>-inner`;
- automatic relinking of restored `X` windows to matching `X-inner` sessions.

## Requirements

On Ubuntu or Debian:

```bash
sudo apt update
sudo apt install tmux xclip git
```

Install the persistence plugins:

```bash
mkdir -p ~/.tmux/plugins
git clone https://github.com/tmux-plugins/tmux-resurrect ~/.tmux/plugins/tmux-resurrect
git clone https://github.com/tmux-plugins/tmux-continuum ~/.tmux/plugins/tmux-continuum
```

## Install

Back up any existing configuration, then install this one:

```bash
test ! -e ~/.tmux.conf || cp -a ~/.tmux.conf ~/.tmux.conf.backup
cp tmux.conf ~/.tmux.conf
install -Dm755 scripts/tmux-relink-inner-sessions \
  ~/.local/bin/tmux-relink-inner-sessions
tmux source-file ~/.tmux.conf
```

## Nested workspaces

Inside an outer tmux session, press `Prefix + Shift+n` (uppercase `N`), enter a
workspace name, and tmux creates a window attached to `<name>-inner`.

The restore rules also recognize these commands:

```bash
TMUX= tmux attach -E -t example-inner
TMUX= tmux attach-session -E -t example-inner
TMUX= tmux new-session -A -E -s example-inner
```

tmux-resurrect saves the complete target name. After a reboot, tmux-continuum
restores the sessions. The relink helper then reconnects a single-pane shell
window named `X` to an existing `X-inner` session when a client attaches.

## Notes

- The clipboard bindings use `xclip` and target X11 sessions.
- Runtime snapshots are intentionally excluded because they can reveal local
  paths, commands, session names, and pane contents.
- Do not commit `~/.local/share/tmux/resurrect` or tmux configuration backups.
