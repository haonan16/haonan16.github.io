# WM-on-Cosmos correlation: OpenDrawer / beingh05_robocasa

- **Joined rollouts:** 20
- **Success:** 2 / 20 (10.0 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| `wm_frame_progress` | 20 | -0.485 | 0.0301 | -0.462 | 0.0401 | 0.056 | 0.944 | +0.585 | +0.659 |
| `wm_proprio_path_length` | 20 | -0.243 | 0.3014 | -0.202 | 0.3923 | 0.306 | 0.694 | +6.920 | +7.760 |
| `wm_action_iou` | 20 | -0.076 | 0.7486 | -0.076 | 0.7486 | 0.000 | 0.083 | +0.000 | +0.000 |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
