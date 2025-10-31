<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/daf87a5763d069a2039c391712a37ffb94f3aa2c">daf87a5</a>: fix: add IntEnum._convert_() method for signal.py compatibility
- <a href="https://github.com/mmichie/m28/commit/79a680ecc460b54eabc681a1f42e64e7142f5174">79a680e</a>: fix: add CallWithKeywords overrides to all primitive type classes

Systematically fixed all primitive type classes that embed *core.Class
to override CallWithKeywords(), preventing them from being wrapped in
Instance objects when called through Python code.

The root cause was that these type classes override Call() to return
raw primitive values, but Class.CallWithKeywords() was calling the
generic NewInstance() which wraps primitives in Instance objects.

Fixed types:
- ListType, TupleType, DictType (collections.go)
- StrType, IntType, FloatType (types.go)
- PropertyType, StaticmethodType, ClassmethodType (decorators.go)

This fixes issues like:
- sys.intern() receiving Instance-wrapped strings instead of StringValue
- classmethod objects missing __func__ attribute
- Other primitive instantiation bugs in CPython stdlib

SetType and ByteArrayTypeClass were already fixed in previous commits.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>