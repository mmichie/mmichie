<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/775c3672c34eb9a184af5a918ceeb0a05cc5388b">775c367</a>: fix: Fix context manager to support inline expressions in with statements

The with statement now correctly handles inline function calls like:
  (with (open "file.txt" "r") as f ...)

Previously, this would incorrectly parse the function call as a list of
multiple context managers, resulting in "'function' object does not
support the context manager protocol" error.

The fix adds logic to distinguish between:
- Function call lists: (open "file" "r")
- Multiple manager lists: [mgr1 as var1 mgr2 as var2]

Uses a heuristic that checks for 'as' keywords in positions 1-2 to
determine if a list represents multiple managers.

File I/O examples now work with context managers, though some still
fail due to missing methods (writelines) or iteration support.
- <a href="https://github.com/mmichie/m28/commit/4a6dcd3fbfab01fb814a8d166a6d9df047fa1078">4a6dcd3</a>: fix: Fix syntax errors in examples and document interpreter limitations

- Add detailed interpreter fixes needed to ROADMAP.md:
  - Context manager protocol for file objects
  - __name__ attribute for functions and types
  - Tuple unpacking in for loops
  - Local module import resolution
  - Missing Python stdlib modules (shutil, pathlib, tempfile, zipfile, time)

- Fix syntax errors in 7 example files:
  - map_filter_reduce.m28: Fix missing parentheses and undefined infinity
  - dice_game.m28: Change 'do' to 'begin'
  - random_examples.m28: Fix for loop syntax and method calls
  - todo_app.m28: Fix method call syntax and dictionary access
  - text_adventure.m28: Fix lambda syntax, slice usage, remove __name__ check
  - functional_basics.m28: Comment out timer decorator, fix sorted usage

Examples now parse correctly and run until hitting interpreter limitations.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>