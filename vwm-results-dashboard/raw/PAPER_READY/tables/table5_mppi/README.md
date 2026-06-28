# Table 5 (tab:wm_policy_eval) — Frozen MPPI / WM-shoot / Random numbers

**Frozen:** 2026-05-27 as part of `_archive/2026-05-27_cleanup/`.
**Source script:** `CoRL-2026-heterogeneous/scripts/aggregate_tab3_n12.py`
**Output:** `aggregate_output.txt` in this directory.

## Why this freeze exists

`aggregate_tab3_n12.py` reads from several load-bearing folders that would otherwise
have been moved to `_archive/2026-05-27_cleanup/`. Freezing the numbers means we
can retire those inputs without invalidating the Table 5 (`tab:wm_policy_eval`)
claim in `main.tex`.

## Inputs read by the script

| Path | Used for row |
|---|---|
| `results/pathC_random_reach_baseline/<task>_reach25_random_init_*` | Random uniform action |
| `studies/2026-05-14-pathC-final/results/sweep/<task>/seed*/planning_results.json` | WM-shoot (goal-directed), 150 steps |
| `results/_archive/pathC_mppi/<task>/demo_*/planning_results.json` | MPPI mean, 200 steps |
| `results/pathC_mppi_balance/mean/<task>/demo_*/planning_results.json` | MPPI mean, 200 steps |
| `results/pathC_mppi_priming/<task>/demo_*/planning_results.json` | MPPI + priming, 300 steps |
| `results/pathC_mppi_priming_extra/<task>/demo_*/planning_results.json` | MPPI + priming, 300 steps |
| `results/pathC_mppi_balance/priming/<task>/demo_*/planning_results.json` | MPPI + priming, 300 steps |

**Do not delete any of these paths** until Table 5 is locked into the camera-ready paper.

## Frozen output (2026-05-27)

```
n per task = 3; tasks = ('OpenDrawer', 'OpenCabinet', 'TurnOnMicrowave', 'PickPlaceCounterToCabinet')
bootstrap 95% CI on the median, 2000 iters; rng seed = 20260518

Row                                           n  median (cm)                95% CI
----------------------------------------------------------------------------------
Random uniform action                        12        34.89        [25.21, 39.63]
WM-shoot (goal-directed), 150 steps          12         0.62          [0.48, 2.07]
MPPI mean, 200 steps                         12         0.11          [0.07, 0.33]
MPPI + demo-action priming, 300 steps        12         0.10          [0.07, 0.16]
```

See `aggregate_output.txt` for the per-task subsampled values.

## Caveat: non-deterministic bootstrap CI

The bootstrap CI uses `rng = random.Random(SEED + hash(row_name) % 10_000)`.
Python's `hash()` is per-process randomized (PEP 456), so the bootstrap CIs
shift slightly between runs even though the subsampled values themselves are
deterministic. The medians and per-task values are stable; only the bootstrap
CI bounds drift by a few hundredths of a cm.

The current paper table reports:

| Method | Median (cm) | 95% CI |
|---|---|---|
| Random uniform action | 34.9 | [31.0, 39.8] |
| WM-shoot, random candidates | 26.6 | [23.9, 35.4] |
| WM-shoot, goal-biased | 0.7 | [0.4, 2.4] |
| MPPI | 0.10 | [0.07, 0.17] |
| MPPI + expert-action priming | 0.11 | [0.07, 4.88] |

The "WM-shoot, random candidates" row in the paper is NOT produced by this
script — it is supplied by a separate aggregator that this freeze does not
cover.

## Reproducing

```bash
cd /net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/video_world_model
mamba activate ei_world_model
python CoRL-2026-heterogeneous/scripts/aggregate_tab3_n12.py
```
