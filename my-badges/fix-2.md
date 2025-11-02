<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/m28/commit/5d40c0d714647a47e6871aa565c2dadc90e88634">5d40c0d</a>: fix: add optional start/end parameters to str.find() method

- Changed str.find() to accept 1-3 arguments (sub, start, end)
- Matches CPython signature: str.find(sub, start=0, end=len(s))
- Fixes sysconfig.py compatibility where it calls find() with position args
- <a href="https://github.com/mmichie/m28/commit/647d40f01725e976d5b8c3e7f800a6e3c1990352">647d40f</a>: fix: make os.environ a dict and add sys._framework

- Changed os.environ from function to dict value for CPython compatibility
- Python's os.py expects environ to be dict-like, not callable
- Added sys._framework attribute (False for non-framework builds)
- Fixes sysconfig.py import issue where environ.get() was failing


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>