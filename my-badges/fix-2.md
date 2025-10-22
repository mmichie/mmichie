<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/4d6fc12fb5507cbf9167cc7a33155eb5625e59c1">4d6fc12</a>: fix: allow super() to find methods in root metaclass

- Modified Super.GetAttr to check s.Class itself when it has no parents
- Fixes super().__new__() calls in metaclasses that inherit from type
- type is the root metaclass with no parent, but has __new__ method
- Allows abc.ABCMeta and other metaclasses to call super().__new__()
- Resolves 'super has no attribute __new__' error in metaclass creation
- <a href="https://github.com/mmichie/m28/commit/4275dc5fc539bbb879488fbb51a5c1430ea3bc9b">4275dc5</a>: fix: bind instance to methods returned by super().GetAttr

- Modified Super.GetAttr to bind methods to instance using BoundInstanceMethod
- Removed incorrect check in s.Class itself (super should skip current class)
- Now super().__init__() correctly passes self to parent __init__
- Fixes 'missing required argument: self' error in unittest.loader
- Matches Python behavior where super() methods are auto-bound


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>