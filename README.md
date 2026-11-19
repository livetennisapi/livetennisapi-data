# Tennis Point-by-Point Dataset — ATP, WTA, Challenger, ITF

**Free for non-commercial academic research and teaching.**
Public sample: [DOI 10.5281/zenodo.22048730](https://doi.org/10.5281/zenodo.22048730) ·
Dataset page: [livetennisapi.com/data/academic](https://livetennisapi.com/data/academic) ·
Full-dataset applications: **research@livetennisapi.com**

This repository holds the **documentation and loaders** for the dataset. The data itself
lives on Zenodo (public sample) and is granted in full on application — nothing here to
clone but knowledge.

## What the dataset is (measured 2026-08-22)

- **Match index** — 173,571 completed professional matches, 2023-01-01 → 2026-08-13
  (7-day rolling embargo), across ATP, WTA, Challenger and ITF, singles and doubles.
- **Point-by-point score sequences** for 170,527 of them (98.2%), in two stated
  provenance classes:
  - `observed` — captured live by Live Tennis API's own collection with real UTC
    timestamps: **22,031 matches / 3,184,514 point states**, from 2025-10-12 (full
    volume from February 2026), every tier down to ITF — where public point-level data
    has effectively never existed.
  - `reconstructed`: rebuilt after the match, with no capture timestamps. It covers 2023
    to 2025 and part of 2026. It is not in the public files and is shared per request
    under the programme.
- 32,678 players with stable IDs, tour, country, birth date and handedness.

Known limitations are stated plainly in [the data dictionary](DATA_DICTIONARY.md) and on
the dataset page: the observed layer starts late 2025; tapes may start mid-match or end
before the final point (each match declares its own coverage); no serve speed/direction
or shot-level data; nothing before 2023.

## Loading the sample

Python:

```python
import pandas as pd

matches = pd.read_csv("matches.csv.gz")
players = pd.read_csv("players.csv.gz")
points  = pd.read_csv("points_sample_2026-06.csv.gz",
                      parse_dates=["timestamp_utc"])

# games_p1/games_p2 are JSON arrays (games won per set; last element = current set)
import json
points["games_p1"] = points["games_p1"].map(json.loads)
points["games_p2"] = points["games_p2"].map(json.loads)

# One row = one score STATE; a point's outcome is the transition between
# consecutive rows of the same match_id (rows are in capture order).
```

R:

```r
library(readr)
matches <- read_csv("matches.csv.gz")
points  <- read_csv("points_sample_2026-06.csv.gz")
```

## Access & conditions

Email **research@livetennisapi.com** from an institutional address (students: ORCID or a
named supervisor) with one paragraph on your research and the data you need. Target
turnaround: 3 business days. Conditions: non-commercial research and teaching only · no
redistribution of raw data (published aggregates and findings are fine) · citation
required · tell us when you publish.

## Citing

> Live Tennis API (2026). *Tennis Point-by-Point Dataset — ATP, WTA, Challenger, ITF*.
> Zenodo. https://doi.org/10.5281/zenodo.22048730

This DOI resolves to the latest version of the sample. See also the
[corrections to this sample](https://livetennisapi.com/data/corrections).

```bibtex
@dataset{livetennisapi_2026_tennis_pbp,
  author    = {{Live Tennis API}},
  title     = {Tennis Point-by-Point Dataset --- ATP, WTA, Challenger, ITF},
  year      = 2026,
  publisher = {Zenodo},
  doi       = {10.5281/zenodo.22048730},
  url       = {https://doi.org/10.5281/zenodo.22048730}
}
```

## License

Documentation and code snippets in this repository: MIT. The dataset itself:
CC BY-NC 4.0 via [Zenodo](https://doi.org/10.5281/zenodo.22048730) and the
application process — this repository grants no data rights.
