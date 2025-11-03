<img src="https://my-badges.github.io/my-badges/fix-5.png" alt="I did 5 sequential fixes." title="I did 5 sequential fixes." width="128">
<strong>I did 5 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/ee411abc938296582ec77e266ed811b7bd3b71a0">ee411ab</a>: fix: handle type constructors with GetClass() in isinstance

Move Callable check in isinstance after GetClass() extraction to support
type constructors like IntType that are both Callable and have GetClass().

Before: isinstance(128, int) returned False because int (IntType) matched
the Callable check and returned early before GetClass() could extract the
class.

After: isinstance checks GetClass() first, extracts the class from IntType,
then checks primitive type matching. isinstance(128, int) now correctly
returns True.

This fixes CPython's functools.lru_cache which uses isinstance(maxsize, int)
to validate arguments.
- <a href="https://github.com/mmichie/m28/commit/8e03044f33dd5d6b38fa7f5a39d1e266b6bff5f3">8e03044</a>: fix: handle Callable objects in isinstance second argument

Modified isinstanceForm to handle Callable objects that aren't classes,
such as functools.partial (type *modules.partialBuiltin). Previously,
isinstance would raise an error when given a callable constructor that
wasn't a BuiltinFunction or Class.

Changes:
- Added check for BuiltinFunction with __name__ attribute matching
- Added fallback for any Callable object: returns False instead of error
- Prevents "isinstance second argument must be a class" error

This allows code like isinstance(obj, functools.partial) to work without
crashing, even though it returns False (partial instances appear as
functions in M28).

Fixes isinstance errors in inspect.py and dataclasses.py.
- <a href="https://github.com/mmichie/m28/commit/07586b4e8e2f5b21a97f8228ed8d3c1005e38276">07586b4</a>: fix: handle mixed wrapped/flat dict-literal formats

Modified DictLiteralForm to detect when arguments mix wrapped pairs and
non-wrapped items, and fall through to flat format handling in such cases.
Also added support for **unpack markers in wrapped pairs format.

The issue occurred when Python parser generated dict-literal calls with:
- First arg as wrapped pair: (key value)
- Subsequent args as bare symbols or non-pairs

Now validates all args are consistently wrapped before using wrapped format,
otherwise falls back to flat format which handles both regular pairs and
**unpack markers.

Fixes dict-literal parsing errors in inspect.signature() and dataclasses.
- <a href="https://github.com/mmichie/m28/commit/8568308ca3c51e0bad6cec77c5dcabfe066ba87f">8568308</a>: fix: raise proper AttributeError for attribute access failures

Modified getattr() and dot notation to raise core.AttributeError instead
of generic fmt.Errorf when attributes are not found. This ensures Python's
except AttributeError clauses work correctly.

Changes:
- builtin/essential_builtins.go: getattr() now raises core.AttributeError
- eval/dot_notation.go: dot notation raises core.AttributeError for dict,
  list, string, and generic object attribute access failures

Fixes exception handling in inspect.py and other CPython stdlib modules
that rely on catching AttributeError.
- <a href="https://github.com/mmichie/m28/commit/3f8160edaa3623c837004c80c6833d7e95794a95">3f8160e</a>: fix: prevent str() from calling instance __str__ on class objects

- Add special handling in StrType.Call() for Class objects
- Classes should use their own String() method, not instance __str__
- Also handle wrapper types that embed Class (StrType, IntType, etc.)

This fixes the error 'missing required argument: self' when printing
classes from imported modules like inspect.Signature. The issue was
that str(SomeClass) was trying to call SomeClass.__str__() with no
arguments, but __str__ is an instance method expecting self.

Enables proper string conversion of CPython stdlib classes.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>