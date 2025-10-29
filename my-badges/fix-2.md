<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/38abaa32ba2bd5e345762f56b2cbdd75e0d9f25a">38abaa3</a>: fix: support non-string keys in dict() constructor

Fix dict() constructor to accept any hashable type as keys, not just
strings. This matches Python's behavior where dict keys can be any
hashable object (int, float, tuple, etc).

Changes:
- Replace string-only key check in dict() with SetValue() which handles
  any hashable type
- Properly handle both list and tuple pairs in dict() constructor
- Fix tuple unpacking when converting to dict

Example that now works:
  dict([(1, 'a'), (2, 'b')])  # int keys
  dict([((1,2), 'x')])        # tuple keys

This enables stdlib modules like traceback.py that use dict
comprehensions with non-string keys.
- <a href="https://github.com/mmichie/m28/commit/04aa193c19dd8355dc9319eaa88d40c47dee035b">04aa193</a>: fix: enable del statement in class body with proper scoping

Fix class body variable scoping to allow del statements to work
correctly inside class definitions:

- Evaluate all class body statements (not just = and def) in
  classBodyCtx instead of outer ctx, allowing access to class
  variables during construction
- Add special handling for del statements in class body to remove
  attributes from both classBodyCtx and class.Attributes
- Support multiple deletion targets (e.g., del x, y, z)

This fixes patterns like:
  class Foo:
      temp = 1
      y = temp + 1  # Can now reference temp
      del temp       # And delete it from the class

Enables Python stdlib modules like textwrap.py that use temporary
variables in class bodies.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>