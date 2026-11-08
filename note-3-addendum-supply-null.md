# Weight-Space Note 3 — Addendum: the §3 falsifier, executed

*Posted 2026-11-08. Reports the first run of Note 3 §3's published
prediction on public inputs.*

Note 3 §3 ended with a falsifiable prediction rather than a claim: on a
public ≤1B base, a function class on which the base is at floor under a
sampling screen — with a same-format positive control passing — should
stay at floor after a targeted weight-space write. A capability
appearing where supply was verified absent would falsify the "writes
route, never create" interpretation within its tested regime.

This addendum reports that experiment, run end-to-end on public inputs.

## Protocol

- **Base:** Qwen3.5-0.8B (public weights, local snapshot).
- **Screen:** pass@64, temperature 0.8, n=40 problems per cell, 2-shot
  format prompt (`40 + 12 = 52`-style), first-integer exact match.
- **Floor criterion:** pass@64 ≤ 0.02 on the target class with pass@64
  ≥ 0.5 on the (k−1)-digit same-format control — the control proves the
  format is expressible, so a target floor is capability-level, not
  formatting noise.
- **Write:** attention LoRA, rank 16, on `q/k/v/o` projections; 1,024
  examples of the floored class; 1 epoch; AdamW lr 2e-4; 256 steps;
  1.08M trainable parameters.

## Screen

Two classes were screened. **Addition produced no floor:** pass@64 was
1.000 at k=2–4, .875 at k=5, .750 at k=6, .700 at k=7 — this base
retains measurable supply for 7-digit addition, so the note's example
class was not runnable here. That is itself the point of screening
first: floor existence is a measurement per model per class, not an
assumption.

**Multiplication floored cleanly:**

| k-digit × k-digit | pass@64 (pre-write) |
|---|---|
| 1 | 1.000 |
| 2 | 1.000 |
| 3 | .575 |
| 4 | **.000** |
| 5 | .000 |

floor = k4: verified absent at pass@64, k3 control passing at .575.

## Result

| | pre-write | post-write |
|---|---|---|
| k4 target | .000 | **.075** |
| k3 control | .575 | .900 |
| k5 | .000 | .000 |

**The prediction held, with the honest numbers stated.** The write was
demonstrably capable — it lifted the adjacent class from .575 to .900 —
but on the supply-verified-null class it produced only noise-level
hits: 3 of 40 items with ≥1 correct in 64 samples, below the
pre-registered falsification threshold (>0.10 with control intact).

Two caveats, stated rather than smoothed:

1. **.075 is not .000.** The target moved off exact zero. We read it as
   noise-level generalization (the write improved 4-digit arithmetic
   marginally everywhere it had partial supply), but the number is what
   it is — we report it, not "stayed at zero."
2. **One regime.** One model (0.8B), one class (multiplication), one
   write form (attention LoRA), one screen. The interpretation survives
   *within this tested regime*; it is a data point for §3, not a proof
   of it.

## What this changes about the interpretation

Nothing structurally — and that is the result. Note 3's §3
interpretation was published with its falsifier attached, and the first
public-inputs execution of that falsifier came back negative. The
interpretation stands at the same epistemic tier it held before: a
demoted, scoped claim now with one pretrained-family data point added
to the ~23M-parameter from-scratch measurements.

The stronger version remains open to attack the same way: find a base
and class where the screen verifies absence, run the write, and check
whether the capability appears. If it does, we would like to hear about
it — the falsifier is the offer.

## Reproducibility

Every input above is public: the model (Qwen3.5-0.8B on Hugging Face),
the screen shape, the thresholds, the write recipe, and the example
counts. Per-item rows (all 2,560 screen samples per cell, pre and post)
are retained in the measurement artifact; the aggregate JSON and the
harness are available on request.
