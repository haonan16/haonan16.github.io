# Table 5 — Updated 3-VLA Policy Pool (WM as Offline Evaluator)

## Paper Table 5 metric: WM-path vs SIM-path Spearman ρ

| Policy | n | Succ rate | WM-path mean | SIM-path mean | ρ(WM,SIM) | p | CI95 |
|---|---:|---:|---:|---:|---:|---:|---|
| beingh05_libero_robocasa | 66 | 0.11 | 5.974 | 2.094 | 0.759 | 1.23e-20 | [0.63, 0.84] |
| beingh05_robocasa | 65 | 0.11 | 6.314 | 2.053 | 0.839 | 1.5e-34 | [0.74, 0.89] |
| cosmos | 198 | 0.75 | 0.945 | 1.635 | 0.127 | 0.074 | [-0.05, 0.29] |

**Pooled** (n=329): ρ = 0.243, p = 5.93e-06, CI95 = [0.12, 0.35]

## New (stronger) metric: WM-path vs binary SUCCESS Spearman ρ

| Policy | n | Succ rate | ρ(WM,success) | p | CI95 |
|---|---:|---:|---:|---:|---|
| beingh05_libero_robocasa | 66 | 0.11 | -0.341 | 0.00368 | [-0.19, 0.26] |
| beingh05_robocasa | 65 | 0.11 | -0.121 | 0.333 | [-0.24, 0.23] |
| cosmos | 198 | 0.75 | -0.169 | 0.0164 | [-0.13, 0.15] |

**Pooled** (n=329): ρ = -0.462, p = 4.03e-21, CI95 = [-0.54, -0.36]
**Cross-policy ordering**: ρ between 3 policies' WM-path-mean and sim-success-rate = -0.500, p = 0.564, CI95 = [-0.50, 1.00]
