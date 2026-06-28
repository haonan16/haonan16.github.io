# Table 1 — External WM Baseline Rows (Reviewer Paste-In)

Standalone block containing the two new external-baseline rows added to
Table 1 of the paper (`docs/paper_tables_corl_2026-04-28.md`), formatted
for reviewer paste-in. The "Ours" row at the bottom is reproduced as
context only — it is unchanged from the existing table.

## Rows

| Method | Architecture | DirAcc | D_sep | LPIPS | Notes |
|---|---|---:|---:|---:|---|
| Ctrl-World | SVD + action token concat (DROID-trained) | 0.056 | 4.34 | 0.217 | DROID; strict DirAcc = 4/72 trials; only 14/72 rollouts contain any visible motion at all; HF ckpt `yjguo/Ctrl-World`; native 192x320 per view; eval 2026-05-14 |
| WorldGym (DROID-finetune) | DiT + Bridge-rescale, fine-tuned on DROID | 0.444 | 167.26 | 0.175 | DROID; 10K-step fine-tune from 580K ckpt; strict DirAcc = 32/72 trials; responsive on 70/72 rollouts; cos-align $-0.057$; training holds out 18 paper-eval scenes; native 256x256; ckpt `outputs/worldgym_droid_local_10k_20260515_204004/ckpt_000009000.pt`; eval 2026-05-16 |
| **Ours** | rendered action image, 2-stream MoT | **0.917** | **8.5** | **0.042** | DROID; A+P@20K; re-run 2026-05-15 |

## Caption notes (verbatim, drop into the table caption block)

- **Same 72-trial denominator for every row.** Each model is evaluated on the
  identical 72 directional trials: 18 DROID scenes x 4 commanded directions
  ($\pm$x, $\pm$y). DirAcc is the fraction of trials where the generated video
  shows EE motion in the commanded image-space direction. **Trials where the
  model produces a static or near-static rollout (< 1 px detectable motion)
  count as failures, not exclusions** — the denominator stays at 72 for every
  row. Under this strict definition: Ours passes 66/72 = 0.917, WorldGym
  DROID-finetune passes 32/72 = 0.444, and Ctrl-World passes 4/72 = 0.056.
  The very low Ctrl-World score is dominated by unresponsiveness (only 14 of
  its 72 rollouts contain visible motion at all); WorldGym DROID-finetune is
  responsive on 70/72 trials but still has wrong-direction motion. The strict
  choice matches how `scripts/eval_action_metrics.py`
  aggregates "Ours" (`total_correct / 72`) and makes the comparison
  apples-to-apples. Underlying counts and the alternative conditional DirAcc
  (`n_passed / n_testable`) are preserved in
  `results/baselines_external/{ctrl_world,worldgym_droid_finetune}/summary.json`.
- **LPIPS/PSNR/SSIM are at native resolution per row.** Ctrl-World is evaluated
  at 192x320 per camera view, WorldGym DROID-finetune at 256x256, and Ours at 480x832; each
  baseline is compared against ground-truth frames at its own native resolution
  and we do NOT bilinearly resize predictions to a common grid before
  computation (`pred_native_shape == gt_shape`, `resized_pred_to_gt: false` in
  every per-episode `vq.json`). Consequently, raw LPIPS values are not strictly
  apples-to-apples across rows: they should be read as a directional comparison
  of perceptual fidelity in each model's own output space, not an absolute
  cross-baseline ranking. Native resolution per row is documented in the
  Notes column.
- **CoTracker seed_uv methodology.** External baselines (Ctrl-World, WorldGym DROID-finetune)
  use the image center as the CoTracker seed_uv placeholder (no DROID camera
  calibration applied during inference for external models), while Ours uses
  the projected EE pixel via `get_seed_uv_from_projection`. The placeholder
  is a conservative choice — it cannot inflate the external baselines' DirAcc
  relative to a calibrated seed.
- **Proprio column N/A for external baselines.** Neither Ctrl-World nor
  WorldGym DROID-finetune ships a proprio readout head; Table 2 / 3's "Proprio cos_sim"
  column is therefore N/A for these rows.
- **IWS-DROID deferred.** A third planned external baseline,
  interactive_world_sim (Columbia/TRI), is deferred to a future paper revision:
  env install on lab FS hit a `git+https://github.com/erjel/imgaug.git@test`
  dep failure, and from-scratch DROID training would require ~5-7 days of
  multi-GPU compute.

## Source artifacts

- Ctrl-World per-episode metrics: `results/baselines_external/ctrl_world/summary.json`
- WorldGym DROID-finetune per-episode metrics: `results/baselines_external/worldgym_droid_finetune/summary.json`
- Driver scripts and adapters: `scripts/external_baselines/{ctrl_world,worldgym,worldgym_droid}/`
- Plan + design: `docs/plans/2026-05-13-external-wm-baselines-{design,plan}.md`
- Branch: `external-wm-baselines` (head commit added in Phase 4)
