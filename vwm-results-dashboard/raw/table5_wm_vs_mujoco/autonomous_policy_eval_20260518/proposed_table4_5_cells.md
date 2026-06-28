# Proposed cells for combined Table 4/5 (sim-deterministic + WM partial)

Generated from saved sim rollouts under `results/pathB_rollouts_*/` and any WM scores already in `results/pathB_wm_scores_*/`. The WM rows are PARTIAL — WM scoring is still running. Re-run this script (`scripts/compute_proposed_table_cells.py`) to refresh as more rollouts get scored.

Sim path length convention: ``sum_t ||ee[t+1] - ee[t]||`` over the rollout's saved `ee_pos.npy` (in meters in the robot's base frame). Binary success comes from `success.json["success"]`. WM path mean is the average of `wm_proprio_path_length` from `world_model_scores.jsonl`. AUC is Mann-Whitney U vs `sim_success` — undefined (`nan`) when there are zero successes or zero failures in the scored subset.

## Policy: `beingh05_libero_robocasa`

| Task | n_sim | sim_path_mean (m) | success_rate | n_wm | wm_path_mean | n_succ | n_fail | AUC(wm_pl) | AUC(wm_fp) |
|------|-------|-------------------|--------------|------|--------------|--------|--------|------------|------------|
| OpenCabinet | 20 | 1.32 | 0.000 | 20 | 4.50 | 0 | 20 | nan | nan |
| OpenDrawer | 20 | 3.45 | 0.100 | 20 | 7.09 | 2 | 18 | 0.250 | 0.278 |
| PickPlaceCounterToCabinet | 20 | 1.78 | 0.250 | 20 | 6.78 | 5 | 15 | 0.480 | 0.200 |
| TurnOnMicrowave | 20 | 1.29 | 0.050 | 6 | 4.51 | 0 | 6 | nan | nan |
| **Pooled** | **80** | **1.96** | **0.100** | **66** | **5.97** | **7** | **59** | **0.559** | **0.262** |

## Policy: `beingh05_robocasa`

| Task | n_sim | sim_path_mean (m) | success_rate | n_wm | wm_path_mean | n_succ | n_fail | AUC(wm_pl) | AUC(wm_fp) |
|------|-------|-------------------|--------------|------|--------------|--------|--------|------------|------------|
| OpenCabinet | 20 | 1.11 | 0.050 | 20 | 4.14 | 1 | 19 | 0.842 | 0.526 |
| OpenDrawer | 20 | 3.34 | 0.100 | 20 | 7.68 | 2 | 18 | 0.306 | 0.056 |
| PickPlaceCounterToCabinet | 20 | 1.91 | 0.200 | 20 | 7.23 | 4 | 16 | 0.297 | 0.141 |
| TurnOnMicrowave | 20 | 1.34 | 0.050 | 5 | 5.86 | 0 | 5 | nan | nan |
| **Pooled** | **80** | **1.93** | **0.100** | **65** | **6.31** | **7** | **58** | **0.475** | **0.180** |

## Policy: `pi0_robocasa`

| Task | n_sim | sim_path_mean (m) | success_rate | n_wm | wm_path_mean | n_succ | n_fail | AUC(wm_pl) | AUC(wm_fp) |
|------|-------|-------------------|--------------|------|--------------|--------|--------|------------|------------|
| OpenCabinet | 10 | 1.89 | 0.000 | 0 | nan | 0 | 0 | nan | nan |
| OpenDrawer | 10 | 3.74 | 0.000 | 0 | nan | 0 | 0 | nan | nan |
| PickPlaceCounterToCabinet | 10 | 2.70 | 0.000 | 0 | nan | 0 | 0 | nan | nan |
| TurnOnMicrowave | 10 | 5.11 | 0.000 | 0 | nan | 0 | 0 | nan | nan |
| **Pooled** | **40** | **3.36** | **0.000** | **0** | **nan** | **0** | **0** | **nan** | **nan** |

## Cross-policy summary (matches Table 4 layout)

| Policy class | n_sim | Sim path mean (m) | WM path mean | Binary success | n_wm | AUC(wm_pl) | AUC(wm_fp) |
|---|---|---|---|---|---|---|---|
| Being-H05-2B-libero-robocasa | 80 | 1.96 | 5.97 | 0.100 | 66 | 0.559 | 0.262 |
| Being-H05-2B-robocasa | 80 | 1.93 | 6.31 | 0.100 | 65 | 0.475 | 0.180 |
| Pi0-RoboCasa (RLinf, 4B) | 40 | 3.36 | nan | 0.000 | 0 | nan | nan |


## Notes

- All sim cells (n_sim, sim_path_mean, success_rate) are stable (80 rollouts saved per policy; sim phase is complete).
- WM cells (n_wm, wm_path_mean, AUC) WILL CHANGE as the WM scorers keep running. Re-run `scripts/compute_proposed_table_cells.py` to refresh.
- An AUC value of `nan` for a policy means it has all-success or all-failure in the scored subset (AUC undefined). For the all-failure case, treat the policy like the DP variants in the current Table 4 / 5 — list in Table 4 but mark AUC as `n/a`.
