# Table 5 — Updated 3-VLA Policy Pool (WM as Offline Evaluator)

## Paper Table 5 metric: WM-path vs SIM-path Spearman ρ

| Policy | n | Succ rate | WM-path mean | SIM-path mean | ρ(WM,SIM) | p | CI95 |
|---|---:|---:|---:|---:|---:|---:|---|
| beingh05_libero_robocasa | 106 | 0.10 | 5.930 | 2.086 | 0.764 | 1.66e-33 | [0.67, 0.83] |
| beingh05_robocasa | 105 | 0.10 | 6.187 | 1.971 | 0.854 | 2.57e-62 | [0.79, 0.89] |
| cosmos | 218 | 0.77 | 1.354 | 1.610 | 0.054 | 0.431 | [-0.12, 0.22] |
| dp_canonical | 60 | 0.00 | 0.510 | 1.059 | -0.026 | 0.841 | [-0.29, 0.24] |
| dp_step10k | 60 | 0.00 | 0.556 | 2.219 | 0.084 | 0.521 | [-0.15, 0.30] |
| pi0_robocasa | 40 | 0.00 | 5.918 | 3.358 | 0.562 | 2.82e-05 | [0.30, 0.73] |
| random | 65 | 0.00 | 8.949 | 4.212 | 0.381 | 0.00107 | [0.15, 0.57] |
| scripted-noisy | 85 | 0.00 | 6.834 | 2.368 | 0.235 | 0.0275 | [0.03, 0.42] |
| scripted-toward-goal | 65 | 0.00 | 0.898 | 0.155 | 0.387 | 0.000878 | [0.16, 0.58] |

**Pooled** (n=804): ρ = 0.531, p = 1.29e-70, CI95 = [0.48, 0.58]

## New (stronger) metric: WM-path vs binary SUCCESS Spearman ρ

| Policy | n | Succ rate | ρ(WM,success) | p | CI95 |
|---|---:|---:|---:|---:|---|
| beingh05_libero_robocasa | 106 | 0.10 | -0.085 | 0.385 | [-0.15, 0.22] |
| beingh05_robocasa | 105 | 0.10 | 0.067 | 0.497 | [-0.17, 0.17] |
| cosmos | 218 | 0.77 | -0.048 | 0.477 | [-0.09, 0.18] |
| dp_canonical | 60 | 0.00 | 0.209 | 0.103 | [nan, nan] |
| dp_step10k | 60 | 0.00 | 0.120 | 0.356 | [nan, nan] |
| pi0_robocasa | 40 | 0.00 | 0.245 | 0.119 | [nan, nan] |
| random | 65 | 0.00 | 0.015 | 0.903 | [nan, nan] |
| scripted-noisy | 85 | 0.00 | 0.134 | 0.219 | [nan, nan] |
| scripted-toward-goal | 65 | 0.00 | 0.055 | 0.661 | [nan, nan] |

**Pooled** (n=804): ρ = -0.152, p = 1.31e-05, CI95 = [-0.25, -0.12]
**Cross-policy ordering**: ρ between 9 policies' WM-path-mean and sim-success-rate = 0.150, p = 0.688, CI95 = [-0.55, 0.82]
