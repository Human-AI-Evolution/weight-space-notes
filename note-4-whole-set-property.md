# The whole-set property: distributed relief, indivisible selectivity — cross-instrument falsification of revert decompositions

**Weight-space notes #4.** First public disclosure: 2026-11-28.
Author: Jesse Jenkins (posting as JesseJ2nd on the RikiyaT model
discussion). Series home:
github.com/Human-AI-Evolution/weight-space-notes.

## 1. What this note establishes

Note 1 localized the post-trained commit/loop effect to the MLP-block
carrier; Note 2 showed a blunt whole-block revert recovers most of the
released edit's de-loop effect. The open question was whether the
carrier *decomposes*: can a proper subset of the block's 72 MLP tensors
carry the repair?

Two candidate decompositions have now been tested, on two independent
instruments in two labs. Neither carries the selective part:

- **Column-family.** A `down_proj`-only revert (24 tensors — the write
  side) recovers most of the general relief but fails the truth-led
  close on the sign-close instrument (4 of 16 sign-carrying runs;
  pre-registered threshold ≤30%), and on our axis recovers only half
  the whole-block accuracy.
- **Complement.** A `gate_proj`+`up_proj`-only revert (48 tensors — the
  read side) is *not inert*: it buys most of the loop/cap relief on our
  axis while recovering still less accuracy.
- **Half-blocks.** Early/late 12-block partitions fail on the
  sign-close instrument (2 of 13, 3 of 18).

Our reading: **"wants the whole set" is a joint property, not a
decomposition.** The general relief — token-loop share and cap share —
distributes non-specifically through the block: roughly any half buys
most of it. The *selective* component — accuracy on our instrument,
truth-led close on theirs — is what requires the complete set. A
write/read split would predict the read side sits near the unedited
floor; it does not.

The column-family hypothesis this falsifies was ours — posted on the
RikiyaT discussion as a discriminating arm before the result existed.
We take the refutation; the surviving claim is the one both
instruments agree on.

Nothing in this note discloses the evaluation instrument beyond what
the series has already published: batteries, gold sets, calibration
data, constructed weights, and derivation protocols remain held. The
revert tensor lists are stated in full below — they are architecture
names, not method.

## 2. The arms

All arms are post-trained Qwen3.5-0.8B with the named base tensors
restored; all other MLP tensors stay post-trained. `mtp.layers.0.mlp`
stays post in every arm — the same scope as the whole-block revert.

| arm | base tensors restored |
|---|---|
| `down_proj`-only | `model.language_model.layers.{0..23}.mlp.down_proj.weight` (24) |
| `gate+up`-only | `model.language_model.layers.{0..23}.mlp.{gate,up}_proj.weight` (48) |
| whole-block (R-MLP / `nomlp`) | all 72 `mlp.*_proj.weight` |
| early / late half | 12 consecutive blocks' full MLP set (their instrument) |

## 3. Cross-instrument results

**Instrument A — sign-close measure** (NeoCortex0815, llama.cpp q8_0,
pre-registered verdicts; same 50 problems, IFEval n=120; v0.6
§§3.15–3.21, DOI 10.5281/zenodo.24601235):

| revert | correct /50 | loop-or-32K-cap | closes at correct-value sign | IFEval |
|---|---|---|---|---|
| unedited | 5 | 45 | 1 of 11 | 18 |
| `down_proj` only | 18 | 30 | **4 of 16** | **61** |
| whole-block (R-MLP) | 23 | 23 | 21 of 24 | 44 |
| early / late half | 11 / 13 | 38 / 37 | 2 of 13 / 3 of 18 | 34 / 53 |
| base (ref) | 27 | – | 19 of 21 | 44 |

Pre-registered verdicts as run: door-close met only just (12 of 20,
exactly the 60% threshold); truth-led close **refuted**; IFEval open.

**Instrument B — acc/cap/loop axis** (this series' protocol,
PyTorch BF16, thinking-on MATH-500 n=50 seed-0, 16K cap):

| revert | acc | cap share | loop share | mean tok |
|---|---|---|---|---|
| `down_proj` only (24) | .22 | .82 | .68 | 14,456 |
| `gate`+`up` only (48) | .16 | .78 | .70 | 13,002 |
| whole-block (72) | .30 | .74 | .58 | — |
| unedited / base (refs) | .08 / .40 | .98 / .36 | ~.98 / .22 | — |

## 4. Reading: joint property, not decomposition

On instrument A, `down_proj` alone recovers ~72% of the unedited→R-MLP
correct gap and ~68% of the loop-or-cap gap — the general effect is
mostly there. What is missing is precisely the selective component:
runs close, but not at the sign carrying the correct value (4 of 16).

On instrument B the complement arm supplies the datum instrument A
cannot see: `gate+up` alone sits at .78 cap / .70 loop — most of the
whole-block's .74/.58 relief — while recovering only .16 accuracy
against the whole block's .30. If the write/read split were a clean
fact, the read-side arm should have remained near the unedited floor
(.98 cap, ~.98 loop). It did not.

Both directions of the same result: *relief is non-specific* — token-
and cap-relief accrue from roughly any half of the block — while
*selectivity is indivisible*: accuracy and truth-led close each require
the complete set. This is consistent with the earlier weight-only
observation (theirs) that door/sign-token writers are nearly unchanged
by post-training: post-training did not erase the door tokens; it
corrupted the *when*, and the *when* needs the firing pattern
end-to-end.

## 5. A separable datum: commitment without ceremony

Two independent cells now show repair restoring behavior without
restoring deliberative structure:

- **SGNE-CC** (Nondual-Observer's edit, seed-0 main table): 22 of 50
  correct with only 5 signpost-bearing runs — commitment mostly
  without the sign→door ceremony; closes early and finishes the
  computation in the answer part.
- **`down_proj`-only**: restores solving (18 of 50) without truth-led
  close (4 of 16) — repair without selectivity.

Both directions of the same separation: weight-carried repair need not
rebuild the sign structure. Instrument caveat carried: the signpost
pattern is narrow and misses some closing phrases (v0.6 §3.18) — "a
close without a signpost is not a wrong close" — so the reading is
*commitment without measured ceremony*, not commitment without any
ceremony.

Earlier flag-back numbers cited on the thread (S-GDN 29, SGNE-CC
20-correct/1-signpost) were seed-1 rows; the seed-0 main-table values
are 26 and 22/5 respectively. Both observations stand with softer
magnitude; both seeds are in the v0.6 release.

## 6. Predictions

Two falsifiable, stated before the runs that would test them:

- **Dose-response at scale.** If relief is non-specifically
  distributed, half-block reverts at 4B should buy *partial* relief —
  monotone in dose, not none-or-all. An all-or-nothing outcome at a
  larger scale would refute the non-specificity reading.
- **Scale replication of selectivity.** If an R-MLP-equivalent at 4B
  restores closing only at the base-floor rate, "wants the whole set"
  earns its scale replication; if a subset restores it, the
  joint-property claim is scale-relative.

A caution the second prediction needs: on this family's 4B member,
every variant on our short-cap battery — *base included* — burned its
whole budget on a "Thinking Process:" scaffold, computing the answer
mid-preamble and never emitting it. The same surface shape as the
commit gap — answer present, commitment absent — pre-exists
post-training at that scale. A separating instrument there must
distinguish the pre-existing base floor from the post-trained loop,
or every arm floors equally.

## 7. An open cell

The `down_proj`-only IFEval score lands *above* base (61 vs 44 — and
vs R-MLP's 44). On one seed and n=120 it could be noise. If it holds
on a second seed, the configuration "post-trained read side + base
write side" outperforms either full configuration on instruction
following — a datum that post-training wrote something the IFEval
axis likes into `gate`/`up` while damaging `down`. Unresolved; flagged,
not claimed.

## 8. Provenance

- **First public disclosure:** 2026-11-28.
- **Experiments:** instrument-A arms run 2026-11-27/28 with
  pre-registered verdicts (their dated log); instrument-B arms run
  2026-11-28 (job `mlpdownrev-n50-r2`).
- **Public record:** both instruments' cell tables and the falsified
  column-family prediction are on the RikiyaT discussion (events
  `6b09f46180690c00e66fb914`, `6b0ab24cfc392aea491ccf17`,
  `6b0b33842b680e05fb061e70`).
- **Shared rows:** the instrument-A per-item rows merge into this
  repo's `data/shared-rows/` under the v0.6 citation
  (DOI 10.5281/zenodo.24601235); the existing merged table already
  carries their 16K cells.

## 9. Limitations

Single substrate (Qwen3.5-0.8B); the claim is scale-pending on the 4B
arm. Instrument-B cells are n=50, single-seed, private battery —
labeled as such; the scoreboard row and artifact names are cited in
the appendix. The two instruments measure different facets of the
same phenotype; "same verdict" means both falsify subset-carriage of
the selective component, not that the measures are interchangeable.
The non-specificity reading is an interpretation over four arms on
one substrate — the dose-response prediction in §6 is what would
test it.

## 10. What is checkable

Instrument A is end-to-end checkable on public inputs: the base and
post-trained weights, the R-MLP variant weights, the v0.6 report,
and the per-item rows are all public (DOI above). Instrument-B cites
private-battery rows; the revert tensor lists are fully stated in §2
so the arm construction is reproducible on public weights, but the
scoring protocol is held. Constructed `down_proj`-only and
`gate+up`-only variants exist and can be supplied for evaluation under
the series' standing agreement terms.

## Appendix — evidence map

- Instrument-B arms: `DELOOP-WSP-MLPDOWNREV-THINKING-MATH500-N50`;
  artifacts `nn_deloop_mlpdownrev*.json` (+ per-item `.rows.jsonl`).
- Whole-block reference: `DELOOP-WSP-CROSS-THINKING-MATH500-N50`
  (`nomlp` arm, Note 2); base: `DELOOP-WSP-BASE-THINKING-MATH500-N50`.
- Instrument A: NeoCortex0815 v0.6, DOI 10.5281/zenodo.24601235,
  §§3.15–3.21 (sign-close tables, seed-0/seed-1 split, §3.18
  instrument caveat); R-MLP weights public
  (`NeoCortex0815/Qwen3.5-0.8B-R-MLP`, BF16 + q8_0 GGUF).
- SGNE-CC separability cells: `data/shared-rows/` SGNE-CC arms
  (five-arm replication, second stack) and their v0.6 seed-0 table.
- Thread record: RikiyaT/Qwen3.5-0.8B-weight-space-programming
  discussion #2, events listed in §8.
- Cross-references: Note 1 §2 (carrier boundary), Note 2 §1
  (blunt whole-block revert, transplant cell), Note 3 R3
  (MLP-block-carried register claim).
