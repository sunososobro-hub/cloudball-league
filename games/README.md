# games

One JSON file per official game. Append-only: add files, never edit or delete them.

## File name

`games/<season>/<game_id>.json`, for example `games/2027/2027-0001.json`.
`game_id` is `<season>-<4-digit sequence>` in the order games were scheduled.

## Record schema (version 1)

| Field | Type | Meaning |
| --- | --- | --- |
| `schema` | integer | Schema version of this record. `1`. |
| `game_id` | string | Unique id, equal to the file name without `.json`. |
| `season` | integer | Season the game belongs to. |
| `scheduled_at` | string | Premiere time, ISO 8601 UTC. |
| `engine.repo` | string | Engine repository, e.g. `sunososobro-hub/cloudball`. |
| `engine.commit` | string | **Full 40-character commit SHA of the engine that simulated the game.** |
| `engine.version` | string | Engine version string reported by that commit. |
| `seed` | integer | **RNG seed (unsigned 32-bit) passed to the engine.** |
| `league_commit` | string | Full SHA of this repository's head when the game was simulated (the ledger state the player abilities were derived from). |
| `teams.away`, `teams.home` | string | Team ids from the factory settings. |
| `lineups.away`, `lineups.home` | object | `batting_order` (9 player ids), `starter` (pitcher id), `bullpen` (pitcher ids in order of use). |
| `decisions` | object | Every external decision fed to the engine (for example resolved fan votes), keyed by decision id. Empty object if none. |
| `result.score` | [integer, integer] | Final runs, away then home. |
| `result.innings` | integer | Innings played. |
| `result.line_score` | [[integer], [integer]] | Runs per inning, away then home. |
| `result.winning_pitcher`, `result.losing_pitcher` | string or null | Player ids. |
| `box.batting`, `box.pitching` | object | Per-player totals for this game, keyed by player id. |
| `event_hash` | string | SHA-256 (hex) of the engine's event stream for this game, as canonical JSON. |
| `supersedes` | string or null | `game_id` of a record this one replaces, otherwise `null`. |

## Replaying a record

Check out `engine.commit` of `engine.repo`, derive player abilities from the factory settings
plus the ledger at `league_commit`, and run the engine with `seed`, the lineups and
`decisions`. The result must reproduce `result`, `box` and `event_hash` exactly. If it does
not, the record is invalid.

The event stream itself is not stored: it is fully determined by the fields above.
