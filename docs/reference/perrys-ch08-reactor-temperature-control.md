# Perry's Chapter 8 — Temperature control of exothermic stirred-tank reactors

*RAG-grounded summary from the process-engineering RAG, document Perrys_Ch08_Process_Control, drafted 2026-09-23 for Roger's review. Own words; short quotations only.*

## Scope of this summary

What the Process Control section says about controlling an exothermic stirred-tank reactor with a cooling jacket: why the loop is hard, the recommended cascade structure (Shinskey, Fig. 8-55; Seborg, Fig. 8-36), how the two controllers are tuned, and the general rules for a lag-dominant temperature loop. Stability theory (ch. 19) and runaway (ch. 23) are in sibling summaries.

## What Perry's says

**Why the loop is difficult.** Reactor temperature should be controlled by heat transfer, never by a reactant feed: the feed path has two dominant lags (reactant concentration and thermal mass) and inverse response. An exothermic reaction is positive feedback; the cooling system is the opposing negative feedback. Most continuous reactors have enough heat-transfer surface relative to reaction mass to be self-regulating; most batch reactors do not and are steady-state unstable. [gpref-perrys-ch08-process-control-00188]

**Stable, unstable, uncontrollable.** An unstable reactor can still be controlled if the temperature controller gain can be set high enough and the cooling system has margin for the largest expected heat-load disturbance. Stirred tanks are lag-dominant, which permits high gain; plug-flow reactors are dead-time-dominant, so an unstable PFR is also uncontrollable and limit-cycles. A stable reactor becomes unstable as its surface fouls or production rate rises past a critical point (Shinskey, Chem. Eng., March 2002). [gpref-perrys-ch08-process-control-00189]

**The recommended structure (Fig. 8-55).** The reactor temperature controller sets the set point of a coolant outlet-temperature controller in cascade. A circulating pump on the coolant loop is "absolutely essential": it keeps dead time small and constant, whereas without it dead time varies inversely with cooling load and the loop limit-cycles at low load. Heating and cooling valves are split-ranged. The cascade linearises the primary loop, speeds it, and shields it from cooling-system disturbances. Heat removed per unit coolant flow is proportional to the coolant temperature rise, so the flow-to-heat relationship is nonlinear and an equal-percentage valve only partly compensates; heat flow across the surface is linear in both temperatures, so with jacket temperature as the secondary the primary loop has constant gain. [gpref-perrys-ch08-process-control-00190]

**Why the secondary is a jacket temperature.** Coolant exit temperature as the secondary variable puts the jacket dynamics inside the inner loop and shortens the primary period, which suits a stirred tank's large heat capacity; an externally cooled PFR lacks that capacity and needs the coolant-inlet loop instead. A refinement: feeding the secondary measurement into the primary's integral mode (positive-feedback integration) paces the primary integral time to the secondary's response and prevents windup. The reactor's primary time constant is τ1 = MrCr/UA, thermal mass over heat-transfer conductance (Eq. 8-83). [gpref-perrys-ch08-process-control-00191]

**Tuning the pair.** On a pilot reactor where τ1 was varied fourfold, neither controller needed retuning. The primary should be PID and the secondary at least PI; a proportional-only secondary leaves the primary with offset. Set-point overshoot can be avoided by setting the primary derivative time longer than its integral time, effective only with interacting PID. [gpref-perrys-ch08-process-control-00192]

**The general cascade argument (Seborg, Figs. 8-36, 8-37).** Feedback alone on a process with large lags sees a disturbance only after the controlled variable moves. For a jacketed reactor with coolant flow manipulated, a rise in plant coolant temperature reaches the reactor measurement slowly; measuring jacket temperature lets the inner loop cut coolant flow and hold the heat-transfer rate first. The primary outputs the secondary set point; the secondary compares it with the jacket measurement and moves the valve. Tune the inner loop first with the primary in manual; a proportional-only secondary is often enough because the primary's integral removes its offset. Feed temperature or composition disturb the primary; cooling-water temperature disturbs the secondary. [gpref-perrys-ch08-process-control-00093, -00094, -00095, -00098]

**Gain in a single loop.** Perry's opening example is a jacketed reactor whose controller raises coolant flow in proportion to temperature error. Raising Kc speeds the response and shrinks the peak deviation until the ultimate gain Ku, where temperature oscillates indefinitely; beyond it the valve cycles between limits. [gpref-perrys-ch08-process-control-00025, -00029]

**Tuning rules and implementation.** Table 8-2 gives minimum-IAE settings by process class (dead-time-dominant, lag-dominant, non-self-regulating, distributed lag); any secondary lag, filter, or sample interval adds to effective dead time. Derivative is recommended for temperature in multiple-capacity, low-noise processes. In a cascade the primary output may need scaling to the secondary set-point span. A digital controller should execute at least 3 times faster than the dominant lag, or about 10 times faster than the closed-loop time constant, or its stepped output can cause instability. [gpref-perrys-ch08-process-control-00074, -00065, -00378, -00379]

## What it means for cstr_dynamics

Agent interpretation, not Perry's.

- **Cascade structure.** Perry's supports the structure decided on 2026-09-23: reactor temperature primary, jacket temperature secondary, coolant flow manipulated. Shinskey and Seborg differ only in naming the secondary "coolant outlet" or "jacket" temperature; for a well-mixed jacket these coincide.
- **Jacket model.** The Model needs a jacket energy balance with its own holdup so that jacket temperature is a state and the jacket dynamics genuinely sit inside the inner loop. Whether to assume a circulating loop with makeup, or a once-through well-mixed jacket, is a derivation choice to state explicitly.
- **Nonlinearity.** Heat removal per unit coolant flow varies with coolant temperature rise, so the single-loop gain varies with load. The simulation should show this; it is part of why the cascade wins.
- **Behaviour list candidates.** Single loop vs cascade for a coolant-supply temperature step; feed temperature or concentration step; loss of coolant flow; controller gain up to and past Ku; P-only vs PI secondary (offset).
- **Test spec candidates.** Open-loop reactor time constant equals MrCr/UA; a jacket disturbance is rejected faster in cascade than single loop by a stated margin; no offset with a PI secondary; inner loop tuned first.
- **Stepper.** The 3× / 10× execution-rate rule bounds the simulation step by the jacket time constant, the fastest lag.
- **Anti-windup.** Perry's addresses windup only through positive-feedback integration. The secondary will saturate during runaway; a conventional anti-windup design is ours to make.

## Not in the RAG / open questions

- Figs. 8-55 and 8-36 are in the corpus as captions only; valve arrangement and measurement location are inferred from text.
- No retrieved passage compares coolant flow against coolant inlet temperature as the manipulated variable for a stirred tank, beyond the PFR remark in chunk 00191. Training-data territory if wanted.
- Anti-windup for a standard PID (clamping, back-calculation) was not retrieved; any design rests on training data.
- Table 8-2 numbers were retrieved (chunks 00074, 00075) but not reproduced here; consult the RAG when tuning.
- Nothing reactor-specific retrieved on measurement (thermowell) lag.

## Sources

- gpref-perrys-ch08-process-control-00025, -00029 — jacketed-reactor example; Kc up to Ku.
- gpref-perrys-ch08-process-control-00065 — derivative recommended for temperature.
- gpref-perrys-ch08-process-control-00074 — Table 8-2 tuning rules; dead-time additions.
- gpref-perrys-ch08-process-control-00093, -00094, -00095, -00098 — cascade motivation, reactor example, tuning order, disturbance entry points.
- gpref-perrys-ch08-process-control-00188 — feed not manipulated; positive vs negative feedback; self-regulation.
- gpref-perrys-ch08-process-control-00189 — stable/unstable/uncontrollable; Shinskey 2002.
- gpref-perrys-ch08-process-control-00190 — circulating pump, split range, cascade benefits, nonlinearity.
- gpref-perrys-ch08-process-control-00191 — jacket dynamics in secondary; integral feedback; τ1 = MrCr/UA.
- gpref-perrys-ch08-process-control-00192 — pilot test; PID primary, PI secondary.
- gpref-perrys-ch08-process-control-00378, -00379 — cascade output scaling; execution rate.
