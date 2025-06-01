<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/39bfb9867cf57dd6bf9ed626294395ba6b789755">39bfb98</a>: fix: Make 'in' operator check dictionary keys correctly

- Update InFunc to use ValueToKey for proper key comparison
- Now supports all hashable types as dictionary keys (strings, numbers, bools, tuples)
- Document known issue with empty literals in class __init__ methods
- <a href="https://github.com/mmichie/m28/commit/f5375639b799fdc5c1c11ba714d8f690b70c06ea">f537563</a>: fix: Support class variables and fix example code for M28 compatibility

- Add SetAttr method to Class type for class variable assignment
- Support = form in class definitions for class variables (per CLAUDE.md)
- Fix conditionals.m28: correct elif syntax, replace lambda/any with loops
- Fix exceptions.m28: adapt to M28's simpler exception syntax
- Fix class examples: correct default params, comment out unsupported features
- Fix calculator.m28: use (list) instead of [] in class __init__
- Remove parentheses from property access in module examples
- Improve example success rate from 48% to 65.6%


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>