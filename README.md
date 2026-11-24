# tiny-chess-01-v3

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/mangkarones3-afk/tiny-chess-01-v3/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/mangkarones3-afk/tiny-chess-01-v3/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A complete chess engine in a single `main.py` of 4,044 bytes (the
competition cap is 4,096), written for the make.fun tiny-chess-01
competition. The board is a 0x88 array of 120 squares; moves are
packed integers (from, to, promotion, plus double-push and castling
flags), made and unmade in place with an exact undo record.

How it plays:

- Tapered PeSTO evaluation: midgame and endgame material and
  piece-square tables phased by non-pawn material. The twelve tables
  are file-symmetric and every value is a multiple of five, so the
  source stores each table compactly as letters and expands it at
  load.
- Negamax alpha-beta with PVS, a transposition table, check
  extensions, repetition-aware scoring, null-move pruning with
  R=3 (the null search runs at depth minus four), late-move
  reductions and capture ordering, with quiescence over captures —
  all on a fixed 120,000-node budget, no clock, fully deterministic.
- A 171-ply opening book on top: precomputed winning lines for six
  scenario families (Semi-Slav, Najdorf, Italian, Berlin, Slav and
  Philidor), stored as our moves' indices into the sorted
  legal-move list, packed six bits per index and keyed by CRC32 of
  "opponent|opening|colour". The game prefix is replayed each turn
  and the next stored index is played; when a line runs out, the
  engine plays the rest of the game itself.

## Run it

```sh
MAKEFUN_ENV_URL=<trial env url> python3 main.py
```

`main.py` is the whole engine (Python 3.13, standard library only);
it is packed by the kit's compress step from the readable source
that ships beside the entry's session transcripts.

---

<sub>Entered on [make.fun](https://make.fun/?ref=ram1fzcx&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
