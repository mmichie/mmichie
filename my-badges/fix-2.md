<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/mmichie/lima/commit/497af973a11b9ce6e5b1520a1acf51ed9965e33f">497af97</a>: fix(ui): prevent fullscreen wrapper from overriding table selection styles

The renderFullScreenContent() was wrapping the entire table output with a
lipgloss style that applied a blue background, which was overriding the
table's internal Selected row styling. Now skipping this wrapper for the
TransactionsView so the table's native cyan selection highlighting can
show through properly. Also made selection bold and ensured focus is
maintained after every table update.
- <a href="https://github.com/mmichie/lima/commit/bc1d88fadf685a143d77fb18a4dd973bd9f05f26">bc1d88f</a>: fix(ui): remove embedded colors from table data to fix selection highlighting

The previous approach of pre-coloring row data strings with lipgloss embedded
ANSI codes was preventing the Selected style background from showing through.
Now using plain strings and relying on Cell/Selected styles for coloring, which
allows the cyan selection bar to be visible when navigating with j/k.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>