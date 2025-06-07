<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/c71d36c2932095ab24d03eb823975ebc47c79c3d">c71d36c</a>: fix: Fix module import hang and update all tests to current M28 syntax

- Add circular import detection to prevent infinite recursion
- Fix dictionary dot notation (dict.property) by handling key prefixes
- Update import syntax from ':as' to 'as' to avoid parser issues
- Fix duplicate dict() function definition in builtin.go
- Update all tests to use == for comparisons instead of =
- Fix function definitions and exports syntax in tests
- Implement __exports__ support in module loader
- All 10 tests now pass (previously only 3 passed)

The module system now properly handles circular dependencies and supports
dot notation for accessing module members.
- <a href="https://github.com/mmichie/m28/commit/4c2f383703b6395c022a8b8920683d229b6539ee">4c2f383</a>: fix: Fix closures_decorators.m28 example to work correctly

- Add top-level time import so lambdas can access time.sleep
  - Previously failed because time was only imported inside decorator scope
- Fix closure variable mutation in make_counter and count_calls
  - Use mutable containers (dicts) to store state
  - Direct assignment to closure variables creates new local variables

Now all M28 examples work correctly (100% success rate).


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>