<img src="https://my-badges.github.io/my-badges/fix-4.png" alt="I did 4 sequential fixes." title="I did 4 sequential fixes." width="128">
<strong>I did 4 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/907f7ae3aab90fd8e3d89d3d79497a38826b2617">907f7ae</a>: fix(errors): add Unwrap method to EvalError for proper error chain handling

EvalError was missing the standard Go Unwrap() method, which prevented
errors.Is/As from finding wrapped exception types like TypeError through
the error chain. This caused exceptions raised from within __new__ or
__init__ to be caught as generic Exception instead of their actual type.

Now try/except TypeError correctly catches TypeErrors raised from
super().__new__() and similar nested calls.
- <a href="https://github.com/mmichie/m28/commit/e44956713ad1f44a9d94ff9959ee096adb5637d0">e449567</a>: fix(super): correct method binding for __new__ and __init_subclass__

Fix super() no-args in nested class contexts by:
1. Setting __class__ in classBodyCtx when defining a class, ensuring
   methods capture the correct class for super() resolution
2. Passing classBodyCtx (not parent ctx) to createMethod, so closures
   capture the defining class's __class__
3. Removing code that copied __class__ from call context to function
   environment - this was overwriting the correct captured value
4. Checking cls parameter before self in superForm, so __new__ methods
   in nested classes use the local cls, not outer method's self

Before: super() in nested class __new__ used outer class's self
After: super() correctly uses the nested class's cls parameter
- <a href="https://github.com/mmichie/m28/commit/28f25a51e72b5c58461388e116bbfb1e17e678bc">28f25a5</a>: fix(class): add constructor argument validation for object.__new__ and object.__init__

Python requires that classes without custom __init__ or __new__ reject extra
arguments. This implements the validation logic:

- object.__new__: accepts extra args only if class has custom __init__ but no
  custom __new__
- object.__init__: accepts extra args only if class has custom __new__ but no
  custom __init__
- Class.CallWithKeywords: rejects args if neither __init__ nor __new__ is
  overridden

Error messages match Python's format:
- "C() takes no arguments" when neither is overridden
- "object.__new__() takes exactly one argument (the type to instantiate)"
- "C.__init__() takes exactly one argument (the instance to initialize)"
- <a href="https://github.com/mmichie/m28/commit/9c1d9447c0eee082beb2a1b8c5e81e66cd2e8b4d">9c1d944</a>: fix(class): improve metaclass protocol for **kwargs, *bases, and non-class bases

- Unwrap LocatedValue when extracting metaclass from **kwargs
- Track explicit base values separately for metaclass.__new__ calls
- Allow non-class bases (integers, etc.) that metaclass will handle
- Add RangeValue support for *range() unpacking in class definitions
- Collect non-metaclass kwargs from **d and pass to metaclass.__new__
- Extract actual key names from internal s:prefix format


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>