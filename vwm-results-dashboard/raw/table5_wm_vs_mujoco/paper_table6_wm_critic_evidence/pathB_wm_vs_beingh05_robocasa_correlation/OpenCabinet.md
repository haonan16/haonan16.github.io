# WM-on-Cosmos correlation: OpenCabinet / beingh05_robocasa

- **Joined rollouts:** 20
- **Success:** 1 / 20 (5.0 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| `wm_frame_progress` | 20 | +0.008 | 0.9732 | +0.020 | 0.9337 | 0.526 | 0.474 | +0.624 | +0.622 |
| `wm_proprio_path_length` | 20 | +0.168 | 0.4792 | +0.259 | 0.2709 | 0.842 | 0.158 | +5.211 | +4.088 |
| `wm_action_iou` | 20 | -0.061 | 0.7985 | -0.076 | 0.7490 | 0.000 | 0.158 | +0.000 | +0.000 |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
