<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/c90fb52f14adf423d1e49483a4753e2d7834737f">c90fb52</a>: fix: make test.sh fail on any test failure

The test script was returning exit code 0 (success) even when tests
failed, as long as the success rate was 90% or higher. This allowed
failing tests to slip through the pre-commit hooks.

Now the script only returns 0 when ALL tests pass. Any test failure
will cause it to return exit code 1, ensuring the pre-commit hook
properly blocks commits with failing tests.
- <a href="https://github.com/mmichie/m28/commit/9c14f4531732f680003d20dcada15b7c5746e849">9c14f45</a>: fix: update functional example to use new multiple assignment syntax

The functional_basics.m28 example was using the old Fibonacci-style
assignment pattern (= a b expr) which is no longer supported. Updated
to use the new syntax (= a b [b (+ a b)]) which properly unpacks the
list into the two variables.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>