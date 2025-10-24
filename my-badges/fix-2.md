<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/f243ebc820d58dedca21f27087ee5b472e18b7c0">f243ebc</a>: fix: enable functions to access module globals defined after function

Extended Context.Lookup() to check Global.ModuleDict when variable
lookup reaches the end of the scope chain. This allows functions to
access module-level variables that were defined after the function
was created, matching Python's global variable semantics.

In Python, functions look up globals at runtime from the module's
namespace, not at function definition time. Previously, M28 functions
could only access variables in their captured environment chain,
causing NameError for forward references within the same module.

Example that now works:
```python
def func():
    return CONST + 1  # Forward reference
CONST = 42
func()  # Returns 43
```

This progresses CPython stdlib compatibility by enabling proper
module-level namespace resolution for transpiled Python code.

test_bool.py still passes all 70 tests (100%).
- <a href="https://github.com/mmichie/m28/commit/1a8bbb7d03874e9a31c56aeafcafe8a077e7c98a">1a8bbb7</a>: fix: isinstance() with tuple of types now handles Class wrappers

Extended isinstance() tuple handling to properly check Class types
(like StrType) and wrapper types with GetClass() method against
primitive values like StringValue, NumberValue, etc.

Previously, isinstance('', (str, bytes)) would fail because str is
a StrType (Class wrapper) not a BuiltinFunction, and the tuple loop
only checked BuiltinFunction types for primitives.

Now both isinstance('', str) and isinstance('', (str, bytes)) work
correctly, fixing re._compiler.isstring() function which uses
isinstance(obj, (str, bytes)).

This progresses CPython stdlib compatibility for the re module.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>