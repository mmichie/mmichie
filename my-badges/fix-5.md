<img src="https://my-badges.github.io/my-badges/fix-5.png" alt="I did 5 sequential fixes." title="I did 5 sequential fixes." width="128">
<strong>I did 5 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/853b45726aa7db321395276f7531acd8cb51d897">853b457</a>: fix(core,eval): convert 13 collection method errors to structured exception types

Converted fmt.Errorf calls to proper exception types in list, set, and tuple methods:

**List methods (8 fixes):**
- append() argument count → TypeError
- extend() argument count → TypeError
- insert() argument count and type errors → TypeError (2 cases)
- remove() argument count → TypeError
- pop() type error → TypeError
- index() argument count → TypeError, value not found → ValueError
- count() argument count → TypeError

**Set methods (4 fixes):**
- set() constructor argument errors → TypeError (2 cases)
- remove() argument count → TypeError, missing element → KeyError
- pop() empty set → KeyError

**Tuple methods (2 fixes):**
- count() argument count → TypeError
- index() argument count → TypeError

**Enhanced KeyError struct:**
- Added optional Message field to support custom error messages
- Allows KeyError to work for both dict key lookups ("key not found: X") and set operations ("pop from an empty set")
- Updated Error() method to use Message when provided, fallback to Key otherwise

**Improved error unwrapping:**
- Added error unwrapping loop in errorToExceptionInstance to properly handle errors wrapped by fmt.Errorf(...%w...)
- Ensures structured exception types are preserved even when wrapped by higher-level error handling code

Test results: All 60 M28 tests passing + 11/12 collection exception type tests passing
- <a href="https://github.com/mmichie/m28/commit/03b048a44ee1855449825ea05a17784990cbf53a">03b048a</a>: fix(builtin,core,eval): convert 14 more high-impact fmt.Errorf to structured exception types

Converted fmt.Errorf calls to proper exception types in commonly-used operations:
- iter() and next() argument errors → TypeError
- chr() range error → ValueError
- ord() string length error → TypeError
- sum() type errors → TypeError (3 cases)
- dict unhashable key errors → TypeError (4 cases: dict_methods, containers, util.go literal construction)
- string formatting errors → KeyError (missing key), ValueError (malformed format)
- list index out of range → IndexError

Also extended WrapEvalError() to preserve exception types:
- Added type preservation for TypeError, ValueError, KeyError, IndexError, ZeroDivisionError, AttributeError
- Previously only NameError was preserved, causing all other typed errors to show as generic "Exception"
- Now all structured exception types propagate correctly through the error wrapping chain

All exceptions now properly convert to Python exception instances and can be caught with specific except clauses.

Test results: All 60 M28 tests passing + 10/10 exception type tests passing
- <a href="https://github.com/mmichie/m28/commit/7e1c70118a39c7401f3b45362969962dbdd93782">7e1c701</a>: fix(builtin,core): convert 14 high-impact fmt.Errorf to structured exception types

Converted fmt.Errorf calls to proper typed exceptions for common operations
that users will encounter frequently. This allows Python code to catch
specific exception types correctly.

Changes by category:

**Slice operations (4 fixes):**
- slice indices type errors → TypeError
- slice step = 0 → ValueError

**Collection constructors (4 fixes):**
- list() keyword args → TypeError
- tuple() keyword args → TypeError
- set() non-iterable arg → TypeError
- dict() invalid arg → TypeError

**Builtin functions (2 fixes):**
- all() non-iterable → TypeError
- any() non-iterable → TypeError

**Arithmetic (2 fixes):**
- Decimal division by zero → ZeroDivisionError
- Decimal unsupported operand → TypeError

**Attribute access (1 fix):**
- Missing attribute → AttributeError

**Type checking (1 fix):**
- len() with invalid type → TypeError (from previous commit)

Testing:
- All 60 M28 tests pass
- Manual testing confirms exception types preserved correctly
- Python code can now catch TypeError, ValueError, etc. properly

Example impact:
```python
try:
    s = slice("oops", 10)
except TypeError:  # Now works! (was generic Exception before)
    print("Caught it!")
```

Partial progress on M28-2ab3 (fmt.Errorf audit)
14 down, ~1,588 to go (primarily in less critical code paths)
- <a href="https://github.com/mmichie/m28/commit/009d1ed092d6ec1eabd14fe2cab1cb55c64ad457">009d1ed</a>: fix(eval,builtin): enhance exception message preservation and type accuracy

Fixed two issues with exception handling that were causing error messages
to be lost or duplicated:

1. Exception message duplication in errorToExceptionInstance:
   - When converting eval.Exception to Python exception, was calling exc.Error()
   - exc.Error() returns "Type: Message", then passing that to createPythonExceptionInstance
   - This caused "Exception: Exception" instead of preserving the actual message
   - Now use exc.Message directly to avoid duplication

2. TypeError not being raised by len() builtin:
   - len() was returning fmt.Errorf instead of *core.TypeError
   - This caused "object of type 'X' has no len()" to be generic Exception
   - Now returns proper TypeError that can be caught with "except TypeError"

Testing:
- All existing tests pass
- Exception types now preserved correctly (TypeError vs Exception)
- Exception messages preserved without duplication
- Re-raising exceptions maintains type and message

Partial progress on M28-6c9c (unittest.TestProgram issue still needs investigation)
- <a href="https://github.com/mmichie/m28/commit/4193a41361e45cc3c85d2766877f7c22f8d1ceae">4193a41</a>: fix(eval): preserve error messages when converting Go errors to Python exceptions

When exception class instantiation failed in createPythonExceptionInstance,
the function would fall back to returning string values instead of exception
instances. This caused error messages to be lost in the exception handling
chain, resulting in unhelpful "Exception: Exception" messages.

Changes:
- Modified createPythonExceptionInstance to always return exception Instance
  objects, even when class lookup or instantiation fails
- Create minimal Instance objects with message stored in args tuple attribute
- Added debug logging when instantiation fails to aid troubleshooting
- Ensures error messages are preserved throughout exception handling

Testing:
- Added comprehensive test suite for error message preservation
- All existing tests pass
- Error messages now properly preserved for TypeError, ValueError, Exception
- Significantly improves debugging experience

Closes M28-e346


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>