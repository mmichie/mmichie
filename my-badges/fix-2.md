<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/4a7e48a227d9c91e0190b2f9d014946e4339e718">4a7e48a</a>: fix: add bytes/bytearray iteration support and fix iter() comparison bug

Problem 1: iter() panicked with "comparing uncomparable type core.ListValue"
Root cause: Using == to compare values, which fails for slice types like ListValue

Problem 2: bytes and bytearray were not iterable
Root cause: No iterator implementations in protocols package

Solutions:
1. Removed unsafe == comparison in iter()
   - Instead of checking `if iter == obj`, check if returned value has __next__
   - This avoids comparing uncomparable types like slices

2. Added BytesIterator and ByteArrayIterator
   - Implemented in core/protocols/iterable.go
   - Returns each byte as a NumberValue (Python behavior)
   - Added to GetIterableOps switch statement

Changes:
- builtin/iteration_protocol.go: Removed problematic == comparison
- core/protocols/iterable.go: Added BytesIterator, ByteArrayIterator
- core/protocols/iterable.go: Updated GetIterableOps to handle bytes types

Impact:
- iter([1,2,3]) now works without panic
- iter(b"hello") now returns <bytes_iterator>
- iter(bytearray()) now returns <bytearray_iterator>
- _collections_abc.py progresses further (new error: "class has no attribute 'register'")

All 54 tests passing.
- <a href="https://github.com/mmichie/m28/commit/c27b3305c4a131fd80c142ec7167acda7e5b8fb7">c27b330</a>: fix: convert type from builtin function to inheritable metaclass

Problem: Python code `class ABCMeta(type):` failed with error:
"parent must be a class, got *core.BuiltinFunction"

Root cause: type was implemented as a BuiltinFunction, which cannot
be used as a base class for metaclasses.

Solution: Created TypeType wrapper (similar to StrType, DictType) that:
1. Wraps *core.Class to support inheritance
2. Implements Call() to provide type() function behavior
3. Implements GetClass() to return the class for inheritance

Changes:
- Added TypeType struct with embedded *core.Class
- Implemented Call() for type(obj) and type(name, bases, dict) forms
- Implemented GetClass() to support class inheritance
- Updated RegisterTypes to use TypeType instead of BuiltinFunction

Impact:
- type(obj) still works correctly to get object types
- class Meta(type): now works for creating metaclasses
- _py_abc.ABCMeta(type) loads successfully
- unittest import progresses further (new error: "error in iter")

All 54 tests passing.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>