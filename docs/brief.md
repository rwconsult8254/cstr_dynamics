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

Cascade temperature control — reactor temperature as the primary loop, jacket
temperature as the secondary — is part of item 1 and item 2, not a stretch
goal (decision 4, 2026-09-23). The Model must therefore carry a dynamic jacket
energy balance with jacket temperature as a measured state, and the Simulator
runs two PID instances.

## Out of scope

Level control and variable volume; more than one reaction or unit; any
Tennessee Eastman work; frontend features beyond the new schematic; machine
learning on generated data; a new deployment stack.

## Guardrails

- Eight weeks from the start of Phase 1 (2026-09-23); week-four review
  2026-10-21; end 2026-11-18. Scope is cut, never extended.
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
| 2 | Parameter source | decided 2026-09-23 — own numbers, derived from design constraints (residence time, adiabatic temperature rise, cooling margin, stability criterion) and checked against the RAG. Reason: no textbook to hand; deriving them is the learning |
| 3 | Stepper: keep fixed-step, or adaptive / implicit | open — Phase 2, on evidence |
| 4 | Control structure for v1 | decided 2026-09-23 — cascade (reactor temperature → jacket temperature → coolant flow) is core, not stretch. Reason: without it the project is only a slightly more complex tank; cascade is the learning content |
| 5 | Volume | decided 2026-09-23 — constant volume; level control deferred to the later list |
| 6 | Repository visibility | decided — public (Roger created it 2026-09-23; confirmed via the GitHub API). Nothing private is committed; the playbook stays ignored |
| 7 | Which page this project adds to `wiki/process-engineering/` | open — Phase 6. The wiki was opened on 2026-09-23 by the molsieve_adsorption project (garden commit 1b3a7fb), so the question is no longer whether but which page |

Decided:

- **2026-09-23 — Parameters are our own (decision 2); Phase 1 opened.**
  No textbook example to hand, so the parameter set is derived from design
  constraints and made sensible by argument, with the RAG as the check. The
  process-engineering RAG (Perry's ch. 19 reactor stability, ch. 8 reactor
  temperature control, ch. 23 runaway) holds the theory and criteria but no
  worked parameter set; it grounds the derivation, not the numbers.
  Phase 1 start 2026-09-23; week-four review 2026-10-21; eight-week end
  2026-11-18.
- **2026-09-23 — Cascade control is core (decision 4); constant volume
  (decision 5).** Roger: cascade is essential for this to be a learning
  experience; otherwise it is simply a slightly more complex tank dynamics.
  Volume stays constant. Consequences: the Phase 1 behaviour list and test
  spec cover both loops; the stretch line in the definition of done is gone;
  the playbook (private) still reads "cascade is stretch" and is superseded
  on that point by this brief.
- **2026-09-23 — Playbook accepted as written.** Roger reviewed the rendered
  playbook and had no comments; Phase 0 gate passed on the same day.
- **2026-09-23 — Remote.** `git@github.com-rwconsult:rwconsult8254/cstr_dynamics.git`,
  alongside `tank_dynamics` and the website, rather than the rogerwibrew
  account first proposed. Reason: keeps the portfolio projects together.

## Related

- `CLAUDE.md` — how the agent works here.
- `docs/state.md` — where we are.
- `docs/lessons.md` — what we learned, from day one.
- `~/garden/Input/Reference Notes/Garden Decisions Log.md` — the durable record.
