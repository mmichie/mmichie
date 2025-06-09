<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/1d4c7435fef690263ae15472f71815aaa427e9aa">1d4c743</a>: fix: allow objects with just GetAttr method to use dot notation

- Modified eval/dot_notation.go to check for both full Object interface
  and objects that only implement GetAttr method
- Added comprehensive tests for number dunder methods
- Added tests for arithmetic operator edge cases including division by zero
  and type errors

This fix enables dot notation access for types that implement GetAttr
without requiring the full Object interface.
- <a href="https://github.com/mmichie/m28/commit/aadf8edf5206f03cd36880f8cd6de5bc40fbde97">aadf8ed</a>: fix: correct variadic arithmetic operations with operator overloading

- Fixed bug where (+ a b 1) returned 3 instead of 4
- Issue was that __add__ method was being used for built-in number types
- Now built-in types (NumberValue, StringValue, ListValue) use optimized fast paths
- Operator overloading only used for custom objects that define dunder methods
- Applied defensive fixes to all arithmetic operators (-, *, /) to prevent future issues
- Added comprehensive tests for variadic operations and operator overloading
- Updated ROADMAP to note that only __add__ is currently registered for numbers

This ensures arithmetic operations work correctly for both regular operations
and custom classes with operator overloading.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>