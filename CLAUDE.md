# CLAUDE.md — cstr_dynamics

A live, browser-based simulation of a continuous stirred-tank reactor with an
exothermic reaction and a cooling jacket: the next rung after `tank_dynamics`
on the simulation ladder (mass balance → energy balance and kinetics →
Tennessee Eastman, later). The operational brief is [docs/brief.md](docs/brief.md);
read it first. It fixes the goal, the definition of done, the scope guardrails,
the staging, and the decisions still open.

A private `playbook.md` may exist at the repo root (gitignored). If present,
read it after the brief. The brief and this file are sufficient on their own;
the playbook adds rationale, not rules.

## Predecessors — read before building

- `~/dev/process-engineering/tank_dynamics/` — the architecture and stack this
  project reuses: four classes (Model, PID Controller, Stepper, Simulator) in
  C++ with GSL and Eigen, pybind11 bindings, FastAPI over WebSocket with a
  per-session registry, Next.js dashboard, Docker + Traefik on the VPS.
  **Read `docs/LESSONS_LEARNED.md` there before touching any layer.**
- `~/garden/Input/Reference Notes/Tank Dynamics Project Lessons.md` — the
  distilled lessons (lesson 0: query framework docs, never assume versions).
- `~/garden/Zettelkasten/Simulation Architecture.md` and the Model / Stepper /
  Simulator / PID Controller reference notes — the architecture being reused.

## Staging (from the brief)

One new idea per phase, so a failure is attributable to one layer:

0. Stand-up (this scaffold).
1. Physics on paper — Roger derives; the agent checks against the RAG. No code.
2. The model in C++ with tests.
3. Bindings and API — reuse the tank's, expose the new state and upsets.
4. Dashboard — new schematic and tags only.
5. Deploy — the tank's pattern, new subdomain.
6. Article and garden capture.

A phase does not start until the previous gate (in the brief) is passed.

## Claude's role

**Planner and implementer of small, requested tasks. Not the engineer.**

- Roger owns the physics, the parameters, the control structure, and every
  architectural call. He derives the model himself; the agent checks it.
- Plan first, in plan mode. An agreed plan becomes 10–15 small tasks on one
  theme, one branch per plan, one task at a time, each with the single command
  that verifies it. Roger reads and understands every result before the next.
- Never adjust a test to fit the code. Call out any test change explicitly.
- No unrequested code, no unrequested prose, no scope change without a logged
  decision in the brief.
- Query current framework documentation (Context7) before any task that touches
  an external framework. State versions.
- Be honest about provenance: RAG-grounded vs training data; say which.
- Every session: start by reading `docs/state.md` and the recent git log and
  saying where we are; end with `docs/state.md` updated, `docs/lessons.md`
  appended if anything was learned, and everything committed.

## Tools

- **process-engineering RAG** (`process-engineering-rag` MCP) — Perry's, GPSA,
  Campbell, plant docs. Scope with `source_type=reference_perrys` for textbook
  material. The corpus for grounding any claim that would later be cited.
- **garden RAG** (`garden-rag` MCP) — Roger's vault; search before authoring
  any note.
- **Context7** — current library documentation.
- If a needed MCP is missing from the session, **stop and say so** — never
  work blind (`~/garden/CLAUDE.md`).

## Where things go

- **This repo** — code, tests, and project-operational docs (`docs/brief.md`,
  `docs/state.md`, `docs/lessons.md`). Plans go in `docs/plans/NN-topic.md`;
  the matching branch is `plan/NN-topic`. Remote:
  `git@github.com-rwconsult:rwconsult8254/cstr_dynamics.git`. Commit as work
  proceeds; a task is not done until it is committed.
- **Zettels** are Roger's voice — never draft one. Reference notes that become
  stable migrate to `~/garden/Input/Reference Notes/` with a pointer left here
  (way-of-working §6). Wiki pages, if any, go to
  `~/garden/wiki/process-engineering/`, never this repo.
- `playbook.md` and `docs/private/` are gitignored and stay that way.

## Environment

- **uv, not pip.** `uv sync --extra dev`, `uv run pytest`.
- C++ toolchain, CMake, GSL, Eigen, pybind11, scikit-build-core — added in
  Phase 2, mirroring `tank_dynamics`. Nothing is installed ahead of need.
- This project does no GPU or local-LLM work of its own. The RAG MCP servers
  use the GPU and Ollama internally; that is theirs, not ours.
- `.mcp.json` and the ssh alias `github.com-rwconsult` are newton-specific.
  Away from newton the RAGs are unreachable: offline sessions are for writing,
  per `~/garden/way-of-working/way-of-working.md` §7.
