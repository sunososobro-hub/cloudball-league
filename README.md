# cloudball-league

The official ledger of the cloudball league
([engine and viewer: cloudball](https://github.com/sunososobro-hub/cloudball)).

## What this repository is

An **append-only ledger**: one file per official game, added once, never edited.

- One official game = one new file under `games/`. Old files are never changed, renamed or
  deleted. A mistake is corrected by adding a new record that supersedes the old one, not by
  editing history.
- **Factory settings + this ledger = current player ability.** The factory settings (teams
  and players as they were created) live in `league/` of the cloudball repository. Everything
  that has happened to a player since then is derived by replaying the records here. Nothing
  else is stored anywhere: no hidden database, no cached "current stats" that could drift.
- Every record names the engine commit and the RNG seed, so any game can be replayed exactly
  and checked by anyone.

## Pre-season games are not written here

During pre-season (warm-up) the games are previews: results do not count, and they are **not**
recorded in this repository. `preseason/` stays empty on purpose. See `preseason/README.md`.

## Forks are parallel universes

Fork this repository and your ledger diverges from this one: a different history, different
careers, different champions. That is welcome, and it is what the license allows. A fork is
a parallel universe; it is not the original league and must not present itself as such.

## License

The data in this repository is dedicated to the public domain under CC0 1.0 (`LICENSE`).
The league name, the team names and the team logos are **not** covered by that dedication.
