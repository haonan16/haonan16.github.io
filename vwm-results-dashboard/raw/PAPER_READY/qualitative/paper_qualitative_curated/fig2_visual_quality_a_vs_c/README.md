# Fig 2: Visual Quality — A vs C (Action Replay)

Paired visual quality comparison between A (robot-only) and C (robot+human) at step 5125.
These are **action-replay** examples: the model receives the same logged DROID actions as
ground truth, so the comparison measures visual quality under identical conditioning.

## Episodes shown

- **CLVR** (EE delta = 25.8 mm) — moving episode, both methods DirAcc=1.0
- **RAIL** (EE delta = 26.9 mm) — moving episode, both methods DirAcc=0.5

RPL and IRIS are near-stationary (delta < 1mm) and are omitted from the main figure.

## Panel layout

Each `{EP}_t+{N}_A_vs_C.png` shows: history frame | projection | A prediction | C prediction | GT
The `{EP}_all_horizons_A_vs_C.png` shows all 4 horizons stacked.
Individual `{A,C}_{EP}_t+{N}.png` panels show: history | projection | prediction | GT.

## Metrics (from rerun with bootstrap CIs)

### Overall (4 episodes, action replay)

| Metric | A (robot-only) | C (robot+human) | Copy-last | VAE oracle |
|--------|---------------|-----------------|-----------|------------|
| **LPIPS** (lower=better) | 0.0281 [0.0216, 0.0347] | 0.0281 [0.0209, 0.0352] | 0.0168 | 0.0066 |
| **PSNR** dB (higher=better) | 30.5 [26.6, 33.6] | 31.0 [27.4, 34.0] | 33.6 | 39.1 |
| **SSIM** (higher=better) | 0.948 [0.929, 0.965] | 0.948 [0.934, 0.962] | 0.969 | 0.984 |

Brackets are episode-level bootstrap 95% CIs (n=4 episodes, 1000 resamples).

### Per moving episode

| Episode | A LPIPS | C LPIPS | A PSNR | C PSNR |
|---------|---------|---------|--------|--------|
| CLVR | 0.0237 | 0.0227 | 32.5 | 32.4 |
| RAIL | 0.0376 | 0.0367 | 24.6 | 25.7 |

## Interpretation

A and C are near-identical on all full-frame visual quality metrics. Both underperform
copy-last-frame on LPIPS/SSIM, which is expected: the model generates new content (moving
robot arm) while copy-last-frame benefits from the scene being mostly static. The model's
value is in action-conditioned generation, not static frame prediction.

## Source data

- A metrics: `visual_quality/ablation_A_v2_rerun_ci/summary.json`
- C metrics: `visual_quality/ablation_C_v2_rerun_ci/summary.json`
- Panel PNGs: `visual_quality/ablation_{A,C}_v2/panels/`
- Comparison composites: `figures/a_vs_c_comparison/`

## Caveats

- n=4 DROID episodes; bootstrap CIs are wide
- Full-frame metrics include static background (which dominates); the model's action
  response is localized to the robot arm region
- Checkpoint: step 5125, immutable (first-run crash)

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
