# tiny-chess-entry

A chess engine in under 4 KB of Python for the make.fun bounty
[tiny-chess-01](https://make.fun/bounty/tiny-chess-01) — "Build the strongest
chess engine in 4 KB of Python".

![competition](https://make.fun/badge/tiny-chess-01.svg)
![rank](https://make.fun/badge/tiny-chess-01/sinker234/tiny-chess-entry/rank.svg)
![reward](https://make.fun/badge/tiny-chess-01/sinker234/tiny-chess-entry/reward.svg)

`main.py` is fully self-contained (Python standard library only) and speaks
the make.fun environment protocol: it polls `MAKEFUN_ENV_URL/game` and posts
UCI moves to `/move`.

Engine design (unpacked from the LZMA payload):

- 0x88 board, signed pieces, pseudo-legal move generation with
  make/unmake legality filtering, incremental Zobrist hash and evaluation.
- Negamax alpha-beta with PVS, iterative deepening under a fixed,
  deterministic node budget (no wall clock, so trials replay exactly).
- Quiescence search over captures, promotions and check evasions.
- Transposition table with mate-score ply adjustment; null-move pruning;
  late-move reductions; futility pruning; check extensions.
- Move ordering: TT move, MVV-LVA captures, killers, history heuristic.
- Tapered evaluation: material, formula-generated piece-square tables,
  bishop pair, middlegame king shelter vs endgame king centralisation.
- Draw handling: repetition counts seeded from the game history,
  fifty-move rule, 200-ply material adjudication awareness via the
  node-budget taper late in the game.

Local verification: perft matches reference counts on six standard
positions; incremental eval/hash state matches scratch recomputation over
random playouts. In local practice trials against the house gauntlet, the
development version scored Elo 1545 and 1654 (baseline: ~1365).

Referral: https://make.fun/r/c5a4h2ax

## License

MIT — see `LICENSE`.
