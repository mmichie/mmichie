<img src="https://my-badges.github.io/my-badges/fix-5.png" alt="I did 5 sequential fixes." title="I did 5 sequential fixes." width="128">
<strong>I did 5 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/ad67b8d5cfe75e7ce0f8da6807c1cbb87148e924">ad67b8d</a>: fix: correct setattr to properly bind __setattr__ descriptor for classes

When setattr(cls, name, value) is called on a class object, the
__setattr__ descriptor returned by cls.GetAttr() is not automatically
bound. The builtin setattr() was calling it with only (name, value)
args, but __setattr__ expects (self, name, value).

The fix checks if the __setattr__ method is already bound (via
BoundInstanceMethod or BoundMethod). If not bound, prepends the object
as the first argument to properly bind self.

This fixes functools.total_ordering decorator which uses setattr() to
add missing comparison methods to classes with __slots__. Previously
failed with "descriptor '__setattr__' requires an instance" error.

Fixes imports for modules using @functools.total_ordering decorator
like ipaddress.py which is needed by test.support and unittest.mock.

Test suite now 98% passing (59/60 tests). Remaining failure is a
different issue with package submodule imports (test.support.os_helper).
- <a href="https://github.com/mmichie/m28/commit/6862a25e752113e44ce8f8967acfc2e211f98de4">6862a25</a>: fix: correct superForm 2-arg case to use specified class not parent

Fixed superForm to pass the specified class (not its parent) when creating
Super objects for the explicit super(Class, obj) form. Super.GetAttr already
handles skipping the current class in MRO search, so we should pass the class
itself.

This fixes super(Dog, self).__init__() which was incorrectly creating
Super(Animal, instance) instead of Super(Dog, instance), causing it to
skip Animal and only find object.__init__.
- <a href="https://github.com/mmichie/m28/commit/4104f8bcf61cd279e3becd3947551ffbe7a77b25">4104f8b</a>: fix: correct super() and fix metaclass infinite recursion

Fixed multiple issues with super() implementation:

1. superForm now uses current class for instance methods (has self) but
   parent class for class/metaclass methods (no instance). This hybrid
   approach fixes super().__init__() for regular classes while avoiding
   infinite loops in complex metaclass hierarchies.

2. Fixed infinite recursion in Super.GetAttr fallback path by checking
   direct Methods/Attributes dicts instead of calling GetAttr/GetMethodWithClass
   which can trigger descriptor protocol and cause recursion.

3. Added __class__ attribute support to Super objects to prevent lookup
   loops when code checks super().__class__.

4. Preserved __class__ in BoundInstanceMethod.Call to avoid overwriting
   it during super() chains. Added nil checks to prevent panics.

These fixes eliminate the infinite loop when importing typing.py. The module
now fails with a different error (_CallableType.__init__) which indicates
progress past the original _DeprecatedGenericAlias issue.

Test results:
- Simple super() with Child(Base) works correctly
- Metaclass super() works without infinite loops
- typing.py progresses further (no longer hangs)
- <a href="https://github.com/mmichie/m28/commit/6ecc9f7d40b65bbdba2f269aa395d66f2b735152">6ecc9f7</a>: fix: super() should skip current class in MRO search

When super() is used in a method, it should search for parent class methods
starting AFTER the defining class, not including it.

The Bug:
  When Child inherits Middle's __init__, and Middle.__init__ calls super().__init__(),
  the MRO search was starting from Middle (inclusive), which would find Middle.__init__
  again, creating infinite recursion or wrong method lookup.

The Fix:
  Changed Super.GetAttr to search starting from startIdx + 1 instead of startIdx.
  This skips the current class and only searches parent classes.

Example that now works:
  class Base:
      def __init__(self, x): self.x = x
  class Middle(Base):
      def __init__(self, x, y):
          super().__init__(x)  # Now correctly calls Base.__init__
          self.y = y
  class Child(Middle): pass
  obj = Child(1, 2)  # Success!

This fixes typing.py's _CallableType(_SpecialGenericAlias) which uses
super().__init__() with multiple inheritance.
- <a href="https://github.com/mmichie/m28/commit/8e455bc848e503db06c185892cca454a6330b601">8e455bc</a>: fix: implement breadth-first MRO for multiple inheritance

Change GetMethod, GetMethodWithClass, and GetClassAttr to use breadth-first
search instead of depth-first. This matches Python's C3 linearization behavior
more closely.

The issue: With depth-first search, when a class has multiple parents like
Child(Mixin1, Parent), it would search Mixin1's entire hierarchy (including
object) before checking Parent. This meant object.__init__ would be found
instead of Parent.__init__.

The fix: Use breadth-first search - check all direct parents first before
checking grandparents. This ensures Parent's methods are found before object's
built-in methods.

Example that now works:
  class Mixin1: pass
  class Parent:
      def __init__(self, a): self.a = a
  class Child(Mixin1, Parent): pass
  obj = Child(10)  # Now correctly calls Parent.__init__

This fixes the _CallableType(_SpecialGenericAlias) instantiation in typing.py
which uses multiple inheritance.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>