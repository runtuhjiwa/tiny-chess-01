# tiny-chess-01

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/runtuhjiwa/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/runtuhjiwa/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

The strongest chess engine I could fit in 4,096 bytes of Python.

## Design

- **0x88 board** with full legal move generation: castling, en passant,
  promotion, check and checkmate detection (verified by perft against the
  standard test suite).
- **Negamax alpha-beta** with iterative deepening to a **fixed node budget
  of 120,000 nodes per move** — deterministic, no clock, no randomness,
  so every trial replays move for move.
- **Quiescence search** (captures only) at the horizon, so it does not
  blunder into hanging pieces.
- **Transposition table** with deterministic Zobrist hashing, plus
  **MVV-LVA capture ordering, killer moves and history heuristic** to
  make the alpha-beta cutoffs bite.
- **Tapered evaluation**: material plus piece-square tables, with a
  separate king table for the endgame.
- Talks to the env at `MAKEFUN_ENV_URL` (`POST /game`, `POST /move`)
  exactly like the starter.

`main.py` is the compressed entrypoint (built from `engine.py` with the
kit's own `makefun compress` command, so the transcript check counts the
readable source's lines). `engine.py` is the readable source; it is kept
in the folder for the bundle. The engine is Python 3.13 standard library
only and reads no other file.
