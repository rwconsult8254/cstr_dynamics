# Lessons learned

*Started 2026-09-23, day one — the tank project started its file too late.
One entry per lesson, dated, a few lines each. Stable, cross-project lessons
migrate to `~/garden/Input/Reference Notes/` with a pointer left here.*

(none yet)

## 2026-09-23 — Equations do not survive the RAG's PDF extraction; check provenance of every formula

Perry's Eq. 19-26 (the critical temperature difference for global CSTR
stability) is in the corpus as "T − Tj < ΔTc" with the right-hand side lost
to text extraction. The agent quoted the usual form RT²/E in conversation as
if it were retrieved; a subagent drafting the ch. 19 summary caught it. Rule:
when a chunk shows a garbled fraction or a bare equation number, the formula
is not RAG-grounded, whatever the surrounding prose says. State the form as
training data, and confirm against a printed copy before it enters a test
spec. Sources: `docs/reference/perrys-ch19-reactor-stability.md`, open
questions.
