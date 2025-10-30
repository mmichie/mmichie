<img src="https://my-badges.github.io/my-badges/fix-4.png" alt="I did 4 sequential fixes." title="I did 4 sequential fixes." width="128">
<strong>I did 4 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/1bdc765e9470b41da2514769555800920088d990">1bdc765</a>: fix: make exceptions proper Python instances instead of strings

FUNDAMENTAL FIX for exception handling:

1. Exception Binding (eval/evaluator.go):
   - Changed try/except to create proper Python exception instances
   - Added createPythonExceptionInstance() helper function
   - Maps Go error types to Python exception classes (ImportError, TypeError, etc.)
   - Exceptions caught in 'except' blocks are now instances, not strings

2. Exception String Methods (builtin/errors.go):
   - Added __str__() to BaseException to return first arg
   - Added __repr__() to BaseException for proper repr() output
   - Format: ImportError("message") instead of <instance at 0x...>

This fixes the root cause where:
- 'except ImportError as e:' bound 'e' to a string
- type(e).__name__ returned "string" instead of "ImportError"
- e.__class__ didn't exist

Now exceptions work correctly:
- e is proper ImportError instance
- type(e).__name__ returns "ImportError"
- str(e) returns the message
- e.args contains the tuple of arguments
- e.__class__ returns the exception class

Critical for CPython stdlib compatibility.
- <a href="https://github.com/mmichie/m28/commit/b1c4040f47fdac6857401821f82cdf6eac7dff31">b1c4040</a>: fix: implement Python bool-int equality semantics

In Python, bool is a subclass of int, so:
- False == 0 should be True
- True == 1 should be True
- False == any_other_number should be False
- True == any_other_number should be False

Modified EqualValues() in core/utilities.go to handle:
1. BoolValue compared to NumberValue
2. NumberValue compared to BoolValue

This fixes the core bool regression test requirement that False should
equal 0 and True should equal 1.
- <a href="https://github.com/mmichie/m28/commit/9d4568eab3dc55cd05f465771241217587ea3c11">9d4568e</a>: fix: correct TupleGetter descriptor protocol argument indexing

The __get__ method in TupleGetter was expecting self as args[0], but
since it's implemented as a closure that captures self (tg), the
arguments should be:
- args[0]: instance (the namedtuple instance)
- args[1]: owner (the namedtuple class, optional)

Changed from expecting 2 args with args[1] as instance to expecting 1 arg
with args[0] as instance.

This fixes namedtuple field access - now p.x returns the actual value (10)
instead of the tuplegetter descriptor object.
- <a href="https://github.com/mmichie/m28/commit/8867b50e7a2b4bc5e6b6ae4a9386f3822421aca7">8867b50</a>: fix: implement Python str.split() semantics

Without separator argument, Python's split() uses any whitespace
as delimiter and removes empty strings (like Go's strings.Fields).
With separator argument, keeps empty strings (strings.Split).

This fixes namedtuple field validation which uses:
  field_names.replace(',', ' ').split()

Changes:
- Added hasSep flag to detect if separator was explicitly provided
- When no separator: use strings.Fields() (removes empty strings)
- When separator provided: use strings.Split() (keeps empty strings)
- Fixed maxsplit handling for both cases


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>