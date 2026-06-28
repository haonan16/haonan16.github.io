# Paper Qualitative Figure Candidates — Visual-Proprioceptive Robotic Simulator

**Updated:** 2026-04-26 11:00
**Framing:** Action as Rendering for Visual-Proprioceptive Robotic World Models

---

## Visual Robotic Simulator Figures (Core Claim)

### Fig 1: Method Rendering (DONE)
- **Location:** `results/corl_experiments_2026-04-20/figures/fig1_method/`
- **Content:** DROID RPL + SSv2 per-channel R/G/B PNGs
- **Claim:** Action-as-rendering visual representation
- **Status:** Paper-safe

### Fig 2: SSv2 A vs C vs GT Strips
- **Source:** WandB [4g4kcgct](https://wandb.ai/hca/ei_world_model/runs/4g4kcgct) (A), [x4ie5lxz](https://wandb.ai/hca/ei_world_model/runs/x4ie5lxz) (C)
- **Claim:** Human data improves human-video prediction (C 26% lower SSv2 loss)
- **Selection:** Top-5 by |loss_A - loss_C|, bottom-5 closest, 1-2 failure cases
- **Status:** Clip selection needed

### Fig 3: Visual Simulator — Action Grid Rollouts
- **Claim:** Sustained directional controllability
- **Best candidates:**

| Checkpoint | Episode | Grid | Path | Status |
|---|---|---|---|---|
| A@25750 | RPL | 10-step 2x4 | `results/eval_A_converged_step25750_20ep/grids/grid_RPL_10steps.mp4` | Paper-safe |
| A@25750 | CLVR | 10-step 2x4 | `results/eval_A_converged_step25750_20ep/grids/grid_CLVR_10steps.mp4` | Paper-safe |
| C@18000 | RPL | 5-step 2x4 | `results/eval_C_step18000_20ep/grids/grid_RPL_5steps.mp4` | Progress-only |
| C@18000 | CLVR | 5-step 2x4 | `results/eval_C_step18000_20ep/grids/grid_CLVR_5steps.mp4` | Progress-only |

- **Note:** A vs C NOT step-matched. Label clearly.

### Fig 4: Horizon Displacement Curves (DONE)
- **Source:** `results/eval_horizon_A_converged/horizon_summary.json`
- **Claim:** Sustained controllability over 10 streaming steps, magnitude-proportional
- **Key data:**
  - 0.05m: 62-103 px final displacement (strong)
  - 0.02m: 11-32 px
  - 0.01m: 5-14 px
  - nop: 4.1 px (flat drift)
- **Status:** Paper-safe (A@25750, 4 verified episodes)

### Fig 5: No-Action vs Action-Conditioned Visual Comparison
- **Claim:** Action conditioning closes the generation gap
- **Best episodes (largest LPIPS gap):**

| Episode | No-action LPIPS | C@18000 LPIPS | Improvement | Notes |
|---|---|---|---|---|
| **IPRL** | 0.190 | 0.083 | 2.3× | Largest gap |
| **GuptaLab #2** | 0.103 | 0.050 | 2.1× | Clean improvement |
| **AUTOLab #1** | 0.088 | 0.045 | 2.0× | Consistent |
| **ILIAD #2** | 0.107 | 0.060 | 1.8× | |
| **CLVR #2** | 0.118 | 0.096 | 1.2× | Model also beats copy-last |

- **Panels:** `results/eval_C_step18000_20ep/visual_quality/panels/` vs `results/no_action_droid_tuned_step16500_full/panels/`
- **Status:** Paper-safe (20-ep, immutable checkpoints)

---

## Visual-Proprioceptive Simulator Figures (Extension Claim)

### Fig 6: Sampled-Rollout Proprio Accuracy
- **Claim:** Model extracts meaningful EE trajectory from generated video
- **Source:** `results/eval_proprio_sampled_AP_step20000/summary.json`
- **Key data (best episodes for figure):**

| Episode | cos_sim | L2 (mm) | Good for figure? |
|---|---|---|---|
| **RAIL** | **0.995** | 7.0 | ✅ Best — near-perfect direction |
| **CLVR** | **0.988** | 13.8 | ✅ Strong with meaningful displacement |
| **AUTOLab** | **0.982** | 5.3 | ✅ Near-perfect |
| **REAL** | **0.955** | 19.8 | ✅ Strong, large displacement |
| RPL | 0.083 | 8.9 | ❌ Failure case (appendix) |
| ILIAD #1 | -0.929 | 84.7 | ❌ Wrong direction (appendix) |

- **Visualization:** Per-episode predicted vs GT delta as 3D arrow overlay on video frame
- **Status:** Data exists. Visualization script needed (`Part E`).

### Fig 7: Proprio Counterfactual (if time)
- **Claim:** Different actions produce different predicted EE trajectories
- **Setup:** Same initial history, commands: +x, -x, +y, -y, zero
- **Show per cell:** generated RGB + predicted proprio arrow overlay
- **Status:** NOT YET IMPLEMENTED. Requires extending `teleop_grid_rollout.py` with `return_proprio=True`.

---

## Appendix Figures

### IRIS Failure Case
- **DirAcc=0.25** across all checkpoints (persistent)
- **Debug:** `results/eval_C_step18000_20ep/action_metrics/debug/IRIS_seed_debug.png`
- **Videos:** `results/eval_C_step18000_20ep/action_metrics/videos/IRIS_*.mp4`

### Seed-Point Edge Cases
- **IPRL:** seed_uv=(0,1), DirAcc=0, D_sep=0
- **PennPAL:** seed_uv=(0,1), DirAcc=0, D_sep=0
- **ILIAD #1:** seed_uv=(449,0), DirAcc=0 at step 18000
- **Debug images:** `results/eval_C_step18000_20ep/action_metrics/debug/`

### Proprio Failure Cases
- **RPL:** cos_sim=0.083 (small GT delta, direction noisy)
- **ILIAD #1:** cos_sim=-0.929 (completely wrong direction, 84.7mm L2)
- Source: `results/eval_proprio_sampled_AP_step20000/summary.json`

### A+P Video Quality (pending — Part D running)
- **Claim:** Proprio head does not degrade video quality
- **Source:** `results/eval_AP_step20000_visual_quality/` (running now)
- **Compare:** A@25750 LPIPS=0.043 (converged, not step-matched)

---

## Summary: What We Can Claim

| Claim | Evidence | Strength | Status |
|---|---|---|---|
| **Visual-proprio simulator** | C+P cos_sim=0.730±0.278, zero VQ degradation | **Strong** | ✅ **Core claim** |
| **Visual robotic simulator** | DirAcc=0.538, horizon curves, VQ, grids | Strong | ✅ Core claim |
| **Human data helps proprio** | C+P (0.730) > A+P (0.678) at convergence | Strong | ✅ Core claim |
| **Human data doesn't degrade video** | A vs C step-matched, overlapping CIs | Strong | ✅ Core claim |
| **Human data helps human-video** | SSv2 577-clip, 26% gap | Strong | ✅ Core claim |
| **Rendering > raw-vector** | Raw-vector not controllable at step 6875 | Moderate | ⚠️ Waiting on step-matched eval |
| **nf17+P (longer context)** | OOM fixed with gradient checkpointing, relaunched | Pending | ⏳ Training |
| **Gripper conditioning** | Code done, jobs running | Pending | ⏳ Training |
