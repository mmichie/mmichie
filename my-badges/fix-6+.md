<img src="https://my-badges.github.io/my-badges/fix-6+.png" alt="I did 8 sequential fixes." title="I did 8 sequential fixes." width="128">
<strong>I did 8 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/841f42a6a7913e08a9956769e49fa21e03105534">841f42a</a>: fix(core): add __name__ and attribute support to AsyncFunction

Add GetAttr and SetAttr methods to AsyncFunction to support:
- __name__, __qualname__, __module__ attributes
- __code__ with co_flags=128 (CO_COROUTINE)
- __annotations__, __globals__, __dict__, __doc__, __wrapped__
- Custom attributes set by decorators (e.g., setattr(func, '_marked', True))

This fixes test_async_await in test_grammar.py which requires async
functions to have proper function attributes.
- <a href="https://github.com/mmichie/m28/commit/9969517ee1435444da9c8c5ceebd2e5d24a828c6">9969517</a>: fix(builtin): set SyntaxError.offset to 1 instead of None

CPython's SyntaxError for assertion warnings has offset=1 (start of
line), not None. This is required by test.support.check_syntax_error
which asserts that offset is not None.
- <a href="https://github.com/mmichie/m28/commit/6ba79afd7082f9e6a5d63261d5c7afb8dc6d2e5d">6ba79af</a>: fix(builtin): add proper SyntaxError attributes in compile()

Set msg, lineno, offset, text, and filename attributes when creating
SyntaxError from SyntaxWarning in compile(). These attributes are
required by Python's exception handling and unittest framework.
- <a href="https://github.com/mmichie/m28/commit/1e8732934fd0608ee8a883520d83293dcb4c8e1e">1e87329</a>: fix(builtin): convert SyntaxWarning to SyntaxError in compile()

When warnings.simplefilter('error', SyntaxWarning) is active, compile()
now properly converts SyntaxWarning to SyntaxError to match CPython
behavior.

Changes:
- Make walkForAsserts, checkAssertCondition, and emitSyntaxWarning
  return errors to propagate exceptions from warnings.warn_explicit
- Check for eval.Exception with Type="SyntaxWarning" and convert to
  SyntaxError in compile() builtin
- Also handle core.PythonError case for completeness

This fixes test_grammar.py tests that expect:
- compile('assert(x, "msg")', ...) to raise SyntaxError
- compile('assert(False, "msg")', ...) to raise SyntaxError
- compile('assert(False,)', ...) to raise SyntaxError
- <a href="https://github.com/mmichie/m28/commit/a7604b2b84f116db43bcf69db4712f5ba157506c">a7604b2</a>: fix(parser): validate underscore placement in numeric literals

Add validateUnderscores() function to detect invalid underscore patterns
in numeric literals per Python 3 specification:
- Trailing underscores (42_)
- Consecutive underscores (4__2)
- Underscore after base prefix (0b_1, 0o_5, 0x_f)
- Underscore before/after decimal point (1_.4, 1._4)
- Underscore before/after exponent (1_e10, 1e_10)

All 9 invalid patterns now correctly raise SyntaxError while valid
patterns (1_000_000, 0xff_ff) continue to work.
- <a href="https://github.com/mmichie/m28/commit/1f8afde78623fa9d4b3233c6eb93f5b2d7b249c9">1f8afde</a>: fix(parser): detect duplicate keyword arguments in function calls

- Add seenKwargs map to track keyword argument names during parsing
- Report SyntaxError when same keyword appears multiple times
- Example: f(x=1, x=2) now raises "keyword argument repeated: x"
- This matches Python 3 behavior for compile-time duplicate detection
- <a href="https://github.com/mmichie/m28/commit/e0cb503673adeb54447815b604ebec06c1fde47a">e0cb503</a>: fix(parser): add SyntaxError validation for function parameters and literals

- Add validateParameters() to reject bare * without keyword-only params
  - def f(*): pass -> SyntaxError: named arguments must follow bare *
  - def f(*,): pass -> SyntaxError: named arguments must follow bare *
  - def f(*, **kwds): pass -> SyntaxError: named arguments must follow bare *

- Fix tokenizer to detect invalid decimal literals
  - Add lookahead validation for scientific notation (e/E must be followed by digit)
  - Add check for number followed by letter (e.g., "1Else" -> invalid decimal literal)
  - This matches Python 3 behavior which rejects these at tokenization time
- <a href="https://github.com/mmichie/m28/commit/b24aec3a7e462c48ac907fadff6feaa98ab32b7b">b24aec3</a>: fix(warnings): properly pass filename through compile() to warnings

- Thread filename parameter through walkForAsserts, checkAssertCondition, and emitSyntaxWarning in builtin/misc.go
- Use AST node's source location for line number in warnings
- Create WarnExplicit helper that calls Python's warnings.warn_explicit
  for proper integration with catch_warnings() context manager
- This fixes the '<string>' != '<testcase>' assertion error in
  test.support.warnings_helper when compile() emits SyntaxWarnings


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>