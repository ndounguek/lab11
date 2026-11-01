# Error table

| Scenario | Function that detects it | Error value | What the player sees |
|---|---|---|---|
| Fleet file does not exist | `load_fleet` | `Err(DataError::Io(..))` (kind `NotFound`) | `Error: could not load data/fleet.txt: No such file or directory (os error 2)`, exit code 1 (fatal) |
| A folder is given as the fleet path | `load_fleet` | `Err(DataError::Io(..))` | `Error: could not load data: Is a directory (os error 21)`, exit code 1 (fatal) |
| Fleet file has only comments | `load_fleet` | `Err(DataError::NoShips("data/fleet_empty.txt"))` | `Error: could not load data/fleet_empty.txt: no valid ships in data/fleet_empty.txt`, exit code 1 |
| Every line of the fleet file is bad | `load_fleet` | `Err(DataError::NoShips("data/fleet_all_bad.txt"))` | `Error: could not load data/fleet_all_bad.txt: no valid ships in data/fleet_all_bad.txt`, exit code 1 |
| One fleet line has length 1 (`Tugboat,1`) | `parse_ship_line` | `Err(ShipLineError::LengthOutOfRange(1))` in `skipped` | `data/fleet.txt line 10: has length 1, outside 2-5`, then the menu |
| Board file with nine rows | `parse_board` (via `load_board`) | `Err(DataError::Board(BoardError::WrongRowCount(9)))` | `Practice board unavailable: bad board layout: found 9 rows, expected 10`, back to the menu |
| Board file with a bad character | `parse_board` (via `load_board`) | `Err(DataError::Board(BoardError::BadChar { line: 6, ch: 'x' }))` | `Practice board unavailable: bad board layout: line 6 has 'x', only '.' and 'S' are allowed` |
| Scores file is missing (first run) | `load_scores` | not an error: `Ok(vec![])` | `no scores yet`; the game runs normally |
| Scores file cannot be written | `append_score` | `Err(DataError::Io(..))` | `Could not save the score: Is a directory (os error 21)`, back to the menu |
| `K11` typed at the prompt | `parse_coord` | `Err(CoordError::ColumnOutOfRange('K'))` | `column K is off the board (A-J)`; asked again, no shot counted |


