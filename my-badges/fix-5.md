<img src="https://my-badges.github.io/my-badges/fix-5.png" alt="I did 5 sequential fixes." title="I did 5 sequential fixes." width="128">
<strong>I did 5 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/c59e77e13d3133f1b168cfe2c30d24c195c025ed">c59e77e</a>: fix: implement correct dict.clear() method behavior

Fixed dict.clear() to properly mutate dictionary in-place instead of
returning a new empty dict.

Changes:
- Modified core/dict_methods.go lines 189-198
- Now iterates through all keys and deletes them
- Returns None (Python standard behavior)
- Properly clears the dictionary in-place

Before: d.clear() returned NewDict() (wrong - doesn't modify original)
After: d.clear() deletes all entries from d and returns Nil (correct)

Testing:
- CPython test_dict.py now passes tests 1-10 (was failing at test 5)
- dict.pop(), dict.popitem(), dict.clear() all work correctly
- Go unit tests: all pass ✓
- M28 test suite: 60/60 pass ✓
- CPython tests: 3/4 pass (75%)

Closes M28-38ee
- <a href="https://github.com/mmichie/m28/commit/c794d6779307a69fc59d667610d1ba1e869e6eaa">c794d67</a>: fix: unwrap LocatedValue in star unpacking and walrus operator

Fixed critical LocatedValue unwrapping issues in eval/util.go:

- Star unpacking (line 798-803): Added unwrapLocated() call and proper
  error handling before type assertions. Now uses GetItemAsSymbol()
  smart accessor for safer access.

- Walrus operator (line 1375): Replaced direct type assertion with
  GetItemAsSymbol() smart accessor to properly handle LocatedValue.

These fixes eliminate crash risks when LocatedValue wrappers are
present in parsed expressions. Starred assignment tests now pass.

Closes M28-d932
- <a href="https://github.com/mmichie/m28/commit/bf314c98bdeb6d99177bfaf18428b369fcbaa87f">bf314c9</a>: fix: unwrap LocatedValue in starred unpacking assignment

Fixed starred unpacking (PEP 3132) to properly unwrap LocatedValue before
type checking, enabling syntax like:
  first, *rest = [1, 2, 3, 4]
  callable_obj, *args = arguments

Key changes in eval/util.go:
- Unwrap target before checking if it's the star index
- Unwrap star variable name before extracting symbol
- Unwrap target before checking if it's a regular symbol or dot notation
- Add proper error messages with type information

This fixes unittest.assertRaises which uses:
  callable_obj, *args = args

Tests now passing:
- CPython test/test_bool.py (31 tests)
- unittest.assertRaises with various arguments
- Starred unpacking in all contexts
- <a href="https://github.com/mmichie/m28/commit/6a5211e2710078774720f84ffe29ed0b6dd32b5c">6a5211e</a>: fix: set submodules as attributes on parent packages in sys.modules

When loading dotted module names like 'test.test_bool', the import system
now sets the submodule as an attribute on the parent package module, allowing
code like sys.modules['test'].test_bool to work.

Key changes:
- registerInSysModules/registerModuleInSysModules now check if a parent
  package exists in sys.modules and set the child module as an attribute
- Only set child on existing parents (don't create placeholder parents)
- Check if existing child is module-like before overwriting

This fixes the error "'dict' object has no attribute 'test_bool'" when
unittest tries to access submodules via parent packages.

Prevents two bugs:
1. Creating empty placeholder parent modules that block real module loading
2. Overwriting real attributes (like collections.namedtuple) with failed
   submodule load attempts
- <a href="https://github.com/mmichie/m28/commit/f3501f91182b1571e5fe8f263ead02ee7a300994">f3501f9</a>: fix: unwrap LocatedValue in generator yield detection and transformation

Fixed two related bugs that prevented generator functions from working correctly:

1. containsYield() wasn't unwrapping LocatedValue before checking if an element
   was a yield symbol, causing generator functions to not be detected and wrapped
   as GeneratorFunction

2. transformToSteps() wasn't unwrapping LocatedValue before processing nodes,
   causing yield statements to not be transformed into StepYield execution steps

These fixes enable:
- Generator functions to be properly detected and wrapped
- Yield statements to properly pause generator execution
- Context managers (@contextmanager) to work correctly
- unittest.main() to initialize and run tests

The root cause was that Python's parser wraps AST nodes in LocatedValue for
source location tracking, but the generator infrastructure wasn't accounting
for this wrapping when detecting and transforming yield statements.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>