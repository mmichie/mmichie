<img src="https://my-badges.github.io/my-badges/fix-3.png" alt="I did 3 sequential fixes." title="I did 3 sequential fixes." width="128">
<strong>I did 3 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/lima/commit/b95e20b07b0759af2f9c66faebf2a25d3a66ff9c">b95e20b</a>: fix(ui): use table.KeyMap for proper vim-style navigation

Configured the table's native KeyMap instead of manually converting keys:

- Set km.LineUp to "k" and "up"
- Set km.LineDown to "j" and "down"
- Set km.GotoTop to "g" and "home"
- Set km.GotoBottom to "G" and "end"
- Set page up/down to ctrl+u/d and pgup/pgdn

This is the correct way to add custom keybindings to bubbles table.
The table component will now properly handle j/k navigation.
- <a href="https://github.com/mmichie/lima/commit/91c12f06346541e79b1c47e638391dacae488c14">91c12f0</a>: fix(ui): add vim-style j/k navigation and enable table focus

Added support for vim-style navigation and improved selection visibility:

- Map j/k keys to down/up arrow keys for familiar vim navigation
- Call table.Focus() to enable selection highlighting
- Used fresh lipgloss.NewStyle() for Selected and Cell styles

Now j/k should work in addition to arrow keys, and the selected row
should be highlighted with the TP7 cyan background (black text on cyan).
- <a href="https://github.com/mmichie/lima/commit/c3d1dcf86f8552f7f1025a8a6a67064d57651417">c3d1dcf</a>: fix(ui): enable table scrolling by properly delegating key events

Fixed keyboard navigation in the transactions table:

- Moved table.Update() delegation to handle all non-enter key events
- This allows the table component to handle j/k, arrows, pgup/pgdn, home/end, g/G
- Only intercept 'enter' for categorization functionality
- Category picker still handles its own keys when active

The bubbles table component has built-in navigation that we need to
delegate to - now keyboard scrolling works properly.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>