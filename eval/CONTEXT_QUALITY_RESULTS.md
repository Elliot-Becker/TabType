# Context preservation and quiet-input regression results

Measured 2026-11-09 on Apple Silicon, using the same installed Qwen3-4B-Base i1 Q4_K_M model and Balanced settings. No training, model swap, threshold change, user history, private writing or screenshot data was used. The targeted fictional cases were committed before inference.

The changes preserve authored inline times/relative-time phrases, protect sender-labelled message content from status-line removal, suppress incomplete web addresses, and permit a completed short word on the current line. Existing email suppression is preserved. The evaluator now optionally shares the app's content-only eligibility gate (`--app-gates`); that is not a native foreground/AX test.

| Measure | Before | After |
| --- | ---: | ---: |
| Exact first chunk on 28 conversation fact cases | 13/28 | 21/28 |
| Meeting-time cases correct | 0/8 | 8/8 |
| Place cases correct | 8/8 | 8/8 |
| Relative-time cases correct | 0/6 | 0/6 |
| Topic-name cases correct | 5/6 | 5/6 |
| Quiet-field cases with no suggestion | 13/14 | 14/14 |
| Broad seed exact first chunk | 109/192 | 110/192 |
| Broad seed wrong first-chunk shows | 53/192 | 53/192 |
| Broad seed fully matching phrases | 44/192 | 44/192 |
| Broad seed accepted characters | 651 | 654 |

The two targeted prose/mention controls produced no suggestion in either run. The sole changed broad-seed suggestion became `was awesome`: its first chunk matches, but the full phrase does not. Do not describe this as a broad accuracy breakthrough. The relative-time cases remain incorrect even though their source facts are now preserved.

Latency increased in this single ordered pair: targeted all-case p50/p95 approximately 224/489 ms → 319/623 ms; broad seed 198/368 ms → 251/383 ms. Excluding quiet cases, targeted median was 240 → 339 ms. More preserved context and longer/more frequent outputs alter the workload; these runs do not isolate that cost from thermal or scheduling effects. These are evaluator completion times, not physical-key-to-visible-ghost measurements. No repeated-run latency claim is made.

## Reproducibility

- Frozen original targeted set: `eval/cases/context-quality-v2.jsonl`, SHA-256 `a9fb9accf408af8af126a9bce13f21b4b9667138515520e6f68a1e02fa45a70d`.
- Control source: `6ef51fd` (unchanged app input gate extracted for evaluation); original source before instrumentation: `5d7e016`.
- Model SHA-256: `ff17d19ad5f8c7ddc205d1b8c6fadc360449f06a499440dce8c81db6302f19ac`.
- Baseline evaluator SHA-256: `b4e8668711af325ae68bf074e856acfe8c211acab9676eb1d00afd4e2d1518b7`.
- Candidate evaluator SHA-256: `16559d57ed3796bb62d3eab5b474dd20bd8d0c2b5b41e61194fea6bd6a2f8460`.
- Options: base template, situation header, fictional author Taylor Chen, four-word limit, Balanced, no history, `--app-gates`.

Each version's actual Swift `TranscriptNormalizer` was applied to the same original JSONL before inference. The CLI otherwise takes supplied context as-is and would miss this app preprocessing defect. Case and binary hashes are also embedded in the JSON run metadata. The targeted set is correlated, small and purpose-built; exact matching can reject valid alternatives. It is regression evidence, not an estimate of everyday success or a comparison with another product.

Focused tests first reproduced five lost-context assertions and seven eligibility assertions, then passed after the changes. No late-AX overlay fix or generated-word dictionary enforcement is claimed. See [the engine debugging guide](../docs/ENGINE_DEBUGGING.md) for the remaining diagnostic work.
