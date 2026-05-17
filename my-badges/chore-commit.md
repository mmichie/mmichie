<img src="https://my-badges.github.io/my-badges/chore-commit.png" alt="I did a little housekeeping! 🧹" title="I did a little housekeeping! 🧹" width="128">
<strong>I did a little housekeeping! 🧹</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/plx/commit/d96e177ce151fffb267808284e20b27539e784b2">d96e177</a>: chore: workflow_dispatch trigger + .direnv/.br_recovery gitignores

- ci.yml: add `workflow_dispatch:` so CI can be manually rerun from
  the GitHub UI (useful for chasing flake rates).
- .gitignore: add `.direnv/` (created by direnv on first activation
  via the .envrc we added) and `.beads/.br_recovery/` (timestamped
  beads recovery snapshots not covered by .beads/.gitignore).

Both follow the sak conventions.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>