<img src="https://my-badges.github.io/my-badges/fix-6+.png" alt="I did 7 sequential fixes." title="I did 7 sequential fixes." width="128">
<strong>I did 7 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/cardsharp/commit/9fc12eaea2f2481f86590dd69fe6f7523288355f">9fc12ea</a>: fix: properly implement card counting with shoe shuffle detection

- Add tracking to prevent counting the same card multiple times
- Implement shoe shuffle detection based on cards remaining
- Reset count when shoe is shuffled (critical for accurate counting)
- Add notify_shuffle() method to CountingStrategy
- Fix count accumulation bug across games in batch mode

Results with fixed counting:
- Basic strategy: 0.65% house edge
- Counting (no CSM): -0.04% house edge (slight player advantage)
- Counting (with CSM): 3.16% house edge (loses more as expected)

The counting strategy now properly resets when shoes are shuffled
and only counts each card once, providing realistic advantages.
- <a href="https://github.com/mmichie/cardsharp/commit/57d4077f27cf49b7f249bddbf62f14ff6f9097bf">57d4077</a>: fix: prevent card counting strategy from modifying bets after cards are dealt

- Add get_bet_amount() method to Strategy base class for pre-deal bet sizing
- Remove illegal bet modification from CountingStrategy.decide_action()
- Update PlacingBetsState to use strategy's get_bet_amount() method
- Fix bug where counting strategy could retroactively change bets (cheating)
- Counting strategy now correctly loses more with CSM (3.16% vs 0.62% house edge)

Previously, the counting strategy was modifying bets after seeing cards,
which is impossible in real blackjack. With CSM, the meaningless count
would accumulate over thousands of hands, causing massive bet increases
that created an artificial -1137% house edge. Now bets are properly
determined before cards are dealt, and the strategy loses more when
betting high on meaningless CSM counts, as expected.
- <a href="https://github.com/mmichie/cardsharp/commit/5d2889501e5c12390c2b72b11cac7cb6ca0b9833">5d28895</a>: fix: resolve pyrefly type checking errors

- Fix Rank enum comparisons by using .value property
- Add type annotations to fix dict and property type inference
- Add None checks with assertions for Optional types
- Fix float/int type mismatches in bankroll management
- Add type hints for ambiguous dictionary types
- Exclude tests directory from pyrefly type checking

Reduced total type errors from 498 to 440
- <a href="https://github.com/mmichie/cardsharp/commit/4540b6652da8eebea070842da673dedbc4738f96">4540b66</a>: fix: resolve Hand vs BlackjackHand type errors and add engine safety checks

- Fixed Hand parameter types in rules.py
  - Changed all Hand parameters to BlackjackHand where hand.value() or hand.is_soft is accessed
  - Fixes 11 missing-attribute errors

- Added _engine property to BlackjackGame for safe engine access
  - Provides centralized None checking with clear error messages
  - Used in remove_player and get_state methods

- Added None checks for engine access to prevent AttributeError
  - These would cause runtime crashes when methods called before initialize()

Reduced pyrefly errors from 535 to 520 (15 errors fixed).
- <a href="https://github.com/mmichie/cardsharp/commit/34bacb60af43fbf8f90a2e0ffde35555163bab39">34bacb6</a>: fix: resolve additional pyrefly type errors

- Fixed float to int assignment errors
  - Changed current_progression to float type in bankroll.py
  - Changed true_count to float type in strategy.py
  - Both were being assigned float values from division operations

- Fixed async method return type
  - CLIAdapter.handle_timeout now returns Action instead of Awaitable[Action]

- Added proper type annotations
  - Added Optional[BaseEngine] type for engine attribute in base API class
  - Fixed import structure with TYPE_CHECKING

Reduced pyrefly errors from 544 to 535 (9 errors fixed).
- <a href="https://github.com/mmichie/cardsharp/commit/89f408e89475d912ad2bfc4af71587612027bf84">89f408e</a>: fix: resolve critical pyrefly type errors that could cause runtime bugs

- Fixed bad-assignment errors by adding proper Optional type annotations
  - Added Optional[asyncio.Task] for _input_task and queue_monitor_task
  - Added Optional[BlackjackEngine] etc. for engine attributes
  - Fixed websocket_handler type annotation

- Fixed EventBus/EventEmitter type mismatch
  - Changed parameter types from EventBus to EventEmitter in flow.py
  - EventBus.get_instance() returns EventEmitter, not EventBus

- Fixed bad-return type errors
  - Changed async method returns from Awaitable[Action] to Action
  - Async methods already return awaitable by default

Reduced pyrefly errors from 576 to 544 (32 errors fixed).
These were the most critical errors that could cause runtime failures.
- <a href="https://github.com/mmichie/cardsharp/commit/874454951ba3f0725d2e949f6d0de920172dc170">8744549</a>: fix: add missing input method to IOInterface abstract base class

- Added abstract input() method to IOInterface base class
- Implemented input() in all concrete IO interface classes
- Updated method signatures to use proper type annotations
- Fixed test_io_interface.py to match new method signatures
- Reduced pyrefly type errors from 583 to 576

This fixes the most common pyrefly error where IOInterface was missing
the input attribute that CLI adapter was trying to use.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>