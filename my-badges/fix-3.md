<img src="https://my-badges.github.io/my-badges/fix-3.png" alt="I did 3 sequential fixes." title="I did 3 sequential fixes." width="128">
<strong>I did 3 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/cardsharp/commit/77c1cbd4b1e47eaff6c0a998056a51b9c825b5c5">77c1cbd</a>: fix: handle edge cases in surrender logic

- Only force surrender actions if allowed by rules and is a valid action.
- Verify surrender payouts only if surrender is allowed in the rules.
- Warn if surrender action is recorded but not allowed by the rules.
- <a href="https://github.com/mmichie/cardsharp/commit/917023392c152bfa23f1f5d8e304316f4104add4">9170233</a>: fix: enhance surrender payout verification

- Add support for testing odd-numbered bets to verify accurate surrender payouts.
- Improve surrender payout validation by checking for floating-point precision and adding detailed error messages.
- Force surrender actions more frequently during testing to increase test coverage.
- Add summary report for surrender test results, including odd-bet verification status.
- Bump version to 0.5.0.
- <a href="https://github.com/mmichie/cardsharp/commit/a887e452caf9fe05b8af8424f15dec285211f65e">a887e45</a>: fix(blackjack): handle surrender with odd bets

- Use floating-point division when calculating surrender amount.
- Add tests for surrender with odd bet amounts.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>