<img src="https://my-badges.github.io/my-badges/chore-commit.png" alt="I did a little housekeeping! 🧹" title="I did a little housekeeping! 🧹" width="128">
<strong>I did a little housekeeping! 🧹</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/plx/commit/9cbf9e0c67ab74d745b4f0b95f947eb6cb009414">9cbf9e0</a>: chore: bump to 0.4.0

Five new commits since 0.3.0 land the `plx health` subcommand:

  ccfe67e  feat: add health subcommand
  c6f88ed  feat(health): macOS checks + cache layer + IP address check
  6eeb58e  feat(health): add --json, --check <name>, and --value modes
  086b3f3  feat(health): TOML config for thresholds, network host, and disabled checks

`plx health` replaces a 261-line shell function with native-Rust checks
covering load, memory, disk, uptime, ip, network, and (on macOS) cpu_temp,
disk_health, software_updates, and firewall. Warm-cache full report runs
in ~33ms; single-check --value mode lands in 8-10ms (tmux-status-line
friendly). Slow checks are cached under ~/.cache/plx/health/ with
mtime-based TTL. Thresholds and the network reachability target are
configurable via the [health] section of ~/.config/plx/config.toml.

Test surface: 256 unit + 42 integration tests; clippy --all-targets
clean under pedantic.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>