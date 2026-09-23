# cstr_dynamics

A live, browser-based simulation of a continuous stirred-tank reactor with an
exothermic reaction and a cooling jacket. The operator controls reactor
temperature, introduces the classic upsets, and can push the reactor into
runaway and watch it happen.

The second rung of a simulation ladder that started with
[tank_dynamics](https://github.com/rwconsult8254/tank_dynamics) (live at
tank.rogerwibrew.com) and heads toward a Tennessee Eastman plant. Same
architecture: C++ physics engine, Python bindings, FastAPI over WebSocket,
Next.js dashboard in ISA-101 style, Docker on a VPS.

Status: Phase 0, stand-up. See [docs/brief.md](docs/brief.md).

## Running

Nothing to run yet. `uv sync --extra dev` sets up the Python environment.
