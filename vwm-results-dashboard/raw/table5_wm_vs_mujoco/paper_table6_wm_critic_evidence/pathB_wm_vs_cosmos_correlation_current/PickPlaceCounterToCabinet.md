# WM-on-Cosmos correlation: PickPlaceCounterToCabinet / cosmos

- **Joined rollouts:** 50
- **Success:** 32 / 50 (64.0 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| `wm_frame_progress` | 50 | +0.196 | 0.1735 | +0.196 | 0.1718 | 0.618 | 0.382 | +0.223 | +0.209 |
| `wm_proprio_path_length` | 50 | +0.116 | 0.4230 | +0.069 | 0.6325 | 0.542 | 0.458 | +0.763 | +0.733 |
| `wm_action_iou` | 50 | +nan | nan | +nan | nan | 0.000 | 0.000 | +0.000 | +0.000 |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
