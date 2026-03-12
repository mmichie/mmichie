<img src="https://my-badges.github.io/my-badges/fix-3.png" alt="I did 3 sequential fixes." title="I did 3 sequential fixes." width="128">
<strong>I did 3 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/plx/commit/a273b1d84ab93650dcba19cebd552ffb782da564">a273b1d</a>: fix: use chafa in tmux, Kitty graphics in direct terminal only

Kitty graphics protocol through tmux passthrough has fundamental
issues (images persist across tab switches, cursor doesn't advance).
Fall back to piping PNG through chafa when $TMUX is set. Kitty
protocol is used only in direct terminal sessions where it works
perfectly.
- <a href="https://github.com/mmichie/plx/commit/844236e0045b1fa1554f5240bc96485cfc2db7f7">844236e</a>: fix: add q=2 (quiet) to suppress Kitty graphics response
- <a href="https://github.com/mmichie/plx/commit/e1f2541c7d1e91488f4862c58c53d7dffac18104">e1f2541</a>: fix: tmux passthrough for Kitty graphics protocol

Detect $TMUX and wrap each Kitty graphics chunk in DCS passthrough
(ESC Ptmux; ... ESC \) with inner ESC bytes doubled per tmux spec.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>