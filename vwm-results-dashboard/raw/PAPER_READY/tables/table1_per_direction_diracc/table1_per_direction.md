# Table 1 · Per-direction DirAcc breakdown

Strict 72-trial denominator (18 DROID episodes × 4 commanded directions ±x, ±y, displacement threshold = 1 px in commanded image direction). Trials where the model is static (<1 px detectable motion in any direction) count as failures, not exclusions.

## Passes per direction

| Method | +x | −x | +y | −y | Total (72) | DirAcc |
|---|---:|---:|---:|---:|---:|---:|
| Ours (A+P@20K) | 18/18 | 17/18 | 15/18 | 16/18 | **66/72** | **0.917** |
| Ctrl-World | 1/18 | 2/18 | 0/18 | 1/18 | **4/72** | **0.056** |
| WorldGym (zero-shot) | 5/18 | 4/18 | 5/18 | 0/18 | **14/72** | **0.194** |
| WorldGym (DROID-finetune) | 15/18 | 5/18 | 2/18 | 10/18 | **32/72** | **0.444** |

## Testable per direction (showed any visible motion)

| Method | +x | −x | +y | −y | Total testable (72) |
|---|---:|---:|---:|---:|---:|
| Ours (A+P@20K) | 18/18 | 18/18 | 18/18 | 18/18 | **72/72** |
| Ctrl-World | 4/18 | 5/18 | 2/18 | 3/18 | **14/72** |
| WorldGym (zero-shot) | 6/18 | 8/18 | 11/18 | 11/18 | **36/72** |
| WorldGym (DROID-finetune) | 18/18 | 16/18 | 18/18 | 18/18 | **70/72** |

## Conditional DirAcc (passed / testable, per direction)

| Method | +x | −x | +y | −y |
|---|---:|---:|---:|---:|
| Ours (A+P@20K) | 18/18=1.00 | 17/18=0.94 | 15/18=0.83 | 16/18=0.89 |
| Ctrl-World | 1/4=0.25 | 2/5=0.40 | 0/2=0.00 | 1/3=0.33 |
| WorldGym (zero-shot) | 5/6=0.83 | 4/8=0.50 | 5/11=0.45 | 0/11=0.00 |
| WorldGym (DROID-finetune) | 15/18=0.83 | 5/16=0.31 | 2/18=0.11 | 10/18=0.56 |

## Reading

- **Strict DirAcc** (column "Total (72)") is what Table 1 of the paper
  reports. It penalises both wrong-direction failures and
  zero-motion ("static") failures equally.
- **Testable** isolates how often the row produced any visible
  motion at all.
- **Conditional DirAcc** = passed / testable per direction —
  isolates pure direction selectivity when the model does move.

## Provenance

| Row | Source |
|---|---|
| Ours | `results/ours_per_direction_18ep/summary.json` (eval_action_metrics.py +per_direction) |
| Ctrl-World | `results/baselines_external/ctrl_world/summary.json` |
| WorldGym (zero-shot) | `results/baselines_external/worldgym/summary.json` |
| WorldGym (DROID-finetune) | `results/baselines_external/worldgym_droid_finetune/summary.json` |
