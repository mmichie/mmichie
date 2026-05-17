<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/plx/commit/b992969856f1d709485f6f881f160720c6e97cb7">b992969</a>: fix(custom_command): replace `echo -n ''` with `true` in empty-output test

The 5x test stress loop on macos-latest surfaced a flake in
`run_empty_output_returns_none`: `echo -n ''` is non-portable — some
shells print `-n` as a literal when the `-n` flag is followed by an
empty argument, which causes the test to receive output and fail the
`is_none()` assertion.

Switch to `true`, which always succeeds and writes nothing. This
mirrors the `false` used in the adjacent `run_failing_command_returns_none`
test.
- <a href="https://github.com/mmichie/plx/commit/a23ca592dc6d02ce68c136363530df2ceb215a63">a23ca59</a>: fix: resolve clippy lints introduced in rust 1.95

CI's stable rustc (1.95) flags lints that local 1.94 doesn't yet:

- `clippy::map_unwrap_or` on `Result::map(..).unwrap_or(..)` — rewrite as
  `map_or` (and `is_ok_and` for the bool-returning case in tmux_title).
  Hits in repo_status.rs, segments/git.rs, segments/tmux_title.rs, and
  weather/mod.rs.
- `clippy::duration_suboptimal_units` on `Duration::from_secs(60)` —
  switch to `Duration::from_mins(1)` in health/cache.rs tests.
- `clippy::unnecessary_trailing_comma` in `write!` invocations in
  segments/aws.rs and segments/git.rs.
- `unused_mut`/`dead_code` from cfg-gated macOS code: gate `mod cache`
  to `#[cfg(target_os = "macos")]` (it's only consumed by the macOS
  checks today), and pair the unused `mut names` with
  `#[cfg_attr(not(target_os = "macos"), allow(unused_mut))]`.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>