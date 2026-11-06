# Precision vs. blunt surgery: rank-1 pathologies and distributed writes in post-trained weights

**Weight-space notes #2.** First public disclosure: 2026-11-05.
Author: Jesse Jenkins. Series home:
github.com/Human-AI-Evolution/weight-space-notes.

## 1. What this note establishes

Note 1 reported that post-training behavioral effects localize to
weight-space carriers, and evaluated the released
weight-space-programming edit of Qwen3.5-0.8B under two regimes.
This note crosses the two interventions on the same artifact: their
single-direction MLP edit and our MLP-block revert.

The discriminating result: **a blunt MLP-block revert — no
direction-finding, no activation probing — recovers most of the
released edit's de-loop effect** (accuracy .30 vs. their .34;
detected-loop .58 vs. their .52, n=50 thinking-on MATH-500). The
loop pathology and the persona share the post-MLP carrier, and a
rank-1 pathology can be excised either by a precisely found
direction or by reverting the block that carries it. Their
direction also partially transplants onto the base-MLP substrate
(strict detected-loop .26 vs. .58), so it is not merely a
post-context artifact.

Read together: **pathologies differ by surgical footprint.** A
looping attractor is a rank-1 lesion — one direction removes it,
and the same direction works on a reverted substrate. A persona is
a distributed write — no single direction removes it; the block
boundary is the unit of surgery.

## 2. The crossed-intervention result

| Element | Detail |
|---|---|
| Interventions | (a) released weight-space edit: −direction in `layers.15/19.mlp.down_proj` (2 tensors, verified diff footprint); (b) MLP-block revert: all post-MLP-block tensors replaced by base; (c) transplant: revert + direction re-applied to the base-MLP substrate. |
| Comparison | post (unedited) / astra (their edit) / nomlp (block revert) / nomlpdelta (transplant) — same items, seed-0 order, paired within run |
| Evaluation | MATH-500 n=50, thinking-on chat template, 16,384-token cap, their sampling config (temp .6 / top-p .95 / top-k 20 / rep 1.0). Metrics: boxed-answer acc, cap-exhaustion, 8-gram loop flag; strict loop = flag ∧ cap. |
| Result | post .08 acc / .98 strict — astra .34 / .52 — nomlp .30 / .58 — nomlpdelta .26 / .26 |
| Selection | the 2×2 was specified in Note 1 §5 as the prediction test before the runs; no arms dropped |
| Boundary | single model, single benchmark, n=50; transplant uses their post-derived direction on base — portability is partial (loop halved, acc flat); the blunt revert is not free: it removes the whole MLP write, persona included |

## 3. Provenance

- **First public disclosure:** 2026-11-05. The four-cell result
  table was appended to Note 1 on the same date (series commit
  `279a340`); this note is the standalone mechanism treatment of
  that result — the interpretation and prediction are the new
  content, not the numbers.
- **Experiments conducted:** construction and probe runs 2026-11-04;
  prior cells from Note-1 arms 2026-11-03–04.
- **Supporting records:** run artifact and scoreboard row
  `DELOOP-WSP-CROSS-THINKING-MATH500-N50`; the released edit's
  footprint was independently re-verified by full-tensor diff
  (exactly 2 differing tensors, `layers.15/19.mlp.down_proj`).

## 4. Observation / interpretation / prediction

- **Observation:** block revert ≈ surgical de-loop on this
  pathology (within n=50 noise on acc; .58 vs .52 strict loop);
  transplant halves residual looping without changing accuracy.
- **Interpretation:** the loop attractor and the verbose persona
  are co-located on the post-MLP block but have different
  footprints — rank-1 vs. distributed. This explains both why the
  released single-direction edit worked and why it left the persona
  residual intact (Note 1 §4).
- **Prediction:** other rank-1-shaped pathologies (repetition
  attractors, format loops) on this substrate should be excisable
  by either a found direction *or* a carrier-block revert;
  distributed behaviors (register, style, self-report) should
  resist single-direction removal. The discriminating follow-up:
  apply the same crossed design to a second failure mode and a
  second substrate — Note 3.

## 5. Limitations

n=50, one benchmark, one substrate, one block boundary. Strict-loop
counts carry the Note-1 classifier caveat (structural 8-gram rule;
notation-repetition noise affects all arms equally). The
transplant's partial effect could reflect a direction estimated
against post weights rather than an intrinsic rank-1 property.
The block revert is a destructive instrument by construction —
its point is mechanistic, not prescriptive.

## 6. What is checkable

Fully reproducible on public inputs: the released model +
Qwen3.5-0.8B/-Base + MATH-500 define every arm; the block-revert
mask is a stated set of tensor names, and the transplant is
`base + (astra − post)` on the two published tensors. Probe
protocol identical to Note 1's appendix (thinking-on template,
sampling, scoring, loop rule). Constructed variant weights are
held; the construction is one tensor-arithmetic line.

## Appendix — evidence map

- `nn_deloop_wsp_astra_thinking.json` (post/astra cells),
  `nn_deloop_wsp_cross.json` (nomlp/nomlpdelta cells)
- Scoreboard rows `DELOOP-WSP-ASTRA-THINKING-MATH500-N50`,
  `DELOOP-WSP-CROSS-THINKING-MATH500-N50`
- Construction: `nn_cross_build_nomlpdelta.py` — verifies the
  2-tensor footprint before transplanting
