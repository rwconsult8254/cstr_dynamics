# Route to a sensible parameter set

*Agent proposal for Roger's review, 2026-09-23. This is a method, not a
parameter set. Decision 2 in the brief: the numbers are our own, derived from
design constraints and checked against the process-engineering RAG. Where a
step leans on the RAG it says so; the rest is engineering judgement offered
for Roger to accept, change, or reject.*

## Why a route rather than a table

There is no textbook worked example to hand, and a parameter set copied from
memory would be unverifiable. Deriving the numbers from constraints gives
each one an argument, which is what the Phase 1 gate asks for ("parameters
chosen ... with the argument for each number", `docs/state.md`). It also
makes the derivation itself part of the learning, which is the point of the
project.

## The route

1. **Fix the basis.** Liquid phase, a single irreversible first-order
   reaction A → B, constant density and heat capacity, water-like physical
   properties, constant volume (decision 5). Perfect mixing, so the reactor
   contents are at one temperature and one concentration. This is the
   smallest model that shows steady-state multiplicity and runaway (Perry's
   ch. 19 treats exactly this case).

2. **Choose reactor volume and residence time.** These set the mass balance
   on their own and fix the time scale of the simulation. A residence time
   of a few minutes to a few tens of minutes keeps the live trends readable
   in a browser session. Pick round numbers; nothing downstream needs them
   to be exact.

3. **Choose feed concentration and feed temperature.** The feed temperature
   sits below the operating temperature so the reactor has to be warm to
   run, and the design point lives on the upper branch.

4. **Choose the heat of reaction through the adiabatic temperature rise.**
   The adiabatic rise ΔT_ad = (−ΔH) C_A0 / (ρ c_p) is the single most
   important lever. Too small and there is no upper branch worth having; too
   large and every upset is a disaster. Choose ΔT_ad first, as a temperature,
   then back out ΔH. Perry's ch. 23 uses the maximum adiabatic rise as the
   first hazard-assessment quantity, which is the same reasoning in the
   safety direction.

5. **Choose the activation energy and pre-exponential factor for the shape
   of the heat-generation curve.** E sets the steepness of the S-shaped
   generation curve; k₀ sets where it sits on the temperature axis. Choose
   them so that at the design point conversion is high and the operating
   temperature is on the upper stable branch, and so that a lower
   (extinguished) branch exists. Perry's ch. 19 gives the first-order
   steady-state relation and a critical temperature difference ΔT_c, a
   function of E and R, as the bound on how far the reactor temperature may
   sit above the jacket temperature for globally stable operation. The
   expression itself is lost in the RAG's text extraction; the usual form
   ΔT_c = R T² / E is from training data and must be confirmed against a
   printed copy before it enters the test spec. Use it as a check on the
   pair (E, design point), not as a design target.

6. **Size the cooling: UA, jacket volume, coolant inlet temperature, and
   nominal coolant flow.** Two constraints. First, at the design point the
   heat-removal line must cross the generation curve with a margin, so the
   controller has room to act in both directions. Second, loss of coolant
   flow must carry the reactor past the temperature of no return within a
   few residence times, so the Upsets tab produces a runaway on a human time
   scale. Perry's ch. 23 names loss of coolant flow and a rise in coolant
   temperature as the canonical upsets; both should be reachable with the
   chosen numbers.

7. **Give the jacket its own dynamics.** A jacket holdup and residence time
   shorter than the reactor's, so the secondary (jacket temperature) loop is
   faster than the primary (reactor temperature) loop. This is what makes
   the cascade worth having (decision 4). Perry's ch. 8 recommends a
   circulating coolant loop to keep dead time small and constant; the model
   can represent that as a well-mixed jacket with coolant flow as the
   manipulated variable.

8. **Check the set.** With the candidate numbers, plot heat generation and
   heat removal against reactor temperature and confirm three intersections,
   with the design point on the upper branch. Confirm the ΔT_c check.
   Confirm a loss-of-cooling trajectory crosses the temperature of no
   return. Confirm the open-loop time constants are in a range a PID loop
   can handle at the simulation step. This check is arithmetic in a scratch
   notebook, not project code; it belongs to Phase 1.

## What the RAG contributes and what it does not

RAG-grounded (Perry's, via the process-engineering RAG; see the three
chapter summaries in `docs/reference/`): the first-order CSTR heat balance
and multiplicity treatment, the existence of the ΔT_c stability criterion
(its algebraic form is not legible in the corpus), the runaway
mechanism and the list of triggering upsets, the reactor-control guidance on
lag-dominant stirred tanks and the circulating coolant loop.

Not in the RAG: any worked parameter set. Steps 2, 3, and the target values
in 4 to 7 are engineering judgement. They should be argued in the parameter
document, one paragraph per number.

## Order of work

Steps 1 to 3 in one sitting; step 4 and 5 together, since they interact;
step 6 and 7 together; step 8 last. Each step's output is a short entry in
the Phase 1 parameter document with the number and its argument.
