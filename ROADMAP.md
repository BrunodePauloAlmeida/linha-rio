# Roadmap

## Where this is going

Load or generate a city's transit network, push a full day of realistic demand through it, measure what passengers experience, and let an optimizer find a better timetable with the same number of vehicles. The results should be reproducible from a seed and shown on a map. This will take many sessions; one session should move one or two phases forward.

## Constraints

- The workspace desktop has no internet: no map tiles, no downloading feeds. Everything starts from a seeded synthetic network.
- 4 CPUs and 16 GB of RAM: the full-day simulation must fit in memory and finish in minutes, not hours.
- Back end in Python and NumPy. The map runs in the desktop's Chrome, drawn on canvas.

## Phases

### 1. GTFS
- [ ] Reader and writer for the core files plus shapes, frequencies and transfers
- [ ] Validator: missing references, out-of-order stop times, impossible speeds, times past 24:00
- [ ] Compact in-memory timetable (NumPy arrays per route pattern)

### 2. Synthetic Rio-like city
- [ ] Seeded generator: neighbourhoods with population and jobs, a coastline, hills that block streets
- [ ] Bus lines, BRT corridors, two metro lines and a ferry, with realistic headways by time band
- [ ] Walking transfer graph between nearby stops
- [ ] The generator writes a GTFS feed that passes our validator

### 3. Journey planning
- [ ] RAPTOR with walking transfers (arrival time × number of transfers)
- [ ] rRAPTOR for a departure window
- [ ] Brute-force reference router for tests on small networks

### 4. Demand
- [ ] Origin–destination matrix by hour from population and jobs (gravity model)
- [ ] Sample 200,000+ individual trips across one day, seeded

### 5. Simulation
- [ ] Event-driven day: vehicles run trips, passengers wait, board, ride, transfer, walk
- [ ] Vehicle capacity, crowding, left-behind passengers and re-planning
- [ ] Random delays that propagate along a line and break transfers
- [ ] Metrics per line, per neighbourhood and per hour

### 6. Optimization
- [ ] Decision variables: headway per line and time band; constraint: fixed fleet size
- [ ] Local search (or simulated annealing) scored by the simulation, with a cheaper proxy score for screening
- [ ] Before-and-after report

### 7. Map
- [ ] Canvas map of the synthetic city, lines and stops
- [ ] Replay of the day at adjustable speed, with crowding colours
- [ ] Side-by-side baseline vs optimized timetable

## Done means

- The generated feed passes the validator with zero errors.
- RAPTOR returns the same Pareto sets as the brute-force router on 1,000 random queries.
- A full day with 200,000 trips simulates in under 10 minutes on the workspace desktop, and two runs with the same seed give identical metrics.
- The optimizer cuts average waiting time by at least 10% versus the baseline timetable, with the same fleet, on a fixed seeded scenario.
- The map replays the whole day in the browser without stalling.

## Open decisions

- Passenger behaviour: always take the planned journey, or re-plan at every stop when something changes?
- Optimization objective: waiting time only, or a weighted sum with crowding and transfers?
- How Rio-like the city should be: invented geography, or rough real outlines (bays, lagoon, mountains)?
- Fares: ignore them, or model the integrated fare and McRAPTOR?

## Later

- Load the real Data.Rio feed, after confirming its license
- GTFS Realtime-style delay feed for the simulation
- Network design (adding or removing lines), not just timetables
