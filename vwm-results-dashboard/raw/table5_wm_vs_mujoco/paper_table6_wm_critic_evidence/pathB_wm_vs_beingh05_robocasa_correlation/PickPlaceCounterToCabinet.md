# WM-on-Cosmos correlation: PickPlaceCounterToCabinet / beingh05_robocasa

- **Joined rollouts:** 20
- **Success:** 4 / 20 (20.0 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| `wm_frame_progress` | 20 | -0.439 | 0.0528 | -0.499 | 0.0252 | 0.141 | 0.859 | +0.533 | +0.594 |
| `wm_proprio_path_length` | 20 | -0.167 | 0.4821 | -0.282 | 0.2287 | 0.297 | 0.703 | +6.664 | +7.378 |
| `wm_action_iou` | 20 | +0.571 | 0.0086 | +0.434 | 0.0558 | 0.500 | 0.094 | +0.003 | +0.000 |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
