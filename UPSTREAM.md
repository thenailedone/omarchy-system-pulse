# Upstream and trust model

System Pulse is a maintained fork of Fernando Menolli's
[`omarchy-htop`](https://github.com/fernandomenolli/omarchy-htop), originally
released under the MIT licence. The original copyright and licence are kept in
[`LICENSE`](LICENSE), and the full upstream Git history remains intact.

## Baseline

- Upstream repository: `https://github.com/fernandomenolli/omarchy-htop`
- Fork baseline: `bea6e81eca7b593019f2fe43116f7080584117f0`
- Upstream release: `v0.1.0`

## Local changes

- Renamed the plugin and ID so this fork cannot be confused with or silently
  replaced by the marketplace package.
- Added process start time to the `/proc/<pid>/stat` snapshot. CPU samples now
  require both PID and start time to match, preventing false spikes when Linux
  recycles a PID between samples.
- Moved the private htop state and window app ID under the fork's namespace.
- Added tests for recycled-PID rejection.

## Update policy

The installed plugin tracks only this repository. Upstream changes are never
merged automatically. Review upstream commits, merge or cherry-pick them here,
run the test and validation commands documented in `README.md`, and inspect the
resulting diff before updating the installed plugin.
