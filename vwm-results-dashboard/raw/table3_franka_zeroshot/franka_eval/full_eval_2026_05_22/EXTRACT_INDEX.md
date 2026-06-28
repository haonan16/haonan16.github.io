# Where to extract MP4s — single entry point

All Franka paper-figure videos accessible from this directory via symlinks.
Updated 2026-05-22 20:30 after the text-overlay regression was fixed:
- `--burn_action_text` was removed from `scripts/sbatch_franka_dataset_action_rollouts_12h.sh`
- The text-burned content under the old `_20260522_114410/ours_*/` subdirs was deleted
- The originally-named `_clean_20260522_200952/` is now the canonical clean ours dataset-action dir
  (resume in flight; 10× `fda_*` jobs writing under it)
- The old top-level dir `_20260522_114410/` is now used only by `fdr_*` (raw-vector baseline replays)
  via `make_six_direction_rollouts.py`, which is annotation-free.

## Categories

### 1. xyz 6-direction comparison — interaction-heavy episodes

Source: `all_methods_franka_interaction_6ep_20260522_123937/` (12 sibling model
folders — merged 2026-05-23 from the former internal/externals split for
easy side-by-side comparison).

Access via `xyz_interaction_all_methods/` (symlink → that source dir):

**Ours** (action-projection, no finetune):
  - `ours_A_nf17_step12000/`              ← A nf17 (DROID-only)
  - `ours_3way_nf17_step18750/`           ← 3-way nf17 (DROID + SSv2 + RoboCasa)
  - `ours_A25750_robot_only/`             ← A nf17 robot-only

**Ours variants** (action-projection, franka-finetuned):
  - `ours_A_nf17_franka_finetuned/`       ← franka-finetuned (6-way)
  - `ours_3way_nf17_franka_finetuned/`
  - `ours_A_nf17_franka10_finetuned/`     ← franka-finetuned (10-way mixture)
  - `ours_3way_nf17_franka10_finetuned/`

**Our baselines** (raw-vector action injection — same backbone, no projection):
  - `baseline_raw_action_adaln_step10000/`           ← AdaLN raw-vector
  - `baseline_raw_action_cross_attention_step26000/` ← cross-attention raw-vector

**External baselines**:
  - `ctrl_world/`
  - `worldgym/`
  - `worldgym_droid_finetune/`

Per-model structure: `<label>/{ext1,ext2}/individual_videos/<ep>_*.mp4`
Plus `direction_grid_<ep>__*.mp4` (6-direction concat per ep).

Episodes (6 interaction-heavy, all with start_frame=80 in active manipulation):
- box: episode_20260516_154452, episode_20260516_154216
- mustard: episode_20260516_001826, episode_20260516_010042
- rope: episode_20260516_013812, episode_20260517_220323

### 2. dataset_action replays — model replays recorded EE trajectory

- **Ours (clean)**: `dataset_action_rollouts_ours_clean/` — 5 ours × 2 modes (`dataset_raw_xyz`, `dataset_ee_7dof`), no text
- **Raw-vector (AdaLN)**: `dataset_replay_adaln_raw_vector/dataset_replay/ext1/` — replay.mp4 + gt.mp4 per ep
- **Raw-vector (Cross-Attn)**: `dataset_replay_cross_attn_raw_vector/dataset_replay/ext1/` — same structure
- **Externals**: their `replay.mp4` + `gt.mp4` under `xyz_interaction_all_methods/{ctrl_world,worldgym,worldgym_droid_finetune}/.../<ep>/` ARE the dataset replay

### 3. Retired / old (do not use)

(none — the `dataset_action_rollouts_texted_OLD` symlink was removed 2026-05-22 20:30
after the underlying text-burned ours/ output was deleted. The raw-vector baseline
subdirs still exist at `_20260522_114410/baseline_raw_action_*/dataset_replay/`
because the annotation-free `fdr_*` jobs write there — those are exposed via
`dataset_replay_adaln_raw_vector/` and `dataset_replay_cross_attn_raw_vector/`.)

## Quick start

```bash
ROOT=/net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/video_world_model/results/franka_eval/full_eval_2026_05_22

# Count everything (follow symlinks with -L)
find -L "$ROOT" -name "*.mp4" | wc -l

# A representative comparison: 3way nf=17 vs Cross-Attn vs WorldGym, ep 154452
ls "$ROOT/xyz_interaction_all_methods/ours_3way_nf17_step18750/ext1/individual_videos/" | head -3
ls "$ROOT/xyz_interaction_all_methods/baseline_raw_action_cross_attention_step26000/ext1/individual_videos/" | head -3
ls "$ROOT/xyz_interaction_all_methods/worldgym/ext1/" | head -3
```

## Protocol (shared across all of the above)

- Horizon: 20 streaming chunks → 80 generated frames per direction
- Sample steps: 40 (Euler flow-matching)
- Magnitude: 0.03 m / chunk (for nudge directions)
- Resolution: 480x832, FPS=8
- Video quality: 10 (imageio scale)
- Render: v2 (action-projection v2 for action_proj models)
- Real history bootstrap (in-distribution for streaming models)
