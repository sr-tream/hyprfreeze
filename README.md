# hyprfreeze
[![basher install](https://www.basher.it/assets/logo/basher_install.svg)](https://www.basher.it/package/)

Hyprfreeze is a utility to suspend a game process (and other programs) in Hyprland and Sway.

https://github.com/Zerodya/hyprfreeze/assets/73220426/541318e2-441a-485a-91c5-f58d4f65926a

**Useful to:**
- Pause single-player games that can't normally be paused (Elden Ring, Baldur's Gate 3, ...)
- Pause cutscenes to read the subtitles or if you suddenly need to leave your desk
- Save system resources (excluding RAM) if you need them for another computer task, or if the game's pause menu uses too many

> **Note:** This repo is in **maintenance-only** mode as it is feature-complete for my scope.
> - For Hyprland: I use this tool in Hyprland everyday and will update it to fix potential bugs or when `hyprctl` changes its syntax. 
> - For Sway: it's not my first priority as I don't daily drive it, so if it stops working there please open a new issue as I will not be aware of it.

## Installation
### Arch Linux
Hyprfreeze is available in the [AUR](https://aur.archlinux.org/packages/hyprfreeze-git). (Maintained by [Aethar](https://github.com/Aethar01))

### Dependencies
- a compatible window manager (`hyprland` or `sway`) to get the PID of the active window
- `jq` to parse json
- `psmisc` contains 'pstree' which is required to list child processes
  ### Optional
- [`hyprprop`](https://github.com/vilari-mickopf/hyprprop) or [`swayprop`](https://git.alternerd.tv/alterNERDtive/swayprop) to get the pid of a window by selecting it with your mouse
  ### Highly Recommended
- Running games in [`gamescope`](https://github.com/ValveSoftware/gamescope) fixes mouse input not working in other XWayland windows after pausing a Wine game (see [#1](https://github.com/Zerodya/hyprfreeze/issues/1)). It's also the superior way to game in Wayland anyway.

### Basher
Hyprfreeze is compatible with shell script package manager [Basher](https://basher.it).
```bash
basher install Zerodya/hyprfreeze
```

### Manual
Clone this repo and symlink the `hyprfreeze` script to a directory in your `PATH`:
```bash
git clone https://github.com/Zerodya/hyprfreeze.git Hyprfreeze
ln -s $(pwd)/Hyprfreeze/hyprfreeze $HOME/.local/bin
```

## Usage
Add a bind in your Hyprland or Sway config to pause the current active window:
```bash
# ~/.config/hypr/hyprland.conf
...
# Toggle freeze on active window
bind = , PAUSE, exec, hyprfreeze -a
```
### Available flags
```
-h, --help            show help message

-a, --active          toggle suspend by active window
-p, --pid             toggle suspend by process id
-n, --name            toggle suspend by process name/command
-r, --prop            toggle suspend by clicking on window (hyprprop/swayprop must be installed)

-s, --silent          don't send notification
-t, --notif-timeout   notification timeout in milliseconds (default 5000)
--info                show information about the process
--dry-run             doesn't actually suspend/resume a process, useful with --info
--debug               enable debug mode
```
### Examples:
```bash
# Pause game by process name
hyprfreeze -n eldenring.exe
```
```bash
# Get info about a process by clicking on its window, without suspending it
hyprfreeze -r --info --dry-run
```
## Disclaimer
There is always the risk, although slim, that an application may crash.

This is intrinsically related to modifying running processes and is not something that Hyprfreeze can prevent.

Please make sure to **save your data** before using hyprfreeze.
