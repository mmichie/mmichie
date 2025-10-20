<img src="https://my-badges.github.io/my-badges/chore-commit.png" alt="I did a little housekeeping! 🧹" title="I did a little housekeeping! 🧹" width="128">
<strong>I did a little housekeeping! 🧹</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/4a852cdf70c70561b1c8be171078537e1d1f7664">4a852cd</a>: chore: remove debug output after fixing comprehension bugs

Remove DEBUG_GLOBALS, DEBUG_COMP, and types module debug output
that was added during investigation. The root issues have been
fixed and the debug code is no longer needed.

Files cleaned:
- builtin/misc.go: removed DEBUG_GLOBALS output and unused os import
- eval/evaluator.go: removed DEBUG_COMP output and unused os import
- modules/python_loader.go: removed types module debug output


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>