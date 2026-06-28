# Fig 4: Action Sensitivity Diagnostic

**Status: DIAGNOSTIC ONLY — not a main paper metric.**

Measures how much the model's output changes when the action input is modified.
For the same visual context, generate futures under:
- **Real action** — logged DROID EE trajectory
- **Zero action** — EE held stationary at the history anchor
- **Negated action** — EE trajectory reflected around the history anchor (diagnostic;
  may produce physically odd paths due to depth/orientation artifacts)

Action sensitivity = LPIPS(real_output, zero_output).
Negated action sensitivity = LPIPS(real_output, negated_output).

Higher values indicate stronger action response. Near-zero on stationary episodes (RPL, IRIS)
is expected — there is no real motion to respond to.

## Metrics from rerun summaries (step 5125)

### Overall (4 episodes)

| Metric | A (robot-only) | C (robot+human) |
|--------|---------------|-----------------|
| Action Sensitivity (all) | 0.0052 | 0.0068 |
| Action Sensitivity (moving only, n=2) | 0.0104 | 0.0136 |
| Negated Action Sensitivity (all) | 0.0078 | 0.0087 |
| Negated Action Sensitivity (moving only) | 0.0157 | 0.0173 |
| Motion Direction Diagnostic | 0.111 | 0.134 |

### Per moving episode

| Episode | delta (mm) | A Sens | C Sens | A NegSens | C NegSens |
|---------|-----------|--------|--------|-----------|-----------|
| CLVR | 25.8 | 0.0056 | 0.0068 | 0.0092 | 0.0104 |
| RAIL | 26.9 | 0.0151 | 0.0204 | 0.0221 | 0.0241 |
| RPL | 0.1 | 0.0000 | 0.0001 | 0.0000 | 0.0000 |
| IRIS | 0.0 | 0.0000 | 0.0001 | 0.0000 | 0.0002 |

## Interpretation

- NegSens > Sens on both moving episodes for both A and C. This is expected: negation is a
  stronger intervention than zeroing.
- C shows slightly higher sensitivity than A on both moving episodes, consistent with the
  hypothesis that human data does not diminish (and may slightly improve) action response.
- Near-zero values on RPL/IRIS are expected — these episodes have < 1mm EE motion.

## No direct visualization

No side-by-side real/zero/negated future frame visualization exists without a GPU rerun
with `--save_panels`. The metric values above are from the summary JSONs only.

## Source data

- A: `visual_quality/ablation_A_v2_rerun_ci/summary.json`
- C: `visual_quality/ablation_C_v2_rerun_ci/summary.json`
