<img src="https://my-badges.github.io/my-badges/fix-3.png" alt="I did 3 sequential fixes." title="I did 3 sequential fixes." width="128">
<strong>I did 3 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/2a2c4dbc1711d046309a051b7803746eb89a4468">2a2c4db</a>: fix: complete Phase 3 of builtin cleanup - eliminate all remaining duplicates

- Removed all 19 remaining duplicate function registrations
- Math functions: removed duplicates from modules/math.go, kept in numeric.go
- Attribute functions: removed from attributes.go, kept better implementations in essential_builtins.go
- Collection/iteration: consolidated to proper locations (iteration.go, list.go, collections.go)
- Utilities: removed duplicates from essential_builtins.go and utilities.go
- Fixed import issues and removed unused code
- All tests pass successfully

This completes the builtin system cleanup:
- Total builtins reduced from 110 to 69
- All 41 duplicate registrations eliminated
- Each function now has a single source of truth
- Code is much cleaner and easier to maintain
- <a href="https://github.com/mmichie/m28/commit/d7d20530e7047cd0dc99f28ed8e3191cda916893">d7d2053</a>: fix: complete Phase 2 of builtin cleanup - consolidate map, filter, reduce

- Consolidated map, filter, reduce functions to functional.go
- Used best implementations: map from utilities.go (better error messages),
  filter from functional.go (supports None), reduce from list.go (supports both argument orders)
- Removed duplicate registrations from list.go and utilities.go
- Eliminated 6 duplicate registrations (3 functions × 2 extra registrations each)
- All tests pass successfully
- Updated ROADMAP.md to mark Phase 2 as complete
- Updated DUPLICATE_BUILTINS_REPORT.md with current state

This reduces total builtins from 94 to 88 and duplicate registrations from 25 to 19.
- <a href="https://github.com/mmichie/m28/commit/a4eba1fb630effb77c7ae4e2231423697e79af31">a4eba1f</a>: fix: complete Phase 1 of builtin system cleanup - remove operator duplicates

- Removed legacy arithmetic.go and comparison.go files
- Eliminated 16 duplicate operator registrations (+, -, *, /, %, **, ==, \!=, <, >, <=, >=, and, or, not, in)
- Updated registry.go to only use the new modular operators/ structure
- All tests pass successfully
- Updated ROADMAP.md to mark Phase 1 as complete
- Updated DUPLICATE_BUILTINS_REPORT.md with current state

This reduces total builtins from 110 to 94 and duplicate registrations from 41 to 25.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>