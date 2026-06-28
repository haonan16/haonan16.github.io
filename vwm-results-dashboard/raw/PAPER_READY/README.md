# Paper-Ready Results

This folder is the curated entry point for the current paper draft. Most entries
are symlinks to original experiment outputs so provenance is preserved.

## Main Tables

- `tables/table1_*`: action representation comparison.
- `tables/table2_*`: robot-only vs robot+human heterogeneous training.
- `tables/table3_interaction_heavy`: interaction-heavy DROID holdout.
- `tables/table4_proprio_*`: proprioceptive readout evaluation.
- `tables/table8_robocasa_replay`: RoboCasa trajectory executability checks
  under the matched controller. Treat as an oracle/demo-replay data-validation
  result, not learned-policy or planning success.

## Figures and Videos

- `figures/fig_horizon_*`: streaming horizon curves and source artifacts.
- `qualitative/interaction_slide_videos`: same-scene interaction-heavy
  robot-only vs robot+human comparison videos.
- `qualitative/long_context_nf13_prelim`: preliminary nf13 long-context
  rollouts. These are slide-only until nf13/nf17 pass the standard quantitative
  evaluation.
- Long-command sampling note: `compress_repeats` is the current qualitative
  leader for demo-style repeated commands. See `qualitative/README.md` for the
  reference video and caveat.

For qualitative comparisons, only use folders with a `manifest.json` that records
the same episode, camera view, start frame, visual history, action command,
sampling protocol, and seed path.
