# Paper Table Artifacts

## Table 1: Action Representation

Use the matched proprio architecture for the main visual-action-projection row:

| Method | DirAcc | D_sep px | LPIPS |
|---|---:|---:|---:|
| No action | N/A | N/A | 0.067 |
| Raw-vector AdaLN | 0.000 | 0.0 | 0.064 |
| Raw-vector cross-attention | 0.000 | 0.0 | 0.073 |
| Ours (robot-only + proprio) | 0.917 | 8.5 | 0.042 |

The older 0.931 DirAcc robot-only result is a no-proprio checkpoint and should be
kept out of the main table for consistency. The 0.917 figure is the
2026-05-15 re-run of A+P@20K under the strict 72-trial protocol; see
`table1_per_direction_diracc/` for the +x/−x/+y/−y breakdown.

## Table 2: Heterogeneous Data

Both rows use the same nf5 architecture with the proprioceptive readout head.

| Model | DirAcc | LPIPS | Proprio cos. | SSv2 loss |
|---|---:|---:|---:|---:|
| Ours (robot-only + proprio) | 0.917 | 0.042 | 0.678 | 0.210 |
| Ours (robot+human + proprio) | 0.875 | 0.042 | 0.730 | 0.155 |

## Interaction-Heavy Holdout

Both models are evaluated on the same 10 episodes and 38 valid trials. Two ILIAD
x-axis trials are excluded because their projected image-space direction is zero.

| Model | DirAcc | D_sep px |
|---|---:|---:|
| Ours (robot-only + proprio) | 0.816 | 8.5 |
| Ours (robot+human + proprio) | 0.974 | 10.0 |

## Proprioceptive Prediction

| Model | Context frames | Cosine similarity | L2 error mm |
|---|---:|---:|---:|
| Ours (robot-only + proprio) | 5 | 0.678 +/- 0.348 | 29.4 |
| Ours (robot+human + proprio) | 5 | 0.730 +/- 0.278 | 19.0 |

## Per-direction DirAcc Breakdown (Table 1)

`table1_per_direction_diracc/table1_per_direction.{csv,md}` reports +x / −x /
+y / −y passes, testable counts, and conditional DirAcc for Ours,
Ctrl-World, and WorldGym on the same 72-trial protocol. Use it alongside
Table 1 to defend against "did Ours just pick the easy direction?" or
"is Ctrl-World failing on direction or on responsiveness?" reviewer
questions.

## RoboCasa Trajectory Executability

Use `table8_robocasa_replay/table8_robocasa_replay.{csv,md}` for the
RoboCasa execution-validity check. These rows establish that recorded RoboCasa
trajectories are executable under the matched `HYBRID_MOBILE_BASE` controller
after action-layout remapping. They are oracle/demo-replay checks, not
learned-policy or WM-planning success claims.
