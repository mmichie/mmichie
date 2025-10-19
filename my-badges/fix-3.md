<img src="https://my-badges.github.io/my-badges/fix-3.png" alt="I did 3 sequential fixes." title="I did 3 sequential fixes." width="128">
<strong>I did 3 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/cc19e46052c140cbd2b87c971af6db1aca414709">cc19e46</a>: fix: handle comments and dotted decorators in Python parser

Two fixes to improve Python module compatibility:

1. Python tokenizer now handles '#' comments in scanToken()
   - Comments can appear anywhere, not just at line start
   - Fixes tokenization of files with inline comments
   - Previously caused "unexpected character '#'" errors

2. Parser now supports dotted decorator names
   - Handles decorators like @contextlib.contextmanager
   - Uses parseAttribute() to handle dot notation after decorator name
   - Previously only supported single-identifier decorators

These fixes allow importing more Python stdlib modules including
those using contextlib decorators and inline comments.
- <a href="https://github.com/mmichie/m28/commit/c4fa3582e850f25efa03e309421ea12ab4882f83">c4fa358</a>: fix: support multi-line imports with parentheses

The parser was failing on Python imports like:
  from module import (name1, name2,
                       name3, name4)

This is a common pattern in Python stdlib (e.g., abc.py) for
organizing long import lists across multiple lines.

Changes:
- parseFromImportStatement now detects and handles LPAREN after import
- Skips NEWLINE tokens between names when inside parentheses
- Supports trailing commas in parenthesized import lists
- Added recursion depth and call count tracking to detect infinite loops
- Added enterParse/exitParse instrumentation for debugging

This fix resolves the parser hang when importing abc.py and similar
stdlib modules that use multi-line imports.
- <a href="https://github.com/mmichie/m28/commit/88ca8294420144eb4d4f037af5e439d59cc49ab3">88ca829</a>: fix: expand C extension detection and document parser limitations

Add missing built-in C extensions to detection list to prevent import
attempts that would fail:
- _weakref (weak reference support, built-in)
- _abc (abstract base class support)
- sys (system functions, built-in)
- builtins (built-in functions)

Document known limitations in ROADMAP.md:
- Parser hangs on some stdlib modules (abc.py, typing.py)
  - Root cause: Infinite loop in parser, needs timeout/recursion limits
  - Discovered during abc import testing
- C extension dependency chains common in stdlib
  - Example: abc → _py_abc → _weakrefset → _weakref
  - Even "pure Python" modules often have transitive C dependencies

Document what currently works:
- Pure Python modules without C deps (keyword module ✓)
- Custom .py files with classes, decorators, f-strings ✓
- Simple control flow and functions ✓

This prevents the hang that occurred when importing modules with C extension
chains, replacing it with clear error messages.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>