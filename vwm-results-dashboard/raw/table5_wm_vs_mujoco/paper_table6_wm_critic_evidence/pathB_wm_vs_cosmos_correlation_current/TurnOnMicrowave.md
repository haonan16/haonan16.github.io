# WM-on-Cosmos correlation: TurnOnMicrowave / cosmos

- **Joined rollouts:** 50
- **Success:** 38 / 50 (76.0 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| `wm_frame_progress` | 50 | -0.320 | 0.0235 | -0.308 | 0.0294 | 0.292 | 0.708 | +0.148 | +0.172 |
| `wm_proprio_path_length` | 50 | +0.131 | 0.3653 | +0.107 | 0.4592 | 0.572 | 0.428 | +0.459 | +0.439 |
| `wm_action_iou` | 50 | +nan | nan | +nan | nan | 0.000 | 0.000 | +0.000 | +0.000 |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
