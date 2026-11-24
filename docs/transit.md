# Transit data and routing facts

Checked on 24 November 2026, before the first session, because the workspace desktop has no internet. Sources are listed under each section.

## GTFS Schedule

- A GTFS Schedule feed is a ZIP of mostly `.txt` CSV files. In its most basic form it has 7 files: `agency.txt`, `routes.txt`, `trips.txt`, `stops.txt`, `stop_times.txt`, `calendar.txt` and `calendar_dates.txt`.
- `calendar.txt` is required unless every service date is defined in `calendar_dates.txt`.
- Optional files we care about: `shapes.txt`, `frequencies.txt`, `transfers.txt`, `fare_attributes.txt`, `fare_rules.txt`, `feed_info.txt`.
- `stops.txt` core fields: `stop_id`, `stop_name`, `stop_lat`, `stop_lon`, plus optional `location_type` and `parent_station` for stations with several platforms.
- `stop_times.txt` links trips to stops (`trip_id`, `arrival_time`, `departure_time`, `stop_id`, `stop_sequence`) and is usually the largest file in a feed.
- Service that runs on a fixed interval can be described with `frequencies.txt` instead of listing every trip.
- Times may go past 24:00:00 for trips that run after midnight on the same service day. Google's documentation allows hours 00 to 99.

Sources: gtfs.org (Realtime overview, Base features, Schedule Best Practices, revision history); Google Transit GTFS reference; US DOT ITS standards training; old.gtfs.org reference.

## RAPTOR

- RAPTOR (Delling, Pajor, Werneck; ALENEX 2012, Transportation Science 2015) computes all Pareto-optimal journeys between two stops for two criteria: arrival time and number of transfers.
- It is not Dijkstra-based and needs no preprocessing, so it works when the timetable changes (delays, a new optimized timetable). It scans each route at most once per round.
- Round k finds the best arrival times using k−1 transfers. Each round has a route phase (scan routes through stops improved in the last round) and a transfer phase (walking to nearby stops).
- Extensions: rRAPTOR (range queries over a departure window) and McRAPTOR (more criteria, e.g. fare).

Sources: Microsoft Research publication page and KIT PDF of the paper; AAAI SoCS extended abstract; arXiv 2111.06654; arXiv 2204.01326.

## Rio de Janeiro open data

- The city publishes an official GTFS feed for its buses on Data.Rio. OpenStreetMap contributors report that Data.Rio data is CC-BY by default and that the city gave OSM written permission to use it.
- The municipal bus system (SPPO) is being renamed Sistema RIO (Rede Integrada de Ônibus) under the 2025 judicial agreement, with phased new contracts through 2028.
- Plan: the project runs entirely on synthetic data. The workspace has no internet, and bringing the real feed into the repo needs its license confirmed first. Loading a real feed is a later, optional milestone.

Sources: OpenStreetMap Community forum, "GTFS dos ônibus do Rio de Janeiro"; Prefeitura do Rio, SMTR presentation "Audiência Online Edital RIO Etapa 2" (2026).
