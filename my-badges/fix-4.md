<img src="https://my-badges.github.io/my-badges/fix-4.png" alt="I did 4 sequential fixes." title="I did 4 sequential fixes." width="128">
<strong>I did 4 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/916e11533d2d47d52232847330bc800e4eccb7d4">916e115</a>: fix(module): define __doc__ at module level

Python expects __doc__ to be available in module scope. Added __doc__
definition (initialized to None) alongside __name__ and __file__ in:
- core/module_loader.go
- core/module_loader_enhanced.go
- modules/python_loader.go
- main.go (__main__ module)

Fixes pdb import failure and resolves 6 failing CPython tests.
- <a href="https://github.com/mmichie/m28/commit/755e6ae41b4ded3dbb44b0b44a5983a644a8e81c">755e6ae</a>: fix(set): accept iterables in all set methods

Updated the following set methods to accept any iterable (lists, tuples,
etc.) in addition to sets, matching Python's behavior:

- union()
- intersection()
- symmetric_difference()
- issubset()
- issuperset()
- isdisjoint()

Both set_methods.go and dot_notation.go updated for consistency.
- <a href="https://github.com/mmichie/m28/commit/737d8c683f66dd545694b8da2f9cb0db76edd315">737d8c6</a>: fix(set): accept iterables in set.difference()

Python's set.difference() accepts any iterable, not just sets.
Updated both implementations (set_methods.go and dot_notation.go)
to accept lists, tuples, and other iterables in addition to sets.
- <a href="https://github.com/mmichie/m28/commit/b88242c97f1064ac1ba1d36c6a2a359191901c1c">b88242c</a>: fix(kwargs): enable list.sort() kwargs and __import__ kwargs support

- Remove sort from auto-call list in dot_notation.go so that lst.sort
  returns a BoundMethod instead of auto-calling (allows key= and reverse=)
- Update __import__ to use BuiltinFunctionWithKwargs for fromlist= support
- Add type info to kwargs error message for debugging

This unblocks unittest.TestProgram which uses list.sort(key=...) in
getTestCaseNames to sort test methods.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>