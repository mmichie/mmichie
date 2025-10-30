<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/c1659cf104fbdac1acd9495220e8ddabe8e04ef3">c1659cf</a>: fix: use GetValue/SetValue for dict operations in c_codecs

The c_codecs error handler registry was using Get(string) and Set(string),
which don't handle M28 value key semantics properly. This caused
"unknown error handler name" errors when loading the codecs module.

Changed to use GetValue(StringValue) and SetValue(StringValue, Value)
which properly convert string keys to the "s:key" format expected by
M28's dict implementation.

Also reverted the incorrect fix to containers.go Get/Set methods,
which should remain as low-level internal APIs that work with raw
string keys. The proper fix is for callers to use GetValue/SetValue
when working with M28 values.

This enables the codecs module to load successfully, which is required
for inspect and other stdlib modules.
- <a href="https://github.com/mmichie/m28/commit/c379f196be93b2ee83dbbad83851a34852af8dd5">c379f19</a>: fix: prioritize ModuleDict over Vars in lookup for dynamic globals

MAJOR FIX: Solves the closure/module dict lookup problem, enabling
importlib to load successfully!

## The Problem:
In Python, functions look up globals dynamically in module.__dict__:
```python
_weakref = None
def test(): return _weakref  # Dynamic lookup

module.__dict__['_weakref'] = actual_module
test()  # Should return actual_module, not None
```

M28 was storing variables in BOTH Context.Vars AND ModuleDict, but
lookups checked Vars FIRST, finding stale values.

## Root Cause:
1. Module-level vars stored in moduleCtx.Vars (line 116)
2. Private vars (_weakref) skipped ModuleDict sync (lines 126-128)
3. Lookups checked c.Vars before c.ModuleDict
4. Functions found stale values in outer.Vars

## The Fix:
Changed Context.lookupWithDepth() to check ModuleDict BEFORE Vars.

New lookup order:
1. Check c.ModuleDict (if exists) - authoritative source
2. Check c.Vars - for true locals
3. Recurse to outer scopes

This matches Python semantics: module.__dict__ IS the namespace.

## Additional Fix:
Auto-load _thread, _warnings, _weakref when importlib is loaded.
importlib._bootstrap._setup() expects these in sys.modules.

## Impact:
✅ importlib module loads successfully!
✅ Functions see dynamic updates to module globals
✅ importlib._setup() can inject _weakref dynamically
✅ Module attribute updates visible to all code

All 60 M28 tests passing.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>