# Selective post-training weight interventions: carrier localization and evaluation-regime dependence

**Weight-space notes #1.** First public disclosure: 2026-11-04.
Author: Jesse Jenkins. Cross-posted: github.com/human-ai-evolution/weight-space-notes (canonical),
arXiv (pending endorsement), and the RikiyaT/Qwen3.5-0.8B-
weight-space-programming model discussion.

## 1. What this note establishes

We independently investigated how post-training behavioral changes
distribute across weight-space components in Qwen3.5-0.8B and
Qwen2.5-2B-Instruct. In the tested models and interventions,
different behavioral effects localized differently — style/persona
writes ride the MLP-block replacement boundary, the release's coding
gain localizes to attention, and the highest-magnitude tensors
(gates, QKV, DeltaNet) are behaviorally inert. Because the effects
live on separable carriers, gains and regressions can be addressed
independently in weight space — and we show a constructed variant
that recovers the release's retained gains while dropping the
carried regression at 2B scale.

As a timely application, we evaluated the recently released
weight-space-programming edit of the same 0.8B substrate (Takehi,
Teoh, Zhang, Khattab — edited model public on HF) under two
protocols: our short-response battery and a long-horizon
thinking-on math protocol in their evaluation regime.

## 2. Central result: carrier localization and separability

| Element | Detail |
|---|---|
| Intervention | Surgical module-class variants: each post-training component class (attn, MLP-block, DeltaNet, gates, QKV) independently swapped to its base-model weight. Variants retain or remove exactly that class's post-training delta. |
| Comparison | Release model ("post") vs. base vs. single-class and combined variants; plus constructed variants (post + selected base-component replacements, no eval feedback used in variant construction). |
| Evaluation | Six behavioral axes (coding, math, logic, pattern, overall, instruction-following), n=20–50 per arm, chat-template protocol, short and verbosity-fair token budgets. Powered arm: paired item-level bootstrap, n=200, 2B scale. |
| Result | At 2B: the MLP-only variant beats the release on math — 14/0 discordant item pairs, p<.001, paired bootstrap improvement +7.0pp CI [+3.3, +10.7]. Constructed variant matches-or-beats the release on all six axes at n=50 (2B). |
| Selection history | The factorial carrier contrast and the powered math comparison were specified before the runs; variant construction is blind (no eval feedback). Axes/arm counts are in the appendix. |
| Boundary | Construction is scale-local: at 2B the variant dominates; at 0.8B the fair-budget result is 4/6 axes without dominance; at 4B parity. The tested sub-block partitions did not recover the block-level style effect — we call the block the carrier boundary, not proven-indivisible. |

## 3. Provenance

- **First public disclosure:** 2026-11-04.
- **Experiments conducted:** carrier map and construction,
  2026-10-10–11; powered factorial arm, 2026-10-11; cross-validation
  of the released edit, 2026-11-03–04; crossed-intervention
  follow-up (§5), 2026-11-05.
- **Supporting historical records:** the Oct-11 results were hashed
  into a 578-file manifest (scoreboard rows SURGICAL-EVAL-CHAT-N20,
  FACTORIAL-2B-N200, B18-CARTDIFF; findings docs) and submitted to
  the OpenTimestamps Bitcoin-calendar network on 2026-10-11 at
  06:21 and 23:53 UTC — and the calendar attestations are now
  complete in the Bitcoin chain (proofs verifiable with the standard
  OpenTimestamps client). This is external, third-party anchored
  evidence of possession, not merely internal git dates. Manifest
  and proof receipts available on request (they disclose file
  hashes, not content).

## 4. Timely application: the released weight-space edit

The released edit removes most of the model's degenerate looping by
subtracting one MLP-carried direction (0.016% of weights, layers
15/19) — reported MATH-500 7.9%→48.1% (independent HF rebuild:
51%, 31.6% cap-hits, mean generation ~21.7K tokens; their shipped
base JSON is a 25-item smoke subset).

**Our battery** (short-response, n=20 chat): no difference detected
between astra and post — a ceiling-limited null, not equivalence.
Their edit targets long-horizon looping; our battery resolves
early-token behavior. So we built a long-horizon probe for their
regime.

**Long-horizon probe** (MATH-500 n=50 seed-0, 16K cap, thinking-on,
their sampling config — matched base-vs-edit, our protocol):

| model | acc | cap-exhausted | detected loop | mean gen len |
|-------|-----|---------------|---------------|--------------|
| post  | .08 | .98 | .98 | 16,384 |
| astra | .34 | .68 | .70 | 16,384 |

(mean lengths are at-cap for both; cap-hit share does the
differentiating here. Loop flag = >30% duplicate 8-grams — see
appendix. The flag over-counts on notation-heavy productive runs:
of astra's 35 flagged items, 26 also hit the cap and 11 produced
correct answers. Strict degenerate-loop estimate (flagged ∧ capped):
astra .52 vs post .98.)

Under this matched protocol we reproduce their base cell almost
exactly (8% acc / 98% cap vs their 7.9%/93% at 32K) and observe a
large improvement after editing. Our 16K numbers differ from their
32K numbers — we have not isolated token budget from other protocol
differences — but direction and base anchor hold. A non-thinking
control (same items, same models) shows post cap-hits at .26: we
did not observe their failure mode in the tested non-thinking
condition.

**Crossed intervention (the §5 follow-up, run 2026-11-05).** Same
items, cap, and sampling; five cells:

| model | acc | cap-exhausted | detected loop | mean gen len |
|-------|-----|---------------|---------------|--------------|
| base  | .40 | .36 | .22 | 7,050 |
| post  | .08 | .98 | .98 | 16,384 |
| astra | .34 | .68 | .70 | 16,384 |
| nomlp | .30 | .74 | .58 | 12,915 |
| nomlpdelta | .26 | .60 | .28 | 12,053 |

(`nomlp` = post with all MLP-block tensors reverted to base;
`nomlpdelta` = nomlp with the released edit's two-tensor delta
re-applied on the base-MLP substrate — constructed weights held.
Strict degenerate-loop = flag ∧ cap: base .22, post .98, astra .52,
nomlp .58, nomlpdelta .26.)

## 5. Observation / interpretation / prediction

- **Observation:** the released edit improves long-horizon task
  performance without a detectable change on our short-response
  battery; both instruments measure the same edited artifact.
- **Interpretation:** consistent with a dissociation between the
  edited failure mode and the style effects our block interventions
  measure. It does not yet establish that block-level organization
  explains the remaining long generations.
- **Prediction:** if the dissociation is real, a crossed
  intervention — base × their edit × our block intervention ×
  combined — under one long-horizon protocol should change the two
  outcome classes independently. That is the specified follow-up
  experiment.
- **Follow-up result (2026-11-05):** on the loop axis the carriers
  behave roughly additively — each intervention alone removes about
  half the excess looping (strict .98 → .52 astra / .58 nomlp), and
  combined they approach the base phenotype (.26 vs .22). On the
  accuracy axis they do not stack: every edited cell stays below
  base (.40), and the combination (.26) sits below either single
  intervention. Loop suppression and task recovery are therefore
  partially dissociated even among the edited models — consistent
  with separable carriers, not yet a clean independence proof.
  Notably, the pre-post-training base is the best cell on both
  axes: in this regime, post-training itself is the looping
  regression.

## 6. Limitations

Single model family (Qwen), three scales, moderate n. Short-cap
scores measure answer-in-budget, not raw capability. Detected-loop
share and cap exhaustion are related but distinct — our classifier
is structural (8-gram repeats) and can misclassify slow productive
reasoning; a reviewed sample quantifies the noise (appendix). Their
eval used 32K and MATH-500-train practice
items; ours uses test items at 16K — same benchmark, different
budget and split.

## 7. What is checkable

The long-horizon probe runs entirely on public inputs: the released
edited model, MATH-500, documented generation settings, and a
stated loop-classification rule — reproducible end-to-end without
our private stack. Private-battery findings are labeled as such;
constructed weights, calibration data, and item sets are held.

We welcome a small number of externally supplied variants for
evaluation under a fixed protocol, results published by agreement.

## Appendix — evidence map

- surgical battery artifacts `nn_surgical_eval_wsp_astra{,_x4}.json`,
  instrument-check artifact `instrument_check_wsp-astra.json` — battery arm
- de-loop artifacts `nn_deloop_wsp_astra{,_thinking}.json` — probe arm
  (non-thinking control + thinking-on main)
- crossed-intervention cells `nn_deloop_wsp_{cross,base}.json` —
  §4 table + §5 follow-up result
- Scoreboard rows: `SURGICAL-EVAL-CHAT-N20{,-X4}` (0.8B/2B),
  `SURGICAL-EVAL-WSP-ASTRA{,-X4}`, `FACTORIAL-2B-N200`,
  `B18-CARTDIFF`, `DELOOP-WSP-ASTRA-THINKING-MATH500-N50`
- **Probe protocol (fully public inputs):** post =
  `Qwen/Qwen3.5-0.8B` rev `2fc06364715b967f1860aea9cf38778875588b17`;
  astra = `RikiyaT/Qwen3.5-0.8B-weight-space-programming` rev
  `d1671a178705bcab7644806d23c535517f35723b`; base =
  `Qwen/Qwen3.5-0.8B-Base` rev `dc7cdfe2ee4154fa7e30f5b51ca41bfa40174e68`.
  Items: MATH-500 `test.jsonl` rows 0–49 (seed-0 order, ids
  `math500-0`..`math500-49` in the artifact). Prompt: problem +
  "Please reason step by step, and put your final answer within
  \boxed{}." Chat template with `enable_thinking=True` (v2 control
  used the default non-thinking render). Sampling: temp 0.6,
  top_p 0.95, top_k 20, repetition_penalty 1.0, no presence_penalty,
  max 16,384 new tokens. Scoring: boxed-answer extraction +
  normalized string match (eval_math). Loop flag: share of
  duplicate 8-grams in generation >0.3 — known noise on
  notation-heavy productive reasoning (11/35 astra flags were
  correct answers); strict loop = flag ∧ cap-exhaustion.
