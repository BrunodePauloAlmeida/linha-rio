# Linha Rio

**A city-scale public transit simulator for a Rio-like network: plan journeys, simulate a full day of passengers, and let an optimizer rewrite the timetable.**

In Rio, getting across town can mean a bus, a BRT, the metro and a long wait in between. Every few months the city announces a new bus network plan and nobody can tell in advance whether it will make trips faster or slower. Linha Rio is a sandbox for that question: build a network, push a whole city's worth of trips through it, measure what people experience, and search for a better timetable with the same fleet.

It is several hard systems that must fit together:

- a GTFS reader, writer and validator;
- a synthetic Rio-like network generator (neighbourhoods, bus lines, BRT corridors, metro, a ferry);
- a RAPTOR journey planner that finds every fastest-versus-fewest-transfers option;
- a travel demand model with hundreds of thousands of trips across the day;
- an event-driven simulation with vehicle capacity, crowding, left-behind passengers and missed transfers;
- a timetable optimizer that changes headways under a fixed fleet;
- a web map that replays the day.

## Status

Starting point only. The package, the license and the plan are here; the simulator is built session by session. See [ROADMAP.md](ROADMAP.md).

## How it will work

1. **Network.** A seeded generator writes a valid GTFS feed for an invented city shaped like Rio. The same code can later load a real feed.
2. **Routing.** RAPTOR over the timetable, with walking transfers between nearby stops, returning the Pareto set of journeys.
3. **Demand.** An origin–destination matrix by hour, sampled into individual trips with departure times.
4. **Simulation.** Every vehicle and passenger moves through one day. Full buses leave people behind; late buses break transfers; passengers re-plan.
5. **Metrics.** Waiting time, in-vehicle time, crowding, left-behind counts and transfer success, per line and per neighbourhood.
6. **Optimization.** A local search over headways per line and time band, under a fixed fleet size, scored by the simulation.
7. **Map.** A browser view that plays the day back with vehicles, crowding colours and before-and-after comparisons.

## Built with

- Python 3.12 and NumPy for the generator, router, simulation and optimizer
- FastAPI for the map's back end
- A browser map drawn on canvas in TypeScript (planned; no online map tiles)
- pytest

The facts about GTFS, RAPTOR and Rio's open data are in [docs/transit.md](docs/transit.md).

## Setup

```bash
python3.12 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
pytest
```

## License

MIT, see [LICENSE](LICENSE). All network and demand data is synthetic. Not affiliated with the City of Rio de Janeiro or any transit operator.
