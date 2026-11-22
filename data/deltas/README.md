# Deltas — SGNE-CC footprint ablations

Ablated variants of the upstream `sgne_cc_dna_delta.npz` patch
(Nondual-Observer/dna-weight-editing, applied to `Qwen/Qwen3.5-0.8B`),
produced for the mechanism arms reported in
RikiyaT/lm_understand-logs issue #4
([comment](https://github.com/RikiyaT/lm_understand-logs/issues/4#issuecomment-7062726129)).

Same npz schema as the upstream patch (`<tensor>_changed_cols` +
`<tensor>_cols_bf16_u16` key pairs); applies with the same
`apply_dna_patch.py` flow against the same pinned base revision.

| file | sha256 | contents |
|---|---|---|
| `sgne_cc_dna_delta_mlponly.npz` | `175e28ae2774988f66dafc0549ff615c53c425f70a58d012325fe1ae26163cc9` | MLP `down_proj` column rotations only — 802 changed columns across 15 tensors; drops the 9 GDN `out_proj` tensors and the L16 gate row |
| `sgne_cc_dna_delta_nogate.npz` | `18b580def10c79244055e10bab2c45e58b92813368f0cbe4a50cebda5b708b8d` | Full patch minus `da_L16_full_bf16_u16` — 802 MLP + 1,182 GDN changed columns, gate row zeroed |

Upstream reference: `sgne_cc_dna_delta.npz` sha256
`4e4d36e032869bc10f56e2a2033a52d3f2774321cf867ef6f74274a7c0d1566d`
(49 keys: 30 MLP + 18 GDN + 1 gate row).

Eval cells for both variants (untouched-300, greedy, 32K cap) are in
`../shared-rows/sgnecc-mlponly-greedy-math500-32k.jsonl` and
`../shared-rows/sgnecc-nogate-greedy-math500-32k.jsonl`.
