# WM-on-Cosmos correlation: PickPlaceCounterToCabinet / beingh05_libero_robocasa

- **Joined rollouts:** 20
- **Success:** 5 / 20 (25.0 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| `wm_frame_progress` | 20 | -0.370 | 0.1084 | -0.451 | 0.0462 | 0.200 | 0.800 | +0.538 | +0.590 |
| `wm_proprio_path_length` | 20 | -0.118 | 0.6217 | -0.030 | 0.9000 | 0.480 | 0.520 | +6.301 | +6.938 |
| `wm_action_iou` | 20 | -0.073 | 0.7594 | -0.014 | 0.9522 | 0.173 | 0.320 | +0.000 | +0.001 |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
