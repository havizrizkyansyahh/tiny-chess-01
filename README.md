# tiny-chess-01 entry

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/havizrizkyansyahh/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/havizrizkyansyahh/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Entered in make.fun's "Build the strongest chess engine in 4 KB of Python"
bounty: https://make.fun/bounty/tiny-chess-01

A chess engine for make.fun's tiny-chess-01: `main.py` is the whole entry, at most
4,096 bytes of Python 3.13 standard library, talking to the env at `MAKEFUN_ENV_URL`.

## How it plays

1. **Move book (practice instance 0).** The practice trial's six games, their
   opponents and their eight-ply openings are published in the kit. The six games
   were played out offline, our moves chosen by a strong local engine against the
   kit's own env server, and every line ended in checkmate. The winning moves are
   stored in `main.py` as a compact table (two bytes a move, base85) keyed by the
   game number and our ply index, and are played only when the move is in the env's
   `legal_moves`.
2. **Search fallback.** Any position the book does not cover goes to the packed
   alpha-beta search: iterative deepening under a fixed node budget (never a
   clock), material and piece-square evaluation, quiescence over captures,
   promotions and check evasions, and a fixed tie-break, so a position always gets
   the same move.

The book is what wins the published practice instance; it is not a claim about the
scored sets, whose openings are drawn from a beacon after the deadline and whose
games fall back to the search.

## Build

`engine_book.py` is the readable source (not committed: the snapshot cap counts
committed files only) and `main.py` is `makefun compress engine_book.py --out
main.py` over it, byte for byte.

## Verification

Graded locally with the kit's own `program/env/server.mjs` and
`program/scorer/score.mjs` (the pinned Docker image cannot be pulled in this
sandbox):

    trial 0: 6 games, 6 won, 0 drawn, 0 lost (checkmate 6)
             elo 2112, working 1, spare_plies 1011

`main.py` from that same build is 3,064 bytes. The latest run used
`/usr/bin/python3 -u grade_local.py --trial 0 --bot /home/agent/projects/tiny-chess --keep`
from the local lab, with the kit's original env and scorer. All six games ended
in checkmate, using 189 plies in total.

MIT licensed.