# Paper Qualitative Artifacts

## Safe Main Picks

- `interaction_slide_videos/direction_grid_RAD.mp4`
- `interaction_slide_videos/direction_grid_RAIL.mp4`
- `interaction_slide_videos/sbs_RAD_+x.mp4`
- `interaction_slide_videos/sbs_RAD_-x.mp4`
- `interaction_slide_videos/direction_strip_CP_RAD.mp4`
- `interaction_slide_videos/summary_multi_episode_A_vs_CP.mp4`

These compare robot-only and robot+human models on the same scene/action pairs.
See `interaction_slide_videos/manifest.json` for provenance.

## Demo Sampling Notes

- `compress_repeats` is the current qualitative leader for repeated
  long-command demos. Reference video:
  `../../long_rollout_modes/20260511_nf5_auto_vs_exact_20ep_rerun/videos/00_RPL_compress_repeats_minusx_5_scale1.00.mp4`.
  This is a qualitative observation, not a paper metric, until aggregate
  `summary.json` / `trial_records.json` results are available.

## Use With Caution

- `advisor_comparison_videos`: older presentation candidates. Use only after
  verifying or regenerating same-scene provenance.
- `long_context_nf13_prelim`: preliminary nf13 long-context videos submitted via
  SLURM job 9749406. Use only for slides until standard nf13/nf17 DirAcc + LPIPS
  evaluation is complete.
