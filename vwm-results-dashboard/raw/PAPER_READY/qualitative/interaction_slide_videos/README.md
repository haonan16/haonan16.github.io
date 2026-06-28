# Interaction-Heavy Slide Videos

These videos are generated from:

- `results/eval_interaction_10ep/A_step25750/videos/`
- `results/eval_interaction_10ep/CP_step30000/videos/`

Regenerated on 2026-05-03 with `scripts/make_comparison_videos.py` for all 10
interaction-heavy episodes.

Comparison validity rule:

- Only use side-by-side comparisons when both sides share the same DROID episode,
  camera view, start frame, visual history, action command, sampling protocol, and
  seed.
- These interaction-heavy videos are paired by identical `{episode}_{action}.mp4`
  filenames from the two matched eval directories above.

Rows/labels:

- `Robot-only`: DROID-only model.
- `Robot+human`: DROID+SSv2 model with proprio readout.

Recommended slide picks:

- `direction_grid_RAD.mp4`: strongest before/after interaction-heavy example.
- `direction_grid_RAIL.mp4`: good near-static vs improved control example.
- `sbs_RAD_+x.mp4` and `sbs_RAD_-x.mp4`: compact side-by-side clips.
- `direction_strip_CP_RAD.mp4`: robot+human only, showing +x / zero / -x response.
- `summary_multi_episode_A_vs_CP.mp4`: multi-episode overview, best for backup slides.

Avoid using ILIAD x-axis clips for claims; those trials are excluded from DirAcc because
the projected image-space direction is zero.
