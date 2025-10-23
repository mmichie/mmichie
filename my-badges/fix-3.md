<img src="https://my-badges.github.io/my-badges/fix-3.png" alt="I did 3 sequential fixes." title="I did 3 sequential fixes." width="128">
<strong>I did 3 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/1f518e9e4a0d05bbabdf0c0406b5527e5aba6e0d">1f518e9</a>: fix: support conditional method definitions at class level

Class body improvements:
- Handle `if` statements at class level to capture conditional method definitions
- Methods defined inside `if` blocks at class level now properly added to the class
- This fixes threading.py where `_set_native_id` is conditionally defined

Weakref module enhancements:
- Add getweakrefcount() and getweakrefs() functions
- Add ReferenceType, ProxyType, CallableProxyType classes
- Add _remove_dead_weakref() internal function
- These additions improve weakref module compatibility

This change enables CPython's threading.py to progress much further
during import, though some issues remain with function __hash__ support.
- <a href="https://github.com/mmichie/m28/commit/ce98698645b98f3fa47f114d1d4a58a96bcfea85">ce98698</a>: fix: fix ThreadLock deadlock and improve set mutating methods

Threading improvements:
- Fix ThreadLock.__enter__ deadlock by adding missing mutex unlock
- Add _HAVE_THREAD_NATIVE_ID constant to _thread module

Set method fixes:
- Fix set.add(), remove(), discard(), pop(), clear() to mutate in-place
- Add missing mutating methods: update(), difference_update(),
  intersection_update(), symmetric_difference_update()
- Support iterables (not just sets) in update methods for Python compatibility

These changes fix the threading module deadlock and improve CPython
standard library compatibility, particularly for WeakSet operations.
- <a href="https://github.com/mmichie/m28/commit/7c1e7c97f79b92844efeb1e9f7e211142fcce6ee">7c1e7c9</a>: fix: list.pop() raises IndexError when empty instead of generic error

Changed list.pop() on empty list to raise IndexError(-1, 0) instead of
generic error. This fixes Python code that catches IndexError, particularly
WeakSet._commit_removals() which uses try/except IndexError to detect
empty list.

This allows threading module to initialize successfully - test_bool.py
now loads all imports without errors (though it hangs during execution).


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>