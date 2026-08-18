# System Pulse for Omarchy

An auditable, low-overhead CPU and memory widget for Omarchy 4. Click the bar
meters for current top processes or open the real `htop` in a normal terminal
window.

System Pulse is a maintained fork of Fernando Menolli's excellent
[`omarchy-htop`](https://github.com/fernandomenolli/omarchy-htop). Fernando
designed and implemented the original plugin; this fork preserves the full Git
history, copyright, MIT licence, screenshots, and core architecture. See
[`UPSTREAM.md`](UPSTREAM.md) for the exact baseline and every intentional
divergence.

![The meters and process panel](docs/panel.png)

## Why this fork exists

The fork gives its owner an explicit review point for every update. The
installed checkout follows this repository only; nothing is pulled from the
upstream project or the plugin marketplace automatically.

The original design is deliberately retained because it is efficient:

- CPU and memory meters read `/proc/stat` and `/proc/meminfo` directly.
- The idle bar launches no recurring shell process.
- Process scanning runs only while the panel is open.
- The full `htop` process starts only when requested.

This fork additionally makes process accounting safe against PID reuse by
matching both PID and Linux process start time between samples.

## Requirements

- Omarchy 4 or newer
- `htop`
- A standard Linux `/proc` filesystem

## Install

```bash
omarchy plugin add https://github.com/thenailedone/omarchy-system-pulse.git --enable
```

## Usage

| Action | Result |
|---|---|
| Left click | Open or close the process panel |
| Right click | Cycle both → CPU → memory |
| Open htop | Launch or focus a dedicated htop terminal window |

Settings under **Setup → Plugins** control the refresh interval, visible bar
meters, urgent threshold, and process-list length.

## Data access and security

System Pulse:

- reads `/proc/stat`, `/proc/meminfo`, `/proc/uptime`, and `/proc/<pid>/stat`;
- creates a private htop configuration under
  `~/.local/state/omarchy/plugins/io.github.thenailedone.system-pulse/`;
- executes only its bundled `htop-window` launcher and a fixed, non-user-input
  process snapshot command;
- does not use the network, `sudo`, `pkexec`, Docker, telemetry, or credentials;
- does not modify Hyprland, the global htop configuration, or packaged files in
  `/usr/share/omarchy`.

## Verification

```bash
node tests/run.js
omarchy plugin validate .
git diff --check
```

Before updating an installed copy, inspect what changed:

```bash
git fetch origin
git log --oneline HEAD..origin/main
git diff --stat HEAD..origin/main
git diff HEAD..origin/main
```

Then update explicitly:

```bash
omarchy plugin update io.github.thenailedone.system-pulse
```

## Remove

```bash
omarchy plugin remove io.github.thenailedone.system-pulse
```

The private htop state is intentionally retained. Remove it separately if no
longer wanted:

```bash
rm -rf ~/.local/state/omarchy/plugins/io.github.thenailedone.system-pulse
```

## Licence and credit

MIT. Original plugin copyright and implementation by Fernando Menolli. Fork
maintenance and PID-reuse hardening by `thenailedone`. See [`LICENSE`](LICENSE)
and the preserved Git history.
