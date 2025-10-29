<img src="https://my-badges.github.io/my-badges/fix-3.png" alt="I did 3 sequential fixes." title="I did 3 sequential fixes." width="128">
<strong>I did 3 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/7f7ce26967e8c0c6562f1cff20dffd58beaa9404">7f7ce26</a>: fix: replace shared EmptyList with fresh instances to avoid mutation bug

Fixed critical bug where EmptyList global was being mutated by append operations.
After importing `re`, all empty list literals shared the same mutable instance.

Changes:
- builtin/attributes.go: Return core.NewList() instead of core.EmptyList
- builtin/collections.go: Return core.NewList() for list() and ListType.Call()
- eval/evaluator.go: Return core.NewList() for empty list literals

This ensures each empty list is a fresh, independent instance.

File I/O test now fails with different error (list index out of range in fnmatch)
- <a href="https://github.com/mmichie/m28/commit/9ac639be92146f1ea17695d25b8a762af2bc6c96">9ac639b</a>: fix: support __doc__ on tuplegetter, bytes(frozenset), and strict str.join()

- Allow setting __doc__ attribute on TupleGetter for urllib.parse compatibility
- Add bytes() constructor support for SetValue and FrozenSetValue
- Fix str.join() to reject non-string elements (match CPython behavior)
- urllib.parse now imports successfully

Note: File I/O test still fails due to unrelated list literal transpiler bug
- <a href="https://github.com/mmichie/m28/commit/44602e6d8eed22c469e8b040363c9595ea455927">44602e6</a>: fix: don't invoke descriptor protocol on Class objects

Root cause: When accessing a class from a module (e.g., functools.cached_property),
dot_notation was checking if the CLASS had __get__ and calling it as a descriptor.
This caused cached_property.__get__(cached_property_class, functools, None) to be
called, which tried to access self.attrname on the CLASS instead of an instance.

Fixes:
1. Don't invoke __get__ when the value is a *core.Class (eval/dot_notation.go:71)
   - Classes themselves are not descriptors when accessed from modules
   - Only instances of descriptor classes should have __get__ invoked

2. Fix descriptor protocol to call __get__(instance, owner) with 2 args (dot_notation.go:92)
   - GetAttr returns a bound method, so we don't prepend the descriptor again
   - Was calling with 3 args (descriptor, instance, owner) causing 'too many args' error

Result: functools.cached_property can now be imported and instantiated!

Test Status: 59/60 passing (98%)
Note: Decorator syntax (@cached_property) not yet working - will be fixed in follow-up


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>