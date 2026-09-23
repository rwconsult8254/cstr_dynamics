# Perry's Chapter 19 (Reactors): steady-state multiplicity and stability of a cooled CSTR

*RAG-grounded summary from the process-engineering RAG, document Perrys_Ch19_Reactors, drafted 2026-09-23 for Roger's review. Own words; short quotations only.*

## Scope of this summary

What Chapter 19 says about the dynamic balances of an ideal CSTR, the heat-generation and heat-removal picture that yields up to three steady states, the local and global stability criteria, and whether a reactor can run away from a lower steady state to a higher one. Chapter 8 (reactor temperature control, cascade) and Chapter 23 (runaway, temperature of no return) are covered in sibling summaries. Bracketed ids are the retrieval chunks cited.

## What Perry's says

**The ideal CSTR (Eqs. 19-11 to 19-13).** For a constant-volume, constant-density CSTR with volumetric feed V′, reactor volume Vr, feed concentration C0 and feed temperature T0, Perry's writes the mass balance as V′C0 = V′C + Vr·r(C,T) + Vr·dC/dt and the energy balance as V′ρcp·T0 = −Q(T) + V′ρcp·T − Vr(−ΔH)·r(C,T) + Vrρcp·dT/dt, with mean physical properties. Q(T) is heat added to the reactor; for transfer through the wall, Q(T) = Ak·U·(Tc − T), with Ak the area, U the overall coefficient and Tc the heat-transfer-fluid temperature. The pair of ODEs is integrated from an initial condition. At steady state with an isothermal first-order reaction the effluent reduces to C/C0 = 1/(1 + kτ) (Eq. 19-13) [gpref-perrys-ch19-reactors-00036].

**Reactor dynamics.** Continuous reactors are meant to hold a steady state under control, but the nonlinearity of kinetics and transport means large departures are possible. For one set of conditions more than one steady state can exist; which is reached depends on the initial condition, and only stable ones are reachable without special control. Oscillation and chaos have been observed. Start-up, shutdown and abrupt changes can drive a reactor past its design limits into runaway, blowout or explosion; the study of response to abrupt changes is called parametric sensitivity [gpref-perrys-ch19-reactors-00053, gpref-perrys-ch19-reactors-00054].

**Multiplicity for a first-order reaction (Eqs. 19-23, 19-24).** The simplest case is kinetics interacting with heat transport in an adiabatic CSTR. With k = exp(a + b/T), the steady-state rate is r = kC = k·Cf/(1 + kτ) (Eq. 19-23). The steady-state energy balance is then a heat-generation term against a heat-removal term: QG(T) = −ΔHr·Vr·r(C,T) and QH(T) = V′ρCp·(T − Tf) (Eq. 19-24). QG is sigmoidal in T through the Arrhenius factor; QH is a straight line; plotted together (Fig. 19-4) they can cross at up to three points. Multiplicity needs a feedback mechanism, which is why a plug-flow reactor without backmixing does not show it [gpref-perrys-ch19-reactors-00057]. Fig. 19-4(a) labels the three states A, B and C, with A and C stable and B unstable; Fig. 19-4(b) shows one, two or three states depending on the feed pair (Cf, Tf) [gpref-perrys-ch19-reactors-00063]. Morbidelli et al., Luss, Schmitz, and Razon and Schmitz are cited for the bifurcation literature, with the caution that many published criteria are not experimentally validated [gpref-perrys-ch19-reactors-00058, gpref-perrys-ch19-reactors-00055].

**Local stability: the slope argument.** Just above A, removal exceeds generation and T falls back; just below A, generation exceeds removal and T rises back. Around B the opposite holds: a push upward sends the reactor to C, a push downward to A. So A and C are stable and B is not, and a unique steady state is always stable for the adiabatic CSTR. Hence for the adiabatic CSTR the slope condition dQH/dT > dQG/dT is necessary and sufficient for asymptotic stability. For an externally cooled CSTR, however, the slope condition is "a necessary but not a sufficient condition": violating it guarantees instability, satisfying it does not guarantee stability, and even a unique steady state can become unstable and oscillate [gpref-perrys-ch19-reactors-00055, gpref-perrys-ch19-reactors-00056, gpref-perrys-ch19-reactors-00059].

**Local stability: the linearised test (Eq. 19-25).** Solve for the steady states, then linearise the transient balances in deviation variables x = C − Css and y = T − Tss to get d/dt [x, y] = A·[x, y]. The determinant condition det(A) > 0 and the trace condition trace(A) < 0 are together necessary and sufficient for asymptotic stability, and the method extends to more states and complex kinetics (Denn; Morbidelli et al.) [gpref-perrys-ch19-reactors-00059].

**Global stability and runaway (Eq. 19-26).** Whether a reactor can leave a lower stable state and run to a higher one cannot be settled by linearised analysis. For the simple CSTR Perry's gives a critical temperature difference ΔTc for "globally stable operation": keep T − Tj below ΔTc, which is defined in terms of the activation energy E and the gas constant R (the expression itself did not survive text extraction; see open questions). For a jacketed PFR the conservative analogue uses the hot-spot temperature. Beyond this, transient equations are solved numerically; Varma et al. is the reference for parametric sensitivity [gpref-perrys-ch19-reactors-00060, gpref-perrys-ch19-reactors-00061].

**Heat removal hardware.** For modest duty a jacketed stirred tank is adequate (Fig. 19-1a); internal coils, then external circulation through an exchanger, follow as duty or the need for tight peak-temperature control rises [gpref-perrys-ch19-reactors-00048, gpref-perrys-ch19-reactors-00050]. The design overview notes runaway "where reactor temperature continues to increase until the reactants are depleted" and that a control strategy is therefore required [gpref-perrys-ch19-reactors-00014].

## What it means for cstr_dynamics

*Agent interpretation, not Perry's text.*

- **Model form.** Eqs. 19-11 and 19-12 are the two reactor ODEs the Phase 1 derivation should reproduce, with Q(T) = UA·(Tj − T) and Tj promoted to a third state with its own balance. Fix the sign convention (Q positive when the jacket is hotter) early.
- **Parameter derivation.** The generation curve is set by (−ΔH)·Cf, a, b and τ; the removal line by V′ρCp and, for the cooled case, UA. A design point on the upper branch with a lower branch present means choosing these so QG and QH cross three times. That is a sketch before any code.
- **Stability check by hand.** Ours is the cooled case, so the slope test alone is not enough (00056). The linearised determinant and trace test of Eq. 19-25, with the jacket as a third state, is the cheap paper check that the design point is locally stable.
- **Behaviour list.** Three steady states at one feed condition; cold start lands on the lower state; a sufficient push crosses B and ignites to C; loss of cooling carries the reactor past C toward the adiabatic limit. Fig. 19-4(b) suggests a (Cf, Tf) map of one-, two- and three-state regions.
- **Test spec.** Steady-state residuals of 19-11 and 19-12 at each computed state; a perturbation from B that diverges and from A or C that returns; eigenvalues of the linearised matrix with the signs required by 19-25; the T − Tj margin reported against ΔTc once its form is confirmed.

## Not in the RAG / open questions

- **Form of ΔTc (Eq. 19-26).** The chunk names T, Tj, E and R but the fraction is lost. From training data, not the RAG: the usual result is ΔTc = R·T²/E. Confirm against a printed copy before it enters the test spec.
- **Cooled-CSTR removal line.** Eq. 19-24 is for the adiabatic CSTR. The retrieved chunks do not write the cooled version with a UA·(T − Tj) term; that extension is Roger's to derive, and the "necessary but not sufficient" remark is the only Perry's guidance retrieved on it.
- **Adiabatic temperature rise.** No formula in the Chapter 19 chunks; the chapter only remarks that a high adiabatic rise raises safety issues [gpref-perrys-ch19-reactors-00101]. The design guidance to calculate it sits in Chapter 23.
- **Jacket dynamics.** Nothing retrieved from Chapter 19 on a jacket balance or holdup; Chapter 8 (Eq. 8-83, the reactor primary time constant) is the nearest material.
- **Eqs. 19-13 and 19-23** are reconstructed from garbled fragments; the reading above is the standard one and consistent with the fragments.

## Sources

- gpref-perrys-ch19-reactors-00014 — reactor dynamics overview; runaway; need for control.
- gpref-perrys-ch19-reactors-00036 — ideal CSTR balances, Eqs. 19-11 to 19-13.
- gpref-perrys-ch19-reactors-00048, -00050 — heat-removal hardware for stirred tanks.
- gpref-perrys-ch19-reactors-00053, -00054 — reactor dynamics; parametric sensitivity; Eq. 19-23.
- gpref-perrys-ch19-reactors-00055 — perturbation argument around A, B, C.
- gpref-perrys-ch19-reactors-00056 — slope condition; adiabatic vs cooled distinction.
- gpref-perrys-ch19-reactors-00057 — Eq. 19-24; three steady states; feedback requirement.
- gpref-perrys-ch19-reactors-00058 — bifurcation literature pointers.
- gpref-perrys-ch19-reactors-00059 — linearised stability, Eq. 19-25; ΔTc introduced.
- gpref-perrys-ch19-reactors-00060 — Eq. 19-26; jacketed PFR analogue; wrong-way effect.
- gpref-perrys-ch19-reactors-00061 — Varma et al.; model-detail trade-off.
- gpref-perrys-ch19-reactors-00063 — Fig. 19-4 caption.
- gpref-perrys-ch19-reactors-00101 — adiabatic temperature rise as a safety issue.
