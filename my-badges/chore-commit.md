<img src="https://my-badges.github.io/my-badges/chore-commit.png" alt="I did a little housekeeping! 🧹" title="I did a little housekeeping! 🧹" width="128">
<strong>I did a little housekeeping! 🧹</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/144fa87d9bb892f1667e82b02a96e29c7bd56374">144fa87</a>: chore: clean up debug code and replace panic() with proper error handling

- Removed commented debug print statements from parser/parser.go
- Replaced panic() with log.Fatal() in initialization functions:
  - core/list_methods.go: InitListMethods()
  - core/dict_methods.go: InitDictMethods()
  - core/set_methods.go: InitSetMethods()
- Removed unused getNumber() function from builtin/utilities.go
- Left intentional panic() calls in Must* pattern functions (following Go conventions)
- Left controlled panic() in eval/evaluator.go (only triggers when StrictDuplicateChecking is enabled)


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>