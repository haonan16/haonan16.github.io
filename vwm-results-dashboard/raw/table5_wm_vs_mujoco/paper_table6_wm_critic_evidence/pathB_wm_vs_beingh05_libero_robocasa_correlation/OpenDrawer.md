# WM-on-Cosmos correlation: OpenDrawer / beingh05_libero_robocasa

- **Joined rollouts:** 20
- **Success:** 2 / 20 (10.0 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| `wm_frame_progress` | 20 | -0.290 | 0.2157 | -0.231 | 0.3267 | 0.278 | 0.722 | +0.614 | +0.664 |
| `wm_proprio_path_length` | 20 | -0.219 | 0.3532 | -0.260 | 0.2680 | 0.250 | 0.750 | +6.134 | +7.193 |
| `wm_action_iou` | 20 | -0.095 | 0.6916 | -0.111 | 0.6414 | 0.000 | 0.111 | +0.000 | +0.000 |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
