# Portable tmux configuration

A compact tmux setup with:

- a dark blue status theme and clear active-pane borders;
- mouse support, vi copy mode, and X11 clipboard integration;
- convenient splits that inherit the current working directory;
- automatic session persistence with tmux-resurrect and tmux-continuum;
- generic restoration of nested tmux sessions;
- `Prefix + N` to create an outer window attached to `<name>-inner`.

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
tmux source-file ~/.tmux.conf
```

## Nested workspaces

Inside an outer tmux session, press `Prefix + N`, enter a workspace name, and
tmux creates a window attached to `<name>-inner`.

The restore rules also recognize these commands:

```bash
TMUX= tmux attach -t example-inner
TMUX= tmux attach-session -t example-inner
TMUX= tmux new-session -A -s example-inner
```

tmux-resurrect saves the complete target name. After a reboot,
tmux-continuum restores both the sessions and their nested attachments.

## Notes

- The clipboard bindings use `xclip` and target X11 sessions.
- Runtime snapshots are intentionally excluded because they can reveal local
  paths, commands, session names, and pane contents.
- Do not commit `~/.local/share/tmux/resurrect` or tmux configuration backups.
