# Franka 6-way mixture FT vs zero-shot (paper §5)

FT was 2000 steps on `franka_6way_action_proj_mixture`. Columns:
zs = zero-shot from the source ckpt (no FT); FT@1k / FT@2k = FT
after 1000 and 2000 training steps. Δ@2k = FT@2k − zs.

| Task | A zs | A FT@1k | A FT@2k | A Δ@2k | 3way zs | 3way FT@1k | 3way FT@2k | 3way Δ@2k |
|---|---|---|---|---|---|---|---|---|
| Box / success | 0.625 | 0.708 | 0.792 | +0.167 | 0.708 | 0.542 | 0.667 | -0.042 |
| Box / failure | 0.667 | 0.750 | 0.750 | +0.083 | 0.708 | 0.750 | 0.792 | +0.083 |
| Mustard / success | 0.667 | 0.750 | 0.792 | +0.125 | 0.792 | 0.708 | 0.833 | +0.042 |
| Mustard / failure | 0.583 | 0.875 | 0.875 | +0.292 | 0.917 | 0.792 | 0.875 | -0.042 |
| Rope / success | 0.667 | 0.750 | 0.792 | +0.125 | 0.750 | 0.750 | 0.750 | +0.000 |
| Rope / failure | 0.750 | 0.625 | 0.833 | +0.083 | 0.667 | 0.667 | 0.458 | -0.208 |
| Rice cooker / success | 0.500 | — | 0.708 | +0.208 | 0.708 | — | 0.625 | -0.083 |
| Rice cooker / failure | 0.750 | — | 1.000 | +0.250 | 0.833 | — | 0.917 | +0.083 |
| Whiteboard / success | 0.625 | — | 0.833 | +0.208 | 0.625 | — | 0.417 | -0.208 |
| Whiteboard / failure | 0.792 | — | 0.833 | +0.042 | 0.708 | — | 0.750 | +0.042 |
| **Mean** | 0.662 | 0.743 | **0.821** | **+0.158** | 0.742 | 0.701 | **0.708** | **-0.033** |
