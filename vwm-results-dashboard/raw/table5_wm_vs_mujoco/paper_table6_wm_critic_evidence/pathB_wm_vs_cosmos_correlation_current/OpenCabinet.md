# WM-on-Cosmos correlation: OpenCabinet / cosmos

- **Joined rollouts:** 48
- **Success:** 32 / 48 (66.7 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| `wm_frame_progress` | 48 | -0.218 | 0.1371 | -0.204 | 0.1639 | 0.375 | 0.625 | +0.254 | +0.303 |
| `wm_proprio_path_length` | 48 | -0.343 | 0.0168 | -0.370 | 0.0096 | 0.273 | 0.727 | +1.149 | +1.703 |
| `wm_action_iou` | 48 | +0.103 | 0.4854 | +0.103 | 0.4854 | 0.031 | 0.000 | +0.000 | +0.000 |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
