# Paper Qualitative Figures — Action as Rendering

Curated qualitative artifacts for CoRL 2026 paper draft.
Generated 2026-04-20. All files are copies (not symlinks) for export portability.

**Draft status:** Current artifacts use step-5125 A/C checkpoints (n=4 episodes).
Final main-text figures should be regenerated from later step-matched checkpoints
(10K+ preferred) using the 20-episode eval list. See
`docs/plans/2026-04-21-final-paper-eval-runbook.md` for the full re-eval sequence.

**Grid rollouts** (2x4 direction grids in `../videos/grid_reference_*.mp4`) are from
the **reference** checkpoint at step 25750, NOT the step-matched A/C pair. These are
draft/reference artifacts only. Step-matched grid commands are templated in the runbook.

## Figure Inventory

| Figure | Dir | Claim supported | Files | Metrics represented | Location |
|--------|-----|----------------|-------|--------------------|---------| 
| **Fig 1: Method** | `fig1_method/` | Action-as-rendering unifies robot EE + human fingertip in one visual representation | 10 PNGs (RGB, R, G, B, composite x2) | N/A (method illustration) | Main |
| **Fig 2: Visual Quality** | `fig2_visual_quality_a_vs_c/` | A and C produce near-identical DROID visual quality under action replay | 26 PNGs (comparison composites + per-horizon panels) | LPIPS, PSNR, SSIM | Main (draft; early step-matched A/C) |
| **Fig 3: Controllability** | `fig3_action_controllability/` | Adding human data does not degrade robot action controllability | 20 MP4s (CLVR/RAIL x 5 actions x A/C) | DirAcc, D_sep | Main (draft; early step-matched A/C) |
| **Fig 4: Sensitivity** | `fig4_action_sensitivity_diagnostic/` | Model responds to action changes; negation > zeroing | README with metric table only | Action sensitivity, negated action sensitivity | Appendix (diagnostic) |
| **Fig 5: SSv2** | `fig5_ssv2_examples/` | Human data improves human-video prediction (26% streaming loss reduction) | 10 MP4s (5 examples x A/C) + 1154 available locally | SSv2 val/streaming_loss | Main |
| **Appendix Debug** | `appendix_debug/` | Hard episode transparency + seed verification | 8 PNGs + 10 MP4s | DirAcc failure mode | Appendix |

## Main text vs Appendix

**Main text (Figs 1-3, 5):**
- Fig 1: Method rendering — the core "action as rendering" concept
- Fig 2: A vs C visual quality — paired action-replay, CLVR + RAIL (moving episodes)
- Fig 3: A vs C controllability — CoTracker-tracked interventions, CLVR + RAIL
- Fig 5: SSv2 cross-domain — human-video prediction improvement

**Appendix / Diagnostic (Fig 4, Debug):**
- Fig 4: Action sensitivity — diagnostic metric, not main paper evidence
- Debug: Seed verification PNGs, IRIS hard-episode analysis

## Checkpoint Maturity Note

The current A/C artifacts are evaluated at early step-matched checkpoints:
- A robot-only: step 5125
- C robot+human: step 5125
- SSv2 streaming-loss eval: step 5625 hard-copy checkpoints

These are draft evidence for checking whether robot+human training degrades early robot
controllability or visual quality. They should be updated before final paper submission
if later step-matched A/C checkpoints become available.

Do not mix these with the converged robot-only reference checkpoint at step 25750 when
making the main A-vs-C claim. Any later paper table or main qualitative figure should
use the same step-matched checkpoint pair.

## Caveats

1. **Limited sample size:** n=4 DROID episodes / 16 controllability trials. Bootstrap 95%
   CIs are available in the visual quality rerun JSONs but are wide.
2. **Full-frame visual quality favors copy-last-frame:** The model's action response is
   localized to the robot arm region; full-frame LPIPS/PSNR/SSIM include the static
   background, which biases toward static predictions.
3. **Action sensitivity is diagnostic:** The negated-action trajectory (reflect EE around
   anchor) may produce physically odd paths. Labeled diagnostic, not a main metric.
4. **Standard I3D-FVD is NOT implemented.** Exploratory S3D-FD scaffold exists but is not
   paper-ready. See `scripts/_archive/eval_fvd.py` and Section 4 of the paper experiments doc.
5. **SSv2 examples are convenience-sampled** (first 5 by hash), not metric-selected. For
   metric-driven selection (top/bottom delta), use the full WandB runs (4g4kcgct, x4ie5lxz).
6. **A and C SSv2 examples are NOT paired** — use WandB for paired clip comparison.

## Metric Sources

| Metric | Script | JSON location |
|--------|--------|---------------|
| LPIPS / PSNR / SSIM + bootstrap CIs | `scripts/eval_visual_quality.py` | `visual_quality/ablation_{A,C}_v2_rerun_ci/summary.json` |
| DirAcc / D_sep | `scripts/eval_action_metrics.py` | `quantitative/ablation_{A,C}_step5125/summary.json` |
| Action sensitivity / Negated action sensitivity | `scripts/eval_visual_quality.py` | `visual_quality/ablation_{A,C}_v2_rerun_ci/summary.json` |
| SSv2 streaming_loss | `scripts/eval_ssv2_offline.sh` | WandB runs 4g4kcgct (A), x4ie5lxz (C) |
| Aggregated table | `scripts/aggregate_paper_metrics.py` | `paper_metrics_summary.{csv,md}` |
