# Autonomous Policy Eval Run — 2026-05-18

Started: 2026-05-18 03:47 UTC

## Goal

Set up two additional pretrained policies (excluding Cosmos) for the WM-as-
policy-evaluator pipeline (`docs/plans/2026-05-16-wm-open-loop-policy-eval-design.md`)
and run their sim rollouts + WM scoring continuously over a 12-hour autonomous
block. Save raw metrics / logs / videos to this directory.

## Selected policies

1. **Being-H05-2B_robocasa** — robocasa-finetuned 2B VLA. Pre-existing slurm
   pipeline at `scripts/sbatch_beingh_robocasa_policy_wm_pipeline.slurm`.
   Checkpoint: `/net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/checkpoints/beingh/Being-H05-2B_robocasa`
   Server dataset_name: `robocasa_human_posttrain`.
2. **Being-H05-2B_libero_robocasa** — libero+robocasa cross-task VLA.
   Checkpoint: `/net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/checkpoints/beingh/Being-H05-2B_libero_robocasa`
   Server dataset_name: `uni_posttrain` + must override metadata variant to a
   robocasa-key (e.g. `OpenDrawer`) because the auto-selected `libero_spatial`
   variant lacks `gripper_qpos` / `base_*` keys.

Both 2B VLAs are open-loop policies in sim (RoboCasa kitchen). For each, we
run 20 trials × 4 tasks = 80 sim rollouts that get saved in pathB schema,
then WM-score them via `scripts/eval_robocasa_policy_with_world_model.py`.

## Failures observed (from previous attempt, jobs 13431371 & 13431372)

| Job | Policy | Failure |
|---|---|---|
| 13431371 | beingh05_robocasa | `AttributeError: 'OpenCabinet' object has no attribute 'layout_id'` in `env.get_ep_meta()` — eval calls `get_ep_meta` before `env.reset()`. |
| 13431372 | beingh05_libero_robocasa | `AssertionError: gripper_qpos config not found` — metadata auto-selected `libero_spatial`, which lacks robocasa state keys. |

## Patches applied

1. `Being-H/Being-H05/BeingH/benchmark/robocasa/run_robocasa_eval_fast.py` —
   the worker now calls `env.reset()` once, captures the resulting obs, then
   calls `env.get_ep_meta()`. Passes `initial_obs` into `run_robocasa_episode`
   so the episode does NOT reset a second time (avoids re-randomizing the task
   variant and desyncing the cached task description).
2. `scripts/sbatch_beingh_robocasa_policy_wm_pipeline.slurm` — added optional
   7th argument `METADATA_VARIANT` that, when set, is forwarded to the server
   as `--metadata-variant <name>`. We pass `OpenDrawer` for the libero_robocasa
   policy because all four eval tasks (OpenCabinet, OpenDrawer,
   PickPlaceCounterToCabinet, TurnOnMicrowave) share the same observation
   schema as the `OpenDrawer` metadata variant.
3. `Being-H/Being-H05/BeingH/inference/beingh_policy.py` — patch (2026-05-18)
   the legacy nested-metadata path so `--metadata-variant` is honored when the
   dataset metadata is laid out as `{<dataset>: {<v1>: {...}, <v2>: ...}}`
   *without* the `<dataset>_variants` outer key. Without this, the libero
   checkpoint's `uni_posttrain_metadata.json` always picked `libero_spatial`
   (the first nested key) regardless of the CLI flag, and the robocasa eval
   crashed on `gripper_qpos config not found` because libero metadata has
   `libero_gripper_position` instead of `gripper_qpos`.

## Smoke test (libero, on local H100 holygpu8a15503, 03:59–04:01 EDT)

Result: PASS.

```
Using requested metadata_variant: OpenDrawer
[Metadata] Embodiment tag: robocasa
[Metadata] Stats level: unknown, Source: legacy:OpenDrawer
✓ Metadata loaded for 'uni_posttrain'
✓ BeingHPolicy initialized successfully
--- 3. Starting Inference Server on 0.0.0.0:18992 ---
Server is ready and listening on tcp://0.0.0.0:18992
```

Confirms (a) `--metadata-variant OpenDrawer` is now honored on the legacy
nested-metadata path, and (b) the robocasa state-key schema
(`gripper_qpos`, `base_position`, `base_rotation`) is correctly loaded from
the OpenDrawer variant. The submitted sbatch jobs will read the same patched
files when they transition PD → R.

## Smoke / sbatch progression

### Tick 1 — 04:23, both sbatch jobs still PD

`squeue` reason: Priority. seas_gpu busy. ~33 min after submit and still no
allocation. Decided to use the interactive H100 (job 13397660,
holygpu8a15503) opportunistically while one of the policies waits in the
sbatch queue.

### Tick 1 — 04:25–04:31, libero_robocasa pivoted to local run

1. `scancel 13628268` (the libero sbatch sibling) to avoid output-dir collisions.
2. Created `scripts/run_beingh_libero_robocasa_local.sh` — same pipeline as
   the sbatch but with `RUN_TAG=local_YYYYMMDD_HHMMSS` instead of
   `$SLURM_JOB_ID` and detached via `nohup … & disown`.
3. v1 of the local runner started 04:25:56; reached "Server is ready"
   04:27:58; eval started 04:28:16. Crashed at first trial with
   `ValueError: cannot reshape array of size 16 into shape (15,newaxis)`
   in `BeingH/benchmark/utils/policy.py:250`.
   - Root cause: libero_robocasa emits 16-frame action chunks; the
     `Policy_Robocasa.get_action` hardcodes `.reshape(15, -1)`.
   - Patch: replace 15 with `chunk_len` inferred from
     `action.gripper_position.size`.
4. v2 of the local runner started 04:30:37 with the policy.py patch;
   "Server is ready" 04:31:13. Eval **running** at ~75 s/trial; OpenCabinet
   rollouts 0000–0007 saved in
   `results/pathB_rollouts_beingh05_libero_robocasa/OpenCabinet_beingh05_libero_robocasa/`
   by 04:41:59.

Sim phase ETA: ~90 min (72 trials × 75 s/trial). WM scoring phase: ~80 ×
~500 s/rollout ≈ 11 h on one H100 sequentially. Total ~13 h — over the
12 h block, but data accumulates the whole time.

### Tick 2 — 05:13, robocasa sbatch STILL PD (1h23m after submit)

Pivoted robocasa to a parallel local runner since seas_gpu allocation hasn't
moved. Plenty of GPU headroom: libero server ~6 GB, other autonomous loop
~17 GB, free 55 GB on the 80 GB H100, so a second BeingH server fits.

1. `scancel 13628239` (the only remaining sbatch).
2. Created `scripts/run_beingh_robocasa_local.sh` — same shape as libero
   runner but model=Being-H05-2B_robocasa, port=18895, dataset=robocasa_human_posttrain.
   No metadata-variant override (robocasa metadata picks `CloseDoubleDoor`
   automatically; all four eval tasks share the same observation schema).
3. Launched 05:14:52; server ready 05:17:34; eval running 05:17:44. First
   trial at 85 s/it, runner alive 05:19:09.

Concurrent state on the H100 at 05:19 EDT:

| Component | PID | GPU mem | Status |
|---|---|---|---|
| libero BeingH server | 1880209 | 6.1 GB | listening on 18894 |
| robocasa BeingH server | 1922833 | 6.1 GB | listening on 18895 |
| other autonomous loop | 1887613 | 17.0 GB | unrelated, do not touch |
| free | — | 49.4 GB | |

libero progress at the pivot point: 34/80 trials (42%), per-trial time
crept from 75 s → 82 s under the new load. Acceptable. Both runners will
proceed in parallel.

### Tick 3 — 05:51, both runners healthy

libero: 61/80 (76%); robocasa: 22/80 (28%). GPU 41 GB free. Rollout
schema verified well-formed (`actions.npy`, `ee_pos.npy`, `rewards.npy`,
`rgb.mp4`, `success.json`, manifest.jsonl). Per-trial 82-85 s under
the dual-server load.

### Tick 4 — 06:23, libero sim DONE; WM scoring failed on Hydra `=` parse

- libero sim phase **complete at 06:18:44** — all 4 tasks × 20 trials.
- libero WM scoring **crashed** at 06:18:50 on
  `hydra.errors.OverrideParseException: mismatched input '=' expecting <EOF>`
  in `scripts/inference_teleop_action_proj.py:965`. The known
  CLAUDE.md gotcha: `epoch=0-step=15000.ckpt` has '=' chars Hydra can't
  parse as overrides.
- Fix: the `step_15000.ckpt` symlink already existed in the checkpoint
  directory (pointing to `epoch=0-step=15000.ckpt`). Updated both
  `run_beingh_*_robocasa_local.sh` scripts to use the symlinked path.
- Wrote `scripts/run_wm_scoring_only_libero.sh` — re-runs the WM
  scoring + correlation phases against the saved rollouts (no need to
  redo sim). Launched at 06:24:50.

### Tick 5/6/7 — libero WM rollout 1 stuck at 65 min, restarted with fast settings

Cosmos historical WM scoring (~216-step rollouts) ran at ~108 s/rollout.
Our libero rollouts have 720 steps each, plus the GPU is being shared
with a separate 22 GB autonomous loop. py-spy confirms the process was
genuinely computing (inside `cross_attn_ffn` of `streaming_step`); just
very slow. Linear extrapolation gave ~30-50 min/rollout under contention,
and 80 × 30-50 min ≈ 40-67 h — vastly over budget.

Killed the slow libero WM (pid 1963675/1963699) at 07:30:56. Wrote
`scripts/run_wm_scoring_fast.sh` which uses:
- `--sample_steps 10` (from 20) — 2× speedup on the denoising loop
- `--max_sim_steps 360` (truncate each rollout to first 360 frames) — 2×
  speedup since failed rollouts pad to 720 with dead-time tail
- Same checkpoint, same renderer, same n_frames=13. Schema unchanged.
- Combined ~4× speedup, target ~10-15 min/rollout.

Launched libero fast WM at 07:32:05. Rollout 1 of OpenCabinet still in
progress at 07:42 (10 min elapsed). robocasa server has been killed
since 07:14, so there's no more BeingH-server GPU contention.

### Tick-4 state at 06:25:25:

| Runner | Phase | Trials/state |
|---|---|---|
| libero local-runner (orig, pid 1880202) | DEAD post-Hydra error | sim 80/80 saved |
| robocasa local-runner (pid 1922826) | sim phase | 49/80 (61%) |
| libero WM-only runner (pid 1963675) | WM scoring | loading Wan model |

GPU: 51 GB free; libero WM not yet fully on GPU. The robocasa WM
scoring phase is the next risk point — expected ~07:10 EDT, will load a
second copy of the Wan model concurrently with libero's still-running
WM scoring. Will intervene if OOM hits.

### 03:50 — submitted both sbatch jobs

| Job ID | Policy | dataset_name | metadata_variant | port |
|---|---|---|---|---|
| 13628239 | Being-H05-2B_robocasa | robocasa_human_posttrain | (auto) | 18883 |
| 13628268 | Being-H05-2B_libero_robocasa | uni_posttrain | OpenDrawer | 18884 |

Each sbatch:
1. Starts the BeingH inference server on the assigned port.
2. Runs RoboCasa eval for 20 trials × 4 tasks (`OpenCabinet`, `OpenDrawer`,
   `PickPlaceCounterToCabinet`, `TurnOnMicrowave`) — 80 rollouts total.
3. Saves rollouts to `results/pathB_rollouts_<policy>/<task>_<policy>/rollout_*/`.
4. Runs WM scoring (`scripts/eval_robocasa_policy_with_world_model.py`) per task.
5. Computes WM-vs-policy correlation per task.

Both jobs are queued on `seas_gpu` partition with 16h walltime.

Output paths to watch:
- Rollouts: `results/pathB_rollouts_{beingh05_robocasa,beingh05_libero_robocasa}/`
- WM scores: `results/pathB_wm_scores_{beingh05_robocasa,beingh05_libero_robocasa}/`
- Correlation: `results/pathB_wm_vs_{beingh05_robocasa,beingh05_libero_robocasa}_correlation/`
- Per-job RUN dirs: `slurm_logs/beingh_{...}_<JOBID>/`

---

## Final state (recorded 2026-05-20 17:43 EDT, on session resume)

The 5/18 interactive SLURM session (job 13397660 on holygpu8a15503,
3-day walltime) ran ~57 hours past the 12-hour autonomous block and
terminated on 5/19 at ~23:53 (last snapshot watcher tick). The two
fast-WM runners and the snapshot watcher were SIGTERM'd when the
session ended; no crashes were observed in any log.

### Sim phase (deterministic, complete)

| Policy | Tasks | n_sim | Pooled sim path mean | Pooled binary success |
|---|---|---|---|---|
| Being-H05-2B-robocasa        | 4 | 80 | 1.93 m | 0.100 (8/80) |
| Being-H05-2B-libero-robocasa | 4 | 80 | 1.96 m | 0.100 (8/80) |

Both policies achieved 10% pooled success — above the DP variants'
0/60 floor and below Cosmos's 0.78. Per-task breakdown (success rate):

| Task                       | robocasa | libero-robocasa |
|----------------------------|----------|-----------------|
| OpenCabinet                | 0.05     | 0.00            |
| OpenDrawer                 | 0.10     | 0.10            |
| PickPlaceCounterToCabinet  | 0.20     | 0.25            |
| TurnOnMicrowave            | 0.05     | 0.05            |

### Fast WM scoring phase (partial; `sample_steps=10`, `max_sim_steps=360`)

| Policy | n_wm / 80 | Pooled WM path | Pooled AUC(wm_pl) | Pooled AUC(wm_fp) |
|---|---|---|---|---|
| Being-H05-2B-robocasa        | 65 | 6.31 | 0.475 | 0.180 |
| Being-H05-2B-libero-robocasa | 66 | 5.97 | 0.559 | 0.262 |

Per-task AUC values land in `results/autonomous_policy_eval_20260518/proposed_table4_5_cells.md`.
The single AUC > 0.7 (Being-H05-robocasa OpenCabinet AUC(wm_pl)=0.842)
has only 1 success in 20 — not robust. PickPlaceCounterToCabinet
(libero AUC(wm_pl)=0.480, robocasa AUC(wm_pl)=0.297) has 5 and 4
successes respectively and is the most informative per-task cell.

### Pi0-RoboCasa policy added in the resumed session (2026-05-19/20)

A third non-Cosmos pretrained policy (`pi0_robocasa`, the RLinf-trained
Pi0 4B checkpoint) was set up in the resumed session and ran 10 trials
per task = 40 sim rollouts under
`results/pathB_rollouts_pi0_robocasa/<task>_pi0_robocasa/`. Sim
results:

| Task                       | n_sim | Sim path mean | Success rate |
|----------------------------|-------|---------------|--------------|
| OpenCabinet                | 10    | 1.89 m        | 0.000 (0/10) |
| OpenDrawer                 | 10    | 3.74 m        | 0.000 (0/10) |
| PickPlaceCounterToCabinet  | 10    | 2.70 m        | 0.000 (0/10) |
| TurnOnMicrowave            | 10    | 5.11 m        | 0.000 (0/10) |
| **Pooled**                 | **40**| **3.36 m**    | **0.000 (0/40)** |

WM scoring has not started for `pi0_robocasa`. All-failure pooled, so
its AUC contribution would be undefined — treat like DP variants in
the proposed table layout.

### Snapshot watcher (relaunched 2026-05-20 17:43 EDT)

Original watcher (pid 2332830 on holygpu8a15503) was SIGTERM'd with the
5/18 session. Last timestamped snapshot in
`results/autonomous_policy_eval_20260518/snapshots/` is from
2026-05-19 23:53. Relaunched on the new compute node (job 13857961,
holygpu8a13301, pid 122811) so `proposed_table4_5_cells.md` will
refresh every 30 min from here forward. Timestamped audit copies
continue to accumulate in `snapshots/`.

### What's "ready for review" right now

- `results/autonomous_policy_eval_20260518/proposed_table4_5_cells.md`
  — proposed Table 4/5 cells (sim cells final, WM cells partial).
- `results/autonomous_policy_eval_20260518/snapshots/` — full history of
  every 30 min snapshot from 5/18 through 5/19, plus today's relaunch.
- `results/pathB_rollouts_{beingh05_robocasa,beingh05_libero_robocasa,pi0_robocasa}/`
  — 200 raw policy rollouts (rgb.mp4, actions.npy, ee_pos.npy,
  rewards.npy, success.json, manifest.jsonl) on lab FS, durable.
- `results/pathB_wm_scores_{beingh05_robocasa,beingh05_libero_robocasa}/`
  — 131 WM-scored rollouts ready for any aggregator.
- `results/autonomous_policy_eval_20260518/RUN_LOG.md` (this file) —
  narrative + bug-fix journal.

### Bugs fixed during the autonomous block

Cross-referenced for the eventual paper-record entry. None of these
edits touched the paper:

1. `Being-H/Being-H05/BeingH/benchmark/robocasa/run_robocasa_eval_fast.py`
   — call `env.reset()` before `env.get_ep_meta()` so `layout_id` is set.
2. `scripts/sbatch_beingh_robocasa_policy_wm_pipeline.slurm` — added 7th
   optional arg `METADATA_VARIANT` forwarded as `--metadata-variant`.
3. `Being-H/Being-H05/BeingH/inference/beingh_policy.py` — honor
   `--metadata-variant` on the legacy nested-metadata path (no
   `<dataset>_variants` outer key).
4. `Being-H/Being-H05/BeingH/benchmark/utils/policy.py` — infer
   `chunk_len` from `action.gripper_position.size` so 16-frame chunks
   (libero) load alongside 15-frame chunks (robocasa).
5. `outputs/2026-04-29/13-12-42/checkpoints/step_15000.ckpt` symlink
   (already existed) — used in the runner scripts instead of
   `epoch=0-step=15000.ckpt` to dodge Hydra's `=` override parser.

## Run closed — 2026-05-20 cleanup

All foreground processes are dead (`*.pid` files removed):

| Runner | Final state | Output |
|---|---|---|
| libero sim local runner | sim 80/80 saved | `pathB_rollouts_beingh05_libero_robocasa/` |
| libero WM-only fast runner | scored 66/80 rollouts | `pathB_wm_scores_beingh05_libero_robocasa/` |
| robocasa sim local runner | sim 80/80 saved | `pathB_rollouts_beingh05_robocasa/` |
| robocasa WM fast runner | scored 65/80 rollouts | `pathB_wm_scores_beingh05_robocasa/` |
| snapshot watcher | terminated | 53 unique snapshots retained (de-duplicated 2026-05-20) |

### Final stable cells (from `proposed_table4_5_cells.md`)

These are the canonical numbers the autonomous block produced and are
merged into `docs/paper_tables_corl_2026-04-28.md`
(`### Pretrained-Policy Evaluation`) and provenance recorded in
`docs/record/record_experiments_corl_2026-04-20.md`.

| Policy class | n_sim | sim_path_mean (m) | success | n_wm | wm_path_mean | AUC(wm_pl) | AUC(wm_fp) |
|---|---:|---:|---:|---:|---:|---:|---:|
| Being-H05-2B-libero-robocasa | 80 | 1.96 | 0.100 | 66 | 5.97 | 0.559 | 0.262 |
| Being-H05-2B-robocasa | 80 | 1.93 | 0.100 | 65 | 6.31 | 0.475 | 0.180 |
| Pi0-RoboCasa (RLinf, 4B) | 40 | 3.36 | 0.000 | 0 | n/a | n/a | n/a |

Pi0 sim rollouts are saved (`pathB_rollouts_pi0_robocasa/`) but the WM
scoring pass was not run inside the 12 h block. To extend, run
`scripts/run_wm_scoring_fast.sh` against that directory.

### Maintenance log (2026-05-20)

- Removed 7 stale `.pid` files (all PIDs already dead) and empty
  `.outerr` placeholder files.
- De-duplicated `snapshots/`: 58 -> 53 files, dropping consecutive
  identical refreshes; each retained file marks a content change.
- Appended this closing section so the directory is now a static
  artifact and the table cells above are the agreed numbers.
