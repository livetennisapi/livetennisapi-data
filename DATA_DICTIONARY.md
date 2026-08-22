# Data dictionary

## matches.csv

| Column | Type | Meaning |
|---|---|---|
| match_id | int | stable match identifier; joins to points and (in the full dataset) statistics |
| player1_id / player2_id | int | join to `players.csv` |
| tournament | text | tournament name as tracked (e.g. "Miami") |
| tournament_key | text | stable tournament identifier |
| tier_key | text | event-tier identifier (ATP / WTA / Challenger / ITF event types) |
| surface | text | `hard`, `clay`, `grass` (empty = unrecorded) |
| indoor | bool | empty = unrecorded |
| best_of | text | `BO3` / `BO5` |
| round | text | round label as tracked, vendor-style (e.g. "ATP Miami - 1/16-finals") |
| scheduled_time_utc | timestamp | scheduled start, UTC |
| event_status | text | terminal status detail where recorded (retirements, walkovers); empty = normal completion |
| is_qualifying | bool | qualifying-draw match |

Only matches with `status = completed` and scheduled before the embargo cutoff are included.

## players.csv

| Column | Type | Meaning |
|---|---|---|
| player_id | int | stable player identifier |
| name | text | display name |
| tour | text | primary tour association where known |
| country | text | IOC-style 3-letter code |
| birthday | text | date of birth where known |
| hand | text | `R`/`L` where known |
| sackmann_id | text | id in Jeff Sackmann's open datasets (crosswalk); empty = unmapped |

## points_sample_2026-06.csv

One row = one observed score state, in capture order per match.

| Column | Type | Meaning |
|---|---|---|
| match_id | int | joins to `matches.csv` |
| sets_p1 / sets_p2 | int | completed sets won at this state |
| games_p1 / games_p2 | json array | games won per set, e.g. `[6,4,3]`; last element = current set |
| points_p1 / points_p2 | text | current-game score: `0`,`15`,`30`,`40`,`A`/`AD`; numeric counts inside tiebreaks |
| server | int | `1` or `2` (player serving); empty = unrecorded at that state |
| is_tiebreak | bool | state occurs inside a tiebreak |
| timestamp_utc | timestamp | live capture time, UTC (microsecond precision) |

Notes: the first row of a match's tape is not guaranteed to be 0-0 (collection may attach
mid-match) and the last row may precede the final point (vendor scoreboard truncation). Rows
are states, not strokes: a point's outcome is read from the transition between consecutive
rows.
