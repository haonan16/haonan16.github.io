# WM-on-Cosmos correlation: OpenDrawer / cosmos

- **Joined rollouts:** 39
- **Success:** 37 / 39 (94.9 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| `wm_frame_progress` | 39 | +0.362 | 0.0234 | +0.279 | 0.0856 | 0.865 | 0.135 | +0.450 | +0.329 |
| `wm_proprio_path_length` | 39 | +0.619 | 0.0000 | +0.114 | 0.4910 | 0.649 | 0.351 | +3.113 | +2.159 |
| `wm_action_iou` | 39 | +0.038 | 0.8197 | +0.038 | 0.8197 | 0.027 | 0.000 | +0.000 | +0.000 |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
