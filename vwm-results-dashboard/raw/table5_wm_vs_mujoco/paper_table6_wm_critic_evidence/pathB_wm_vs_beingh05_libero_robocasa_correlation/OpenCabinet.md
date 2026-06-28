# WM-on-Cosmos correlation: OpenCabinet / beingh05_libero_robocasa

- **Joined rollouts:** 20
- **Success:** 0 / 20 (0.0 %)

## Correlations (one row per WM score)

| WM score | n | Pearson r | (p) | Spearman ρ | (p) | AUROC (higher=success) | AUROC (lower=success) | mean success | mean failure |
|---|---|---|---|---|---|---|---|---|---|
| wm_frame_progress | 20 | — | — | — | — | — | — | — | no class variation; correlation undefined |
| wm_proprio_path_length | 20 | — | — | — | — | — | — | — | no class variation; correlation undefined |
| wm_action_iou | 20 | — | — | — | — | — | — | — | no class variation; correlation undefined |

AUROC > 0.5 means the score is informative for predicting success. We report both polarities because WM scores' sign convention varies (e.g. ``proprio_path_length`` higher when the EE moves a lot, which often correlates with successful task execution; while ``frame_progress`` captures motion through the scene).
