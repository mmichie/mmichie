<img src="https://my-badges.github.io/my-badges/fix-3.png" alt="I did 3 sequential fixes." title="I did 3 sequential fixes." width="128">
<strong>I did 3 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/d977bde301a0cc77f0ce807cd98ee410ca6bbf6d">d977bde</a>: fix: simplify embed package to use Context directly

- Remove Environment usage in favor of Context-only approach
- Fix GetValue to use ctx.Lookup instead of non-existent Get method
- This properly connects builtins to the evaluation context

The embed package now works correctly for embedded M28 evaluation.
- <a href="https://github.com/mmichie/m28/commit/e1dbf81203e092a6c22d64bc15c6e4177e99edb4">e1dbf81</a>: fix: properly initialize and use context in embed package

- Store the initialized context with builtins in M28Engine
- Initialize basic values (true, false, nil) before registering builtins
- Use child context for evaluation to avoid polluting global state
- Register shell functions in context instead of environment
- Fix SetupBuiltins to be a no-op for backward compatibility

This fixes the duplicate registration error and properly connects
builtins to the evaluation context.
- <a href="https://github.com/mmichie/m28/commit/f99e6fe67b3c24bf7df21bc60827cd5188299018">f99e6fe</a>: fix: remove duplicate builtin registration in embed package

The embed package was calling both environment.SetupBuiltins() and
RegisterAllBuiltins(), causing duplicate registrations in the global
builtin registry. Since SetupBuiltins() creates a throwaway context
that's never used, we remove this call and only keep the direct
RegisterAllBuiltins() call with the proper context.

This fixes the 'iter already registered' error in gosh.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>