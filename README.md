# The Quest

A single-page, no-build text/image adventure. Open `index.html` in a browser to play — no server or compile step required.

## How it works

- Each action moves the player 1-3 distance toward a win at 20 total distance (see `CONFIG.winDistance`).
- Each choice has a fixed, determined outcome (`outcome: { type, text }` — no randomness in *what* happens, only *how far* you travel). `type` is one of `normal`, `penalty`, `heal`, or `gameover`. The outcome text shows in a modal right after the choice and is also recorded in the journey log.
- `penalty` gives the player a penalty point (shown as a red X in the HUD); `heal` removes one. Reaching `CONFIG.maxPenaltyPoints` (default 3) ends the game, as does any `gameover` outcome directly.
- The journey is split into three zones by progress (`ZONES`: 0-7, 8-14, 15-20). Every location belongs to exactly one zone (`zone: 1|2|3`), and the next location is picked randomly — weighted by `weight` — only among locations in the player's current zone that the player hasn't visited yet (`state.visited`). If every location in a zone has already been visited, a repeat becomes unavoidable, but the game still avoids repeating the immediately-current room when there's any other option. Zone boundaries are drawn as tick marks on the progress bar, and crossing into a new zone pops up a modal (also tracked: whether the player ever gives crypto to a local Poofo there, which changes the win message).
- "Consult the local Poofo" can appear at a location (`CONFIG.poofoAppearChance`) and highlights a choice — correct or not, based on the picked hint's accuracy (`POOFO_HINTS`). The "correct" choice defaults to whichever has the most desirable outcome type (`OUTCOME_RANK`), or can be pinned explicitly with `poofoCorrect: true` on a choice.

## Where to edit content

Everything you'd want to fill in lives in the **CONTENT SECTION** near the top of the `<script>` block in `index.html`:

- `LOCATIONS` — name, description, image path, zone, weight, and choices (each with a fixed `outcome`) for each location. Give each new location an `id` of the form `loc_<camelCaseOfTheName>` (e.g. "Bear's room" → `loc_bearRoom`) — it just needs to be unique, nothing depends on numbering.
- `ZONES` — the three progress ranges (min/max) locations are grouped into.
- `POOFO_HINTS` — the hint texts Poofo can give and their accuracy.
- `END_TEXT` — win/game-over titles and messages.

The **ENGINE SECTION** below it (selection, state, rendering) shouldn't need changes for content edits.

## Images

Placeholder SVGs live in `images/`, one per location plus `win.svg` and `gameover.svg`. Replace them with real art using the same filenames, or update the `image` path on a location to point elsewhere.
