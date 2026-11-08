# Measured regularities of weight-space writes: four regularities and one interpretation

**Weight-space notes #3.** First public disclosure: 2026-11-08.
Author: Jesse Jenkins. Series home:
github.com/Human-AI-Evolution/weight-space-notes.

## 1. What this note establishes

Notes 1–2 measured *where* post-training behavioral effects live in
weight space (carrier localization; rank-1 vs. distributed
pathology footprints) and showed that evaluation regime changes what
an intervention appears to do. This note states the regularities
those measurements support — five candidate claims from our internal
weight-write program, stated at findings-tier with explicit scope
lines.

Four are **measured regularities**: scoped empirical patterns with
named evidence arms. The fifth — "writes route, never create" — is
**not** a law here. It is a labeled interpretation (Observation →
Interpretation → Prediction) because the evidence supports it only
within the tested regime, and one of our own earlier measurements
cuts against the universal version. We would rather publish the
scoped claim a skeptic can attack than the universal one a skeptic
can falsify in one counterexample.

Nothing in this note discloses the evaluation instrument: batteries,
gold sets, calibration data, constructed weights, and derivation
protocols remain held. What is published is the claim, its scope,
and its evidence class.

Since this note was scoped, an independent replication bears on the
premise above. E. Rizzan
(github.com/clavis-systems/astra-gsm8k-transformers-16k) re-ran the
released Takehi et al. edit on GSM8K (n=100, plan frozen before
generation, HF transformers rather than vLLM, 16K cap) and
reproduces both the direction and the cap-hit drop — original 7/100
with 93 at the cap vs. edited 50/100 with 34. A greedy non-thinking
arm showed no difference detected (47 vs. 43, overlapping 95%
intervals; a separate n=1,319 non-thinking rerun reports 57.1% vs.
55.7%). Same weights, same edit: a large effect in one regime and
no detected effect in the other — the regime-dependence observation
of Notes 1–2, on public inputs.

## 2. The four regularities

*Working definitions, used consistently below.* **Content** —
capability-relevant behavior a delta carries: skills and knowledge
measured by task performance. **Convention** — a learnable surface
mapping: notation, format, chat protocol, the instruction-following
bundle. **Persona/register** — style and persona behavior; measured
here on the same instruction axis as chat-protocol conventions, so
the transport instruction-bundle null is cited under both R2
(convention) and R3 (register). **Address** — the weight-space
location an intervention targets. **Supply** — a function the
unwritten substrate can already compute; operationalized in §3.

### R1 — Learned content transports within a model family, partially

When a post-training delta is re-expressed onto a same-family
substrate by an activation-matched method, *content* direction
survives partially — a full-surface transplant (280 tensors, MLP +
attention, 1.5B→3B) landed coding at .90 vs. the native post model's
.85 and base .80 (n=20), with activation-level alignment cos = .39
against a matched-norm Gaussian control at .017 — roughly 24
standard errors above the null — at an activation-magnitude ratio
of .11, with no persona-axis recovery. The MLP-only sibling arm was
clean but behaviorally inert (coding .80 = base; reasoning
.933 = base; still inert at ×2 dose) — consistent with Note 1's
finding that the coding gain localizes to attention.
A 14-head math-donor transplant into a 7B base (4-bit NF4, both
models) moved GSM8K from 68% to 94% on a 50-item run.
The result replicated at 83% on a 100-item 8-shot CoT run with a
512-token budget (CI95 .745–.891; the two fifty-item halves scored
78% and 88%, a 256-token arm scored 63%, and a separate 0-shot
n=300 arm scored 32%). The same head-swapped model scored 0/20 on
coding on a 6-domain n=20 matrix (reasoning 81.7%,
instruction-following 65%) — the gain was domain-selective with
collateral loss; no pre-swap recipient matrix exists on that
harness, so the loss is directional, not proven causal.

**Scope:** one within-family cross-scale pair plus one same-scale
head swap, two method families, n=20–300 per arm; transport is
partial (right direction, fractional dose), not lossless.
"Within family" is tested, not proven necessary.

### R2 — Conventions did not transport under any tested arm; novel conventions re-derive at a measured cost knee

The convention/persona bundle of the same transplant stayed dead:
instruction-following held at .40 under raw calibration and .50
even at 8× dose, against base .55 and post 1.00.
Mixed chat-template calibration did not recover it either (instr
.50 ≈ base, coding .75) and degraded content alignment
(cos .39→.05).
A different object class — a novel notation, not the transported
bundle — re-derives from data at a measurable cost: on a novel DSL
task (Qwen2.5-3B, n=360 eval), the derivation knee (≥0.9
parse/correct) sat at n=256 examples on attention- and MLP-channel
writes and n=1024 on the full channel — the knee is
channel-dependent, not a constant (the separate single-epoch smoke
cell is not a curve point).
On the same DSL substrate, exposure order changes the outcome:
sequential introduction of a second convention erased the incumbent
(retention ≤.003 at every tested dose) while interleaved
co-teaching retained both conventions (~.94/.94) — interference,
not a simple capacity limit.

**Scope:** transport nulls cover the tested arms of one family
pair; the derivation knee and interference arms are measured on one
substrate and one novel convention pair, single-convention
derivation unless stated. "Did not transport under any tested arm"
is the claim — not "cannot transport."

### R3 — Persona/register is MLP-block-carried and did not transport under any tested arm

Two independent lines agree at findings-tier. The transport campaign's
persona arm is null (R2).
And a direction-transplant arm on a 3B substrate (n=20) returned a negative — persona-as-direction was not linearly steerable at the penultimate layer (arm a1 moved instruction-following +2/20 while costing coding −3/20; arm a2 collapsed coding) — with an open harness caveat: the test-retest floor was not measured, the same base scoring math 1.00 vs .70 and instruction .55 vs .40 across runs, so the null is directional, not precise.
Notes 1–2 supply the 0.8B instance: the style effect rides the
MLP-block replacement boundary, and no tested sub-block partition
recovered it (Note 1, explicitly *not* proven-indivisible); on the
long-horizon pathology Note 2 *interprets* the persona residual as
a distributed footprint, not a rank-1 lesion.

**Scope:** "MLP-block-carried" is Note 1's carrier-boundary
finding, measured on the Qwen3.5-0.8B/2B family; R3's transport and
direction nulls bound behavior — they do not re-measure the
boundary on Qwen2.5. "Did not transport" covers the tested arms
only. We do not claim indivisibility.

### R4 — Deltas are substrate-relative: the address transfers, the content does not

Cross-architecture transplant fails outright — 7B→1.5B translation gained only +0.27 BLEU against the same-recipe baseline's +14.53 — and a same-architecture donor transplant did not restore the ablated function beyond retraining alone (36% vs 38%; unablated 48%; n=50) (both pre-A0 harness, directional).
The transport campaign's own conclusion matches: "what survives transport = skills that both bases already express; what dies = behavior only the post model has."
Note 2's transplant cell bounds R4 rather than contradicting it:
the released direction re-applied to the base-MLP substrate halved
strict detected-loop (.58→.26) with accuracy flat (.30→.26, n=50) —
the address still hit the attractor, the accuracy effect did not
follow. Note 2 §5 notes the direction was estimated against post
weights, so this is the weakest R4 cell, not the strongest.

**Scope:** transplant failures are measured at small scale on one
family pair; "substrate-relative" is a regularity of the tested
interventions, not a theorem about all weight edits.

## 3. The interpretation: writes route what substrate supplies

- **Observation:** a perfectly-confined write can train and still not create — on a K/O-projection write channel (digit-arithmetic dead-write cell) the write confined inside its mask at inside_frac 1.000, drove loss 5.12→3.82, and grew a digit-attractor (first-token .06→.53) while exact answers stayed at floor (norm_exact 0.02→0.02).
- **Observation:** independently, a pass@64 screen found 3-digit addition at 0/40 where the 2-digit control passed 15/16, and a follow-up arm with a landed positive control (2-digit .77) confirmed the substrate genuinely lacks 3-digit supply.
- **Operational definition:** "supply" is measured as pass@k
  (k=64) on the unwritten substrate with a same-format positive
  control passing; "null" means at floor under that screen. Three
  cells meet this definition (the A7, X6B, and C8 cells — see
  appendix), all on one toy substrate family and one task class.
- **Interpretation:** within the tested substrates and intervention classes, weight-space writes route, re-weight, unmask, or overwrite capability the substrate already supplies; none of the tested writes produced a capability the substrate lacked supply for. The interpretation covers *computations* the substrate must perform; it does not cover *conventions* (surface mappings), which R2 shows are derivable from floor at a measured knee. We did not run the supply screen on the R2 convention, so R2 does not test the interpretation — the two sections are not mutually confirming. This is the measured version of "writes route, never create" — scoped, not universal.
- **Prediction (falsifiable):** a write applied to a supply-verified-null capability — positive control passing, target at floor — should fail to produce it. A demonstrated counterexample (capability appearing where supply was verified absent) falsifies the interpretation within its tested regime. The discriminating arm is specifiable on public inputs — e.g., a public ≤1B base at pass@64 floor on k-digit addition with a (k−1)-digit control passing; a LoRA write of ≤1,024 examples at the target; prediction: target stays at floor. Positive control and screen are fully specified above; nothing held is required to run it.

## 4. Provenance

- **First public disclosure:** 2026-11-08.
- **Experiments conducted:** the arms above span 2026-08-04
  (E5/E6 transplant rows) through 2026-10-17 (B0 direction arm);
  the transport campaign ran 2026-10-11–12, the convention cost
  curve 2026-10-13–14, the write-channel/supply arms 2026-10-05–07.
- **Anchoring:** the 2026-10-11 arms fall inside the
  OpenTimestamps-anchored manifest window cited in Note 1 §3;
  earlier arms predate the anchoring practice — git history and
  scoreboard rows are the record for those. Manifest/proof receipts
  for the anchored window available on request.

## 5. Limitations

Single pretrained family (Qwen2.5/Qwen3.5) for R1–R4. The §3
supply arms ran on ~23M-parameter from-scratch substrates built for
the purpose (8-layer decoder, GPT-2 tokenizer, synthetic arithmetic
corpora); the interpretation is anchored at toy scale and its
transfer to the pretrained family is a prediction, not a
measurement. Per-arm cells range n=20–360 with some single-seed
runs; several arms carry open
harness flags (B0 test-retest floor unmeasured; E5/E6 predate the
current harness). R1–R4 are regularities of the tested interventions
on the tested substrates — scope lines are part of the claims, not
boilerplate. **Known tension:** our latent-coverage analysis found
capability written where the base had no linearly-decodable latent — latent-coverage probing did not predict writability (Spearman −.119, p=.78 over 8 cells) — so "supply" in the interpretation means substrate-computable function, not probe-decodability, and the interpretation is scoped accordingly. It is the least-supported of the five claims by design; that is why it is an interpretation.

## 6. What is checkable

R1–R4 cite scoreboard rows and artifact names (appendix) — the
underlying arms are private-battery measurements, labeled as such.
R1's head-swap result is checkable on public inputs end-to-end:
donor and recipient are public Qwen releases and GSM8K is a public
benchmark — no private battery required. The interpretation's
falsifier is specifiable on public inputs (the §3 operational
definition and discriminating arm are fully stated); constructed
weights, gold sets, calibration data, and derivation protocols are
held. We welcome a small number of externally supplied variants for
evaluation under a fixed protocol, results published by agreement.

## Appendix — evidence map

- R1: `XPORT-FULL-AND-DOSE-1.5B-TO-3B`, `XPORT-CTRLNULL-1.5B-TO-3B`,
  `XPORT-MLP-1.5B-TO-3B` (negative, cited), `head-swap-gsm8k`,
  `GSM8K-7B-headswap-replication`, `Bench-7B-headswap-key-domains`
  (negative, cited)
- R2: `XPORT-FULL-AND-DOSE-1.5B-TO-3B`,
  `XPORT-CHATCALIB-FINAL-1.5B-TO-3B`, `FSG-DSL-COSTCURVE-3B`,
  `FSG-DSL-INTERFERENCE` (I1/I2/I3)
- R3: `XPORT-B0-DIR`, plus the Note 1 carrier-boundary arm and
  Note 2 transplant cell (cross-references below)
- R4: `E5-TRANS`, `E6-MATH`, `XPORT-CHATCALIB-FINAL-1.5B-TO-3B`,
  Note 2 transplant cell
- §3: `A7-KO-CHANNEL`, `X6B pass@64 screen`, `C8 A6-CONTROL`,
  `PROBE-GAIN-CORR`
- Transport campaign artifacts: `nn_xport_*.json`,
  `nn_xport_cos_*.json`; write-channel/supply: `nn_a7.json`,
  `nn_c8_a6control.json`; convention curve + interference:
  `fsg_analysis.json`, `fsg_i1_nex256.json`, `fsg_i1_nexs256.json`,
  `fsg_i2_mix512.json`, `fsg_i3_nexs64.json`, `fsg_i3_nexs1024.json`
- Third-party replication: E. Rizzan, "The Astra weight edit on
  Qwen3.5-0.8B: a GSM8K rerun with transformers and a 16K budget,"
  Nov 2026, github.com/clavis-systems/astra-gsm8k-transformers-16k
- Cross-references: Note 1 §2 (carrier boundary), Note 2 §1
  (distributed-write footprint, transplant partial)
