# Brief — cstr_dynamics

*Operational brief. Lives with the project (way-of-working §6); it does not
migrate to the garden. Created 2026-09-23 at stand-up. Amend as decisions are
made; record each decision here and in the garden decisions log.*

## Goal

A live simulation of a CSTR with an exothermic reaction and a cooling jacket,
on the `tank_dynamics` architecture and stack, finished with one published
article. A stepping stone with a hard scope, chosen so that it ends.

## Definition of done

1. A working Model in the four-class architecture, with a test suite that
   includes energy-balance closure and steady-state checks.
2. A live site on the VPS: reactor schematic, instrument tags, trends, an
   Upsets tab that can trigger a runaway.
3. One published article on rogerwibrew.com, in Roger's voice, built from
   zettels written during the project.
4. Knowledge captured in the garden as the project proceeds.

Stretch, only after 1–4: cascade temperature control with the jacket loop as
secondary.

## Out of scope

Level control and variable volume; more than one reaction or unit; any
Tennessee Eastman work; frontend features beyond the new schematic; machine
learning on generated data; a new deployment stack.

## Guardrails

- Eight weeks from the start of Phase 1; week-four review; scope is cut, never
  extended.
- One new idea per phase.
- Parameters chosen in the first Phase 1 session and not reopened.
- The Model class is the unit of work; API or dashboard changes beyond the
  schematic need a logged decision first.
- No new tooling, framework, or deployment target.
- The article is a phase with a gate.
- Minimum complexity first.

## Staging and gates

| Phase | Work | Gate |
| --- | --- | --- |
| 0 | Stand-up: CLAUDE.md, brief, state, lessons, MCP wiring, uv, minimal agent config | A fresh session can describe the project from these files alone |
| 1 | Physics on paper: Roger derives; parameters chosen; behaviour list; test spec in prose | All four written and signed off; no code |
| 2 | Model in C++ with tests; steady-state initialisation; stepper decision | C++ tests pass; headless run reproduces the behaviour list; stepper decision logged |
| 3 | Bindings and API, reusing the tank's | Python and API suites pass; headless run drivable over WebSocket |
| 4 | Dashboard: schematic, tags, trends, upsets | Every behaviour on the list runs in the browser, including runaway |
| 5 | Deploy on the tank's pattern | Live, survives a fresh session from another device |
| 6 | Article and garden capture | Article live; output note; docs migrated; closing log entry |

## Decisions

Open until Roger decides. Record the answer, the date, and a one-line reason.

| # | Question | Status |
| --- | --- | --- |
| 1 | Reuse strategy: fork `tank_dynamics`, copy the reusable layers, or extract a shared library | open — needed before Phase 2 |
| 2 | Parameter source: one textbook example, or Roger's own numbers | open — first Phase 1 session |
| 3 | Stepper: keep fixed-step, or adaptive / implicit | open — Phase 2, on evidence |
| 4 | Control structure for v1: single loop on coolant flow; cascade as stretch | proposed, unconfirmed |
| 5 | Volume: constant, level control deferred | proposed, unconfirmed |
| 6 | Repository visibility | Roger created the repo 2026-09-23; visibility as he set it |
| 7 | Whether this project opens `wiki/process-engineering/`, and with which page | open — Phase 6 |

Decided:

- **2026-09-23 — Remote.** `git@github.com-rwconsult:rwconsult8254/cstr_dynamics.git`,
  alongside `tank_dynamics` and the website, rather than the rogerwibrew
  account first proposed. Reason: keeps the portfolio projects together.

## Related

- `CLAUDE.md` — how the agent works here.
- `docs/state.md` — where we are.
- `docs/lessons.md` — what we learned, from day one.
- `~/garden/Input/Reference Notes/Garden Decisions Log.md` — the durable record.
