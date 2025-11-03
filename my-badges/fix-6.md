<img src="https://my-badges.github.io/my-badges/fix-6.png" alt="I did 6 sequential fixes." title="I did 6 sequential fixes." width="128">
<strong>I did 6 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/d8c02b18a78a5f4c34090aad9ec6e2fb09917358">d8c02b1</a>: fix: make iterators iterable by implementing Iterator() method

In Python, iterators are themselves iterable - calling iter() on an
iterator returns the iterator. This means functions like enumerate()
must accept both sequences (lists, tuples) and iterators.

Previously, enumerate() used types.RequireIterable() which checked for
the core.Iterable interface. Iterators like ListIterator didn't implement
this interface, causing enumerate(iter(list)) to fail with:
  TypeError: enumerate() argument: expected iterable, got list_iterator

Fix: Add Iterator() method to ListIterator that returns an adapter
implementing core.Iterator. This makes ListIterator implement core.Iterable.
The adapter converts between the protocols-style iterator (Next() returns
(Value, error)) and core.Iterator (Next() returns (Value, bool)).

This fixes unittest.TestProgram which calls enumerate() on iterators.
- <a href="https://github.com/mmichie/m28/commit/667acd1fc8c1856bb6b6429c49d14326de812517">667acd1</a>: fix: make all instances hashable by default

In Python, all class instances are hashable by default (using their
object ID for hashing), unless they define __hash__ = None or define
__eq__ without defining __hash__.

Previously, M28 only made instances of int/str/bool subclasses hashable,
which broke argparse's _add_container_actions() method that uses action
instances as dictionary keys.

Fix: Make all instances hashable by default. ValueToKey already handles
this correctly by using pointer address (p:%p) for regular instances.

This fixes: unhashable type: '_StoreConstAction' when creating
ArgumentParser with parents parameter.
- <a href="https://github.com/mmichie/m28/commit/8a6e24b85ec762e066516faa13c2ad923f890e92">8a6e24b</a>: fix: support chained assignment in class bodies

Chained assignments like 'a = b = c = 10' are parsed as nested
assignments: (= a (= b (= c 10))). When evaluating the RHS in a
class body, the inner assignments (b = c = 10) would define variables
in classBodyCtx but not add them as class attributes.

Fix: Track variables before evaluating RHS, then add any new variables
(created by chained assignment) as class attributes.

This fixes unittest.TestProgram which uses:
  failfast = catchbreak = buffer = progName = warnings = testNamePatterns = None
- <a href="https://github.com/mmichie/m28/commit/6921ae6aca6439951effffa246516ceabd533308">6921ae6</a>: fix: use SetValue for _sre pattern dict keys to ensure proper formatting

When _sre.compile() created pattern dicts using Set(), keys were stored
without the 's:' prefix. But dict copying in evaluator.go uses GetValue()
which expects keys with the prefix, causing copied dicts to be empty.

This caused argparse's re.compile() caching to fail - the second call
would return an empty dict without the .match() method, leading to
'dict object has no attribute match' errors.

Fix: Use SetValue(StringValue(key), value) instead of Set(key, value)
to ensure keys are stored with proper 's:' prefix, making them accessible
via GetValue() during dict copying.
- <a href="https://github.com/mmichie/m28/commit/cbd0a95b68cca227103552eb810f92d124d6a873">cbd0a95</a>: fix: create fresh copies of dict and set literals on each evaluation

Dict and set literals were being reused across multiple evaluations,
causing shared mutable state bugs. When code like 'registry =
self._registries.setdefault(key, {})' was executed multiple times
with different keys, the same dict object was returned.

This fix mirrors the existing behavior for empty list literals,
creating a fresh copy of the dict/set on each evaluation to avoid
shared mutable state.

Fixes argparse registry contamination where _registries['action']
and _registries['type'] were pointing to the same dict object.
- <a href="https://github.com/mmichie/m28/commit/238de185e1a2431742d00ab7ced98b9562182471">238de18</a>: fix: properly format dict() keyword argument keys

Fixed dict(other_dict, key=value) to use SetValue() instead of Set()
for keyword arguments. This ensures keys are properly formatted with
ValueToKey so the 'in' operator and subscript access work correctly.

This fixes the argparse issue where dict(kwargs, dest=dest, ...)
created a dict with 'dest' that wasn't accessible via 'in' operator.

Same root cause as the kwargs dict issue fixed earlier - when creating
dicts from keyword arguments, keys must be formatted consistently.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>