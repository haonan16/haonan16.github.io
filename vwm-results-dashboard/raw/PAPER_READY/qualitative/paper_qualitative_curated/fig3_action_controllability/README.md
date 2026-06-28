# Fig 3: Action Controllability — DirAcc / D_sep

CoTracker-tracked rollouts under controlled action interventions (+x, -x, +y, -y, zero)
for A (robot-only) and C (robot+human) at step 5125.

## Episodes shown

- **CLVR** — Both A and C achieve DirAcc=1.0. Strong directional response.
- **RAIL** — Both A and C achieve DirAcc=0.5. Mixed direction accuracy (x correct, y mixed).

IRIS is in `appendix_debug/` as a hard-episode / CoTracker limitation example (DirAcc=0.25).

## Video naming

`{A,C}/{EP}_{action}.mp4` — CoTracker overlay video showing tracked contact point.
Red dot = current tracked position, green dot = seed point, yellow line = trajectory.

Actions: `+x`, `-x`, `+y`, `-y`, `zero` (0.02m magnitude in base frame).

## Metrics

### Step-matched ablation (A vs C at step 5125)

| Metric | A (robot-only) | C (robot+human) |
|--------|---------------|-----------------|
| **DirAcc** (overall) | 0.688 (11/16) | 0.688 (11/16) |
| **DirAcc x-axis** | 0.875 (7/8) | 0.875 (7/8) |
| **DirAcc y-axis** | 0.500 (4/8) | 0.500 (4/8) |
| **D_sep** (px) | 10.7 | 11.7 |

### Per-episode

| Episode | A DirAcc | C DirAcc | A D_sep | C D_sep |
|---------|----------|----------|---------|---------|
| RPL | 1.000 | 1.000 | 11.6 | 6.6 |
| IRIS | 0.250 | 0.250 | 0.1 | 0.1 |
| CLVR | 1.000 | 1.000 | 12.7 | 15.9 |
| RAIL | 0.500 | 0.500 | 18.4 | 24.4 |

## Interpretation

Adding human data (C) does not degrade DROID action controllability. DirAcc is identical
across all episodes and axes. D_sep is comparable overall (10.7 vs 11.7) with per-episode
variation (C stronger on CLVR/RAIL, A stronger on RPL).

## Caveats

- **n=4 episodes, 16 trials total.** This is sufficient for directional claims but CIs
  will be wide. Bootstrap CIs are implemented in `eval_action_metrics.py` but not yet
  rerun for these specific JSONs.
- Magnitude: 0.02m (~2cm) per axis, sample_steps=40, seed=0, use_real_history=True.
- Videos show CoTracker-tracked overlays, not raw model output.

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

## Source data

- A: `quantitative/ablation_A_step5125/summary.json`
- C: `quantitative/ablation_C_step5125/summary.json`
