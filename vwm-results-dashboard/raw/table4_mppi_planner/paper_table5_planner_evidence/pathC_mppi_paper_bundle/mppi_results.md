# Path C MPPI Results (live aggregation, 2026-05-15)

**Total runs:** 10 across 3 MPPI variants.

## Aggregate

| variant | n | env_success | reach_success | mean min_reach (cm) | best min_reach (cm) |
|---|---|---|---|---|---|
| argmin+full | 1 | 0/1 (0%) | 0/1 (0%) | 9.65 | 9.65 |
| mean+early-term | 6 | 0/6 (0%) | 6/6 (100%) | 4.11 | 0.11 |
| priming+full | 3 | 0/3 (0%) | 3/3 (100%) | 0.68 | 0.05 |

## Per-run

| variant | task | demo | source | env✓ | reach✓ | n_steps | final_wp | min_reach (cm) |
|---|---|---|---|---|---|---|---|---|
| argmin+full | OpenDrawer | demo_0094 | archive | ❌ | ❌ | 200 | 3/8 | 9.6 |
| mean+early-term | OpenDrawer | demo_0094 | archive | ❌ | ✅ | 81 | 8/8 | 7.9 |
| mean+early-term | OpenDrawer | demo_0095 | archive | ❌ | ✅ | 83 | 9/9 | 8.1 |
| mean+early-term | OpenDrawer | demo_0096 | archive | ❌ | ✅ | 92 | 8/8 | 7.9 |
| mean+early-term | OpenDrawer | demo_0097 | archive | ❌ | ✅ | 200 | 10/10 | 0.1 |
| mean+early-term | OpenDrawer | demo_0098 | archive | ❌ | ✅ | 200 | 8/8 | 0.1 |
| mean+early-term | PickPlaceCounterToCabinet | demo_0100 | archive | ❌ | ✅ | 200 | 9/9 | 0.5 |
| priming+full | OpenDrawer | demo_0094 | active | ❌ | ✅ | 300 | 8/8 | 0.1 |
| priming+full | OpenDrawer | demo_0094 | archive | ❌ | ✅ | 300 | 8/8 | 1.9 |
| priming+full | OpenDrawer | demo_0095 | active | ❌ | ✅ | 300 | 9/9 | 0.1 |
