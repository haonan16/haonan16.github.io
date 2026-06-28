# Franka Evaluation Results — Index

**Reorganized:** 2026-05-22 (consolidated 19 scattered top-level `results/franka_*`
/ `paper_franka_*` / `paper_xyz_franka_*` / `baselines_external_franka*` /
`smoke_replay_*` dirs into one hierarchy).

## Tree

```
results/franka_eval/
├── INDEX.md                                          ← you are here
│
├── box_zero_shot/                                    Stanford box study (zero-shot)
│   ├── 4ep/                                          4-episode initial pass
│   └── 18ep/                                         18-episode scaleup
│
├── sim2real_2026_05_20/                              10-task sim-to-real run
│   ├── zero_shot/                                    No fine-tune; A_nf17 vs 3way_nf17
│   ├── finetune_6way/                                +2k FT on franka_recordings_action_proj (6 directions)
│   ├── horizon/                                      Failure-divergence vs horizon length
│   ├── multiview/                                    Per-task ext2-only multi-view
│   └── stitching/                                    Paper qualitative stitching MP4s
│
├── sim2real_2026_05_21_10way_ft/                     10-way mixture FT re-evals @ step_2000
│
├── xyz_six_direction_2026_05_21/                     xyz 6-direction round 1
│                                                     (ours + raw-vector + external baselines)
│
├── full_eval_2026_05_22/                             Canonical 2026-05-22 full-eval suite
│   ├── external_baselines/                           Ctrl-World + WorldGym × 10 task variants × ext1+ext2
│   ├── interaction_round2 → ../../paper_xyz_franka_interaction_6ep_20260522_115250/   (SYMLINK)
│   ├── xyz_interaction_all_methods → ../../all_methods_franka_interaction_6ep_20260522_123937/   (SYMLINK)
│   ├── xyz_interaction_round3_dir/                   Kempner_h100 fi6_* (2026-05-22 ~12:39 EDT)
│   │   └── all_methods → ../../../all_methods_franka_interaction_6ep_20260522_123937/   (SYMLINK)
│   └── dataset_action_rollouts → ../../franka_dataset_action_rollouts_20260522_114410/  (SYMLINK; IN-FLIGHT)
│   (NOTE 2026-05-23: paper_franka_interaction_6ep_20260522_123937/ (9 ours + raw-vector
│    baselines) and baselines_external_franka_interaction_6ep_20260522_123937/ (3
│    externals) were merged into a single flat top-level dir
│    `results/all_methods_franka_interaction_6ep_20260522_123937/` with 12 sibling model
│    folders, for easy side-by-side comparison. The four old symlinks
│    (xyz_interaction_internal, xyz_interaction_externals,
│    interaction_round3/{ours_and_raw_vector, external_baselines}) were replaced by the
│    two `xyz_interaction_all_methods` / `xyz_interaction_round3_dir/all_methods`
│    symlinks above. The earlier `ff_*` batch under paper_franka_full_20260522_042400/
│    was CANCELLED 2026-05-22 ~13:10 — its (model, view) lineup is in this same
│    merged dir.)
│
└── _archive/
    ├── failed_launches_2026-05-22/                   18 jobs died at startup (bash 4.4 set -u bug)
    │   ├── paper_franka_full_20260522_040739/        (manifest JSON only)
    │   └── paper_franka_full_20260522_041416/        (manifest JSON only)
    ├── empty_stubs/                                  README-only dirs from duplicate launches
    │   ├── paper_xyz_franka_multiview_six_direction_20260521_215323/
    │   ├── paper_xyz_franka_multiview_six_direction_20260521_220300/
    │   └── paper_xyz_franka_interaction_6ep_20260522_115231/
    ├── smoke/
    │   └── smoke_replay_20260522_041950/             1-ep × 5-step replay validation
    └── superseded/
        └── baselines_external_franka_round1/         3-ep ext1-only; superseded by full_eval_2026_05_22/external_baselines/
            (paper_franka_interaction_6ep_20260522_123453 grew to 4.4G under jobs 14554005/11/12;
             both the dir and its 3 producer jobs were removed 2026-05-22 ~13:10 as duplicates of _123937.)
```

## What lives where

| Experiment family | Path | Handoff doc | Status |
|---|---|---|---|
| Stanford box (4ep) | `box_zero_shot/4ep/` | `docs/record/franka_box_handoff_2026-05-19.md` | COMPLETE |
| Stanford box (18ep) | `box_zero_shot/18ep/` | `docs/record/franka_box_handoff_2026-05-19.md` | COMPLETE |
| sim2real zero-shot | `sim2real_2026_05_20/zero_shot/` | `docs/record/franka_sim2real_run_2026-05-20.md` | COMPLETE |
| sim2real 6-way FT | `sim2real_2026_05_20/finetune_6way/` | `docs/record/franka_sim2real_run_2026-05-20.md` | COMPLETE |
| sim2real horizon | `sim2real_2026_05_20/horizon/` | `docs/record/franka_sim2real_run_2026-05-20.md` | COMPLETE |
| sim2real multiview | `sim2real_2026_05_20/multiview/` | `docs/record/franka_sim2real_run_2026-05-20.md` | COMPLETE |
| sim2real stitching | `sim2real_2026_05_20/stitching/` | `docs/record/franka_sim2real_run_2026-05-20.md` | COMPLETE |
| 10-way mixture FT @ step_2000 | `sim2real_2026_05_21_10way_ft/` | (within the full-eval handoff) | COMPLETE |
| xyz 6-direction round 1 | `xyz_six_direction_2026_05_21/` | `docs/record/franka_xyz_six_direction_handoff_2026-05-21.md` | COMPLETE |
| Full-eval ours + raw-vector | (merged into `xyz_interaction_all_methods/`) | `docs/record/franka_full_eval_handoff_2026-05-22.md` | original `ff_*` batch CANCELLED 11:47 |
| Full-eval external baselines | `full_eval_2026_05_22/external_baselines/` | `docs/record/franka_full_eval_handoff_2026-05-22.md` | COMPLETE (ext2 10/10) |
| xyz interaction round 2 | `full_eval_2026_05_22/interaction_round2/` (symlink) | `docs/record/franka_xyz_six_direction_handoff_2026-05-21.md` | (seas_gpu jobs CANCELLED; see round3) |
| Interaction round 3 all methods (ours + variants + raw-vector + externals) | `full_eval_2026_05_22/xyz_interaction_all_methods/` (symlink → `results/all_methods_franka_interaction_6ep_20260522_123937/`) | `docs/record/franka_full_eval_handoff_2026-05-22.md` | COMPLETE 2026-05-23 — 12 sibling model folders |
| Dataset-action rollouts | `full_eval_2026_05_22/dataset_action_rollouts/` (symlink) | `docs/record/franka_full_eval_handoff_2026-05-22.md` | **IN-FLIGHT** (10× `fda_*` RUNNING on seas_gpu) |

## In-flight rollover plan

One real dir still has a live writer at `results/<original_name>/` so that
running SLURM jobs still hit the `OUT_ROOT` they captured at submit time:

- `results/franka_dataset_action_rollouts_20260522_114410/` ← 8× `fda_*` RUNNING on seas_gpu

The two interaction_6ep dirs FINISHED 2026-05-23 ~02:38 EDT and were merged
into a single flat top-level dir for comparison (see "(NOTE 2026-05-23: ...)"
in the Tree section):

- `results/all_methods_franka_interaction_6ep_20260522_123937/` — 12 sibling
  model folders: 3 ours (`ours_3way_nf17_step18750`, `ours_A_nf17_step12000`,
  `ours_A25750_robot_only`) + 4 ours-finetuned variants
  (`ours_{3way,A}_nf17_franka{,10}_finetuned`) + 2 raw-vector baselines
  (`baseline_raw_action_{adaln,cross_attention}_*`) + 3 external baselines
  (`ctrl_world`, `worldgym`, `worldgym_droid_finetune`).

Also kept at top level (no active writers; previously symlinked, kept for the existing
symlink target stability):

- `results/paper_xyz_franka_interaction_6ep_20260522_115250/` (xyz6i_* family was cancelled at 11:52)

Cancelled duplicate submissions (2026-05-22 ~13:10):

- 14554005, 14554011, 14554012 — Wave A `fi6_*` (FX_TS=20260522_123453) covering 3
  10-way-FT (model, view) tuples that were duplicated by Wave B (FX_TS=20260522_123937).
  Partial 4.4 GB output deleted.
- 14478528, 14478529, 14478569 — 4-GPU + 1-GPU backup ft_franka10 trainers + watcher;
  the canonical step_2000.ckpt was already produced by 14483582 (COMPLETED 11:35:57).

Once the remaining `fda_*` batch finishes, run the rollover sweep to fold the
dataset_action_rollouts dir into the new hierarchy and drop its symlink:

```bash
ROOT=/net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/video_world_model/results
cd "$ROOT/franka_eval/full_eval_2026_05_22"

# Replace the remaining top-level symlinks with their real dirs
for name in interaction_round2 dataset_action_rollouts; do
  target=$(readlink "$name")
  rm "$name"
  mv "$target" "$name"
done
```

The `xyz_interaction_all_methods` symlink intentionally stays as a symlink to
the top-level `all_methods_*` dir so that any future re-rendering can target a
stable canonical path. After this final rollover, `INDEX.md` and the handoff
docs already reference the new paths — no further edits needed.

## How to find a specific eval

| You're looking for... | Path |
|---|---|
| The paper-target full-eval (ours + raw-vector + externals + interaction + dataset replay) | `full_eval_2026_05_22/` |
| Stanford box zero-shot only (the original ad-hoc study) | `box_zero_shot/` |
| The 2026-05-20 sim2real campaign (5 families) | `sim2real_2026_05_20/` |
| xyz 6-direction first pass (May 21, round 1) | `xyz_six_direction_2026_05_21/` |
| Anything from a failed launch / empty stub / smoke run | `_archive/` |

## Caveat: launcher scripts still hardcode the OLD top-level paths

Doc references were updated as part of this reorg, but **launcher / SLURM scripts
under `scripts/` still write to `results/franka_*` etc. at the top level**. This
is intentional and harmless:

- The 45 PENDING jobs in queue captured their `OUT_ROOT` at submit time — they
  will write to the 3 symlinked in-flight dirs (see "In-flight rollover plan"
  above).
- If you re-run a completed launcher (`scripts/franka_sim2real/launch_*.sh`,
  `scripts/franka_box/sbatch_eval_*.sh`, `scripts/sbatch_paper_xyz_franka_*.sh`),
  it will create a NEW dir at `results/<old_name>/`. That's a regression; in
  that case manually `mv` the new output into `results/franka_eval/<family>/`
  afterward, or edit the launcher's `OUT_ROOT` line first.

Scripts containing hardcoded old paths (grep for `results/franka_sim2real` etc.):
- `scripts/franka_sim2real/launch_*.sh` (zero_shot, finetune_6way, horizon, etc.)
- `scripts/franka_sim2real/render_*.sh`, `aggregate.py`, `make_qualitative_*.py`
- `scripts/franka_box/sbatch_eval_franka_box_zero_shot*.sh`
- `scripts/franka_box/make_paper_figure.py`
- `scripts/sbatch_paper_xyz_franka_ext{1,2}_*.sh`, `_round2.sh`,
  `_fillgap.sh`
- `scripts/launch_franka_full_eval_12h.sh`
- `scripts/external_baselines/run_external_baselines_6dir.sh`
- `scripts/sbatch_franka_external_baselines_per_variant.slurm`

Defer these edits until after the in-flight 45 jobs finish to avoid breaking
their `OUT_ROOT` capture, then do a batch script update.

### 2026-05-27 status

Empty `franka_sim2real_*` top-level dirs got deleted twice on 2026-05-27
(once during the broader `results/` cleanup pass, then again after launchers
silently recreated them). They keep coming back because of these defaults:

| Launcher | Hardcoded default | Behavior on bare invocation |
|---|---|---|
| `scripts/launch_franka_eval.sh` lines 285, 294 | `results/franka_sim2real_<DATE>_<SUBCMD>` / `..._horizon_<DATE>` | Date-dynamic: creates `franka_sim2real_2026_05_DD_zeroshot/` and `..._horizon_2026_05_DD/` each calendar day. |
| `scripts/franka_sim2real/launch_new_tasks_zeroshot.sh` | `results/franka_sim2real_2026_05_20` | Hardcoded May 20 path. |
| `scripts/franka_sim2real/launch_6way_ft_on_new.sh` | `results/franka_sim2real_finetune_2026_05_20` | Hardcoded May 20 path. |
| `scripts/franka_sim2real/launch_horizon_all.sh` | `results/franka_sim2real_horizon_2026_05_21` | Hardcoded May 21 path. |
| `scripts/franka_sim2real/launch_finetune_reeval.sh` | `results/franka_sim2real_finetune_2026_05_21` | Hardcoded May 21 path. |
| `scripts/franka_sim2real/launch_finetune_extend_reeval.sh` | `results/franka_sim2real_finetune_extend_2026_05_22` | Hardcoded May 22 path. |
| `scripts/franka_sim2real/watch_ft10_and_reeval.sh` | `results/franka_sim2real_finetune_2026_05_21_10way` | Hardcoded May 21 path. |

**Patch plan (after in-flight jobs drain):**

1. Change `launch_franka_eval.sh` lines 285, 294 to default under
   `results/franka_eval/sim2real_<DATE>/<SUBCMD>/` so future date-dynamic
   runs don't pollute top level.
2. For the 6 hardcoded launchers, either update each `OUT_BASE` default
   to point under `results/franka_eval/sim2real_<DATE>/<phase>/`, or add
   a guard `[[ -d "$ROOT/results/franka_eval/sim2real_<DATE>" ]] && OUT_BASE=...`
   so the launcher prefers the consolidated dir when it exists.
3. Update the 5 readers (`aggregate.py`, `make_qualitative_*.py`,
   `render_paper_tables.py`, `render_all.sh`) to match.

Until that patch lands, any fresh launcher invocation will re-create an
empty top-level skeleton dir. Safe to `rmdir` whenever they accumulate.

## Reorganization audit

Run anytime to spot drift between reality and this index:

```bash
ROOT=/net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/video_world_model/results

# All non-archive franka result dirs that are NOT under franka_eval/
ls "$ROOT" | grep -E '(franka|paper_franka|paper_xyz_franka|baselines_external_franka|smoke_replay)' \
  | grep -v '^franka_eval' \
  | grep -v -E '^(paper_franka_full_20260522_042400|paper_xyz_franka_interaction_6ep_20260522_115250|franka_dataset_action_rollouts_20260522_114410)$'
# Expected: nothing.
```
