<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/lima/commit/21dc424fba6c3fb123f27a283fa333370fa8c505">21dc424</a>: fix: resolve test failures and linter errors

- Remove redundant newlines from fmt.Println in categorizer-demo
- Fix TestCategorizer_Feedback by disabling LearnFromEdits in test config
  to avoid trying to save to non-existent directory
- <a href="https://github.com/mmichie/lima/commit/59c3a411ddae5466c4615f3510fc5804116849f9">59c3a41</a>: fix(ui): show transactions before window resize

Set default maxVisible to 10 if height not initialized yet.
This fixes the issue where transactions view shows count but
no transactions until after a window resize event.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>