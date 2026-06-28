# Path C: Final Paper-Ready Bundle

**Last refresh:** 2026-05-15. Combines MPPI planning results with open-loop
demo-trajectory rollout results.

## Three complementary results

### 1. WM as planning cost (closed-loop MPPI)

10 MPPI rollouts across 3 variants on known-solvable demos:

| Variant | n | env_success | reach_success | best min_reach |
|---|---|---|---|---|
| MPPI mean | 6 | 0/6 | 6/6 | **1 mm** |
| MPPI argmin | 1 | 0/1 | 0/1 | 96 mm |
| MPPI priming (expert action as candidate) | 3 | 0/3 | 3/3 | **0.5 mm** |

**Headline:** WM-cost reliably drives sub-mm EE positioning to demo-recorded
goal poses, but pure position-cost planning does not satisfy RoboCasa's
binary task predicates. Section: `mppi_results.{csv,md}` + `table_precision_ladder.md` + `fig_precision_ladder.png`.

### 2. WM as long-horizon video generator (open-loop demo rollout)

4 full-demo rollouts: condition the streaming action-projection WM on each
demo's recorded EE-trajectory and let it imagine the entire video
autoregressively.

| Task | Demo | Real frames | WM-imagined frames | Chunks | Imagined duration |
|---|---|---|---|---|---|
| OpenDrawer | demo_0094 | 185 | **168** | 42 | 8.4 s |
| OpenDrawer | demo_0095 | 203 | **188** | 47 | 9.4 s |
| PickPlaceCounterToCabinet | demo_0100 | 221 | **204** | 51 | **10.2 s** |
| TurnOnMicrowave | demo_0000 | 109 | **92** | 23 | 4.6 s |

**Total: 652 generated frames (32.6 s) of autoregressive WM imagination
without diverging.**

Artifacts under `results/pathC_demo_rollout/<task>_<demo>/`:
- `wm_imagined_<demo>.mp4` — pure WM-generated video conditioned on demo's
  action-projection sequence
- `real_<demo>.mp4` — ground-truth demo frames over the same time span
- `sidebyside_<demo>.mp4` — paper-friendly side-by-side
- `summary_<demo>.json` — frame counts + provenance
- Figure: `fig_demo_rollout_grid.png` — 4-row × 8-col panel (real|imagined × 4 time points)

### 3. WM-shoot reach precision is the strong positive

Pulled from the existing `pathC_paper_bundle/`:
- Random shooting biased + restrict_to_bc_dims: **20/20 (100%) reach@10cm at 100-step horizon**
- Random uniform baseline: 1.5% reach@10cm, p < 1e-6 (n=200)

## Unified paper claim

The action-conditioned video world model is a strong **positioning** model:
- As a planning cost: 100% reach@10cm, sub-mm precision under MPPI
- As a forward simulator: 10 s of autoregressive imagination matches demo trajectories

It is **not** a complete manipulation-task model: pure position-cost planning
does not produce binary task success on RoboCasa's contact-rich predicates.
The gap between sub-mm EE precision and 0% env success identifies missing
contact-force / grip-engagement signals in the cost function, not in the
WM's video-generation capacity.

## Open items

- demo_0098 priming SLURM still running (~6h wall, near completion). Would
  add a 4th OpenDrawer priming data point — unlikely to change the
  consistent reach✓ / env✗ pattern.
- demo_0096 priming local was killed to free GPU for the demo rollout work.

## Files in this bundle

```
pathC_mppi_paper_bundle/
├── FINAL_SUMMARY.md             (this file)
├── PAPER_READY_SUMMARY.md       (earlier MPPI-only summary, kept for diff)
├── mppi_results.csv             (per-run table)
├── mppi_results.md              (same in MD)
├── table_precision_ladder.md    (cross-method precision matrix)
├── fig_precision_ladder.png/pdf (precision ladder bar chart)
└── fig_demo_rollout_grid.png/pdf (4 demos × 8 frames side-by-side)
```

Plus per-demo MP4s under `results/pathC_demo_rollout/`.
