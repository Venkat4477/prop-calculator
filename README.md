# Prop Performance Calculator

Static propeller shaft power, torque, and thrust vs RPM, with
manufacturer max-RPM limits. Single-file, no build step — open
`index.html` in any browser (needs internet once for the Chart.js CDN).

## Model

- Shaft power: Boucher empirical fit
  `P = Kp · (B/2)^0.8 · (D/12)^4 · (pitch/12) · (RPM/1000)^3` [W],
  D and pitch in inches. Accuracy ±10–15% for conventional planforms.
- Torque: `Q = 60·P / (2π·RPM)` [N·m] (exact from P and RPM).
- Static thrust: momentum theory,
  `T = (2·ρ·A·(FM·P)²)^(1/3)` with `A = π(0.0254·D)²/4`.
  FM (figure of merit) typically 0.4–0.7 for small props.
- Max RPM: constant tip-speed structural limit, `RPM_max = K/D`,
  K per APC published limits (65k slow flyer … 270k racing electric).
  Other manufacturers: verify against the prop's own datasheet.

## Calibration

Kp and FM are the two tunable constants. From one thrust-stand point
(measured T, Q, RPM):

- `Kp = P_measured / [(B/2)^0.8 · (D/12)^4 · (pitch/12) · (RPM/1000)^3]`
  where `P_measured = 2π·(RPM/60)·Q`
- `FM = sqrt(T³ / 2ρA) / P_measured`

Plug both in and the curves are calibrated for that prop family.

## Export

- Per-chart PNG buttons, or all three at once.
- CSV of the full curve (rpm, thrust_N, power_W, torque_Nm, over-limit
  flag) with the model parameters in the header comments.

## Assumptions / limits

Static conditions only (J = 0). Constant coefficients across the RPM
range — no Reynolds or Mach corrections; keep tip speed below ~M0.6.
Sea-level ISA density (1.225 kg/m³).

The blade-count factor (B/2)^0.8 is an **unverified heuristic**: the
underlying mechanism (power absorption increases with blade
count/solidity) is established, but the 0.8 exponent has no published
source. For multi-blade props, calibrate Kp per blade count from your
own test data rather than relying on it.

## References

1. R. J. Boucher, *The Electric Motor Handbook*, AstroFlight, 1994.
   ISBN 0964406500. Source of the power formula
   P = k · pitch · D⁴ · N³ (pitch/D in ft, N in kRPM).
2. J. G. Leishman, *Principles of Helicopter Aerodynamics*, 2nd ed.,
   Cambridge University Press, 2006. Momentum (actuator disk) theory
   and figure of merit: FM = (T^(3/2)/√(2ρA)) / P_shaft.
3. E. P. Lesley, "Propeller Tests to Determine the Effect of Number of
   Blades at Two Typical Solidities," NACA TN-698, 1939.
   https://ntrs.nasa.gov/citations/19930081563 — blade-count effect on
   power absorption (mechanism only; the 0.8 exponent is not from this
   or any published source).
4. APC Propellers, "RPM Limits,"
   https://www.apcprop.com/technical-information/rpm-limits/ —
   max-RPM-per-diameter constants by prop series.
5. Typical figure of merit 0.5–0.7 for small propellers: e.g.
   Bauersfeld & Scaramuzza, "Range, Endurance, and Optimal Speed
   Estimates for Multicopters," arXiv:2109.04741.
6. Blade-count factor (B/2)^0.8: community heuristic (RC/UAV prop
   sizing practice), origin untraced, no published derivation.
   Partially consistent with the author's own 8x4 two-vs-three-blade
   thrust-stand measurements (measured coefficient ratio ~1.45 vs
   1.38 predicted, single prop pair). Use as a seed value only;
   calibrate Kp per blade count from test data.
