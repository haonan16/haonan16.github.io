# Multi-View Consistency Paper-Ready Folder

Use this as the single active multi-view presentation folder.

## Current Ready Outputs

- `merged_12step_available/videos/`: merged Camera A/B 12-step long-rollout videos without CoTracker overlay.
- `cotracker_overlay_12step_available/videos/`: merged Camera A/B 12-step videos with CoTracker trajectories for completed RPL/CLVR/RAIL sources.
- `cotracker_overlay_diverse_available/videos/`: non-3-lab CoTracker videos as diverse sources finish; currently contains AUTOLab outputs.

## Pending Outputs Will Appear Here

- `short_10labs_all/`: all-lab short paired videos after `mv10_short_comp` finishes.
- `long_10labs_pretty_12step/`: full 10-lab pretty long videos after `mv10_compose` finishes.
- `long_diverse4_pretty_12step/`: diverse 4-lab pretty long videos after `mv_div_comp` finishes.

## Notes

- Files here are real MP4/PNG files, not symlinks.
- Older duplicate multi-view folders were moved to `results/PAPER_READY/_archive/`.
- Older selected one-off videos/frames were moved into `_archive_older_selected_*` inside this folder.
- Raw generation outputs remain under `results/eval_multiview_*` because running compose jobs still depend on them.
