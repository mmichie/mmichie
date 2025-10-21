<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/388989963d0bec19eaceb6dd41d3e9cc9efdd79b">3889899</a>: fix: improve Python module import compatibility

- Add raw bytes string (br/rb) support in tokenizer
- Fix module exports to include underscore-prefixed names
- Add sys.platform attribute using runtime.GOOS
- Update dict-literal to handle wrapped pairs from kwargs
- Improve lru_cache decorator to support parameterized syntax

This enables fnmatch and other stdlib modules to parse successfully.
Remaining work: Handle **kwargs marker in function call evaluation.
- <a href="https://github.com/mmichie/m28/commit/9b82e67c45252d1ae6420df7845375e87d57547b">9b82e67</a>: fix: correct Class.__eq__ for protocol dispatch and fix test type comparisons

Core type system fixes:
- Fix Class.__eq__ and __ne__ to expect 1 argument (protocol dispatch)
- Compare classes by name instead of pointer equality
- Handle type wrappers (StrType, TypeType) via GetClass() interface
- Ensure (type x) == (type y) works correctly across instances

Test fixes:
- Replace type(x) == "string" with type(x) == type("")
- Update all type comparisons in dict-ops, merge-ops, and bytes tests
- Get type objects once and reuse for consistent comparisons

These changes fix the remaining 5 test failures, bringing test success rate
from 90% to 100% (54/54 tests passing).


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>