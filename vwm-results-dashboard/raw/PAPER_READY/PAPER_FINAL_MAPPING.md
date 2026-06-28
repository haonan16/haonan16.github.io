# Paper → results/ mapping

**Paper:** *Visual Action Grounding for Scalable Robot World Models from Heterogeneous Data* (CoRL 2026 submission).

This is the canonical cross-reference between the paper's final structure and
`results/`. It supersedes the older `MANIFEST.md` numbering, which was drafted
before the paper was finalized and uses different table numbers
(e.g., `PAPER_READY/tables/table5_mppi/` = **paper Table 4**).

The LaTeX source lives at `CoRL-2026-heterogeneous/{main,appendix}.tex`. The
submission `\input`s only 7 artifact files (5 figures + 1 appendix figure
+ 2 Franka table files); all other tables are inlined.

## Main paper

### Tables

| Paper | Topic | LaTeX | Source data (`results/`) | Notes |
|---|---|---|---|---|
| **Table 1** | Action representation — 7 methods × {DirAcc, Cos-align, LPIPS}, 18 DROID scenes × 4 directions = 72 trials/row | Inlined in `main.tex` | `PAPER_READY/tables/table1_ours_action_metrics/`, `table1_cross_attn_action_metrics/`, `table1_raw_vector_adaln_action_metrics/`, `table1_ours_visual_quality/`, `table1_no_action_visual_quality/`, `table1_action_representation/baselines_external/` (WorldGym, Ctrl-World, IntWorldSim, DreamDojo raw outputs), `table1_action_representation/baselines_comparison/` (extracted CSVs) | Per-direction breakdown for Appendix A1 is in `table1_per_direction_diracc/` |
| **Table 2** | Heterogeneous data — DROID / +SSv2 / +SSv2+RoboCasa × {DirAcc, Cos-align, LPIPS, Proprio cos., Proprio L2} | Inlined in `main.tex` | `PAPER_READY/tables/table2_robot_human_action_metrics/`, `table2_robot_human_visual_quality/` | Proprio columns come from the proprio readout head eval (was a separate table in old MANIFEST.md) |
| **Table 3** | Real-world Franka zero-shot — 5 tasks × {A DROID-only, 3way} × {DirAcc, Cos-align} | `CoRL-2026-heterogeneous/tables/tables_franka_prose.tex` + `tables_franka_zero_shot_pairs.tex` | `table3_franka_zeroshot/paper_franka_full_20260527_094933/` (20 GB canonical), `table3_franka_zeroshot/franka_eval/box_zero_shot/`, `table3_franka_zeroshot/franka_eval/sim2real_2026_05_20/`, `table3_franka_zeroshot/franka_eval/sim2real_2026_05_21_10way_ft/` | The 5 tasks are Box, Mustard, Rope, Rice cooker, Whiteboard (raw videos at `data/franka_{box,mustard,rope}`) |
| **Table 4** | World model as MPPI reaching planner — 4 RoboCasa atomic tasks × {Reach@10cm, Reach@5cm, Reach@1cm}, n=20 | Inlined in `main.tex` | `table4_mppi_planner/paper_table5_planner_evidence/` (4.0 GB), `PAPER_READY/tables/table5_mppi/` (frozen aggregate) | Naming legacy: `table5_mppi/` and `table4_mppi_planner/paper_table5_planner_evidence/` were named when this was projected as Table 5. Don't rename — load-bearing scripts reference these paths. |
| **Table 5** | WM-vs-MuJoCo pairwise ranking — 4 tasks × 4 policies, agreed/total pairs | Inlined in `main.tex` | `table5_wm_vs_mujoco/paper_table6_wm_critic_evidence/` (1.8 GB), `table5_wm_vs_mujoco/autonomous_policy_eval_20260518/`, `table5_wm_vs_mujoco/pretrained_policy_eval_20260518_034139/` | Naming legacy: `paper_table6_*` reflects this was projected as Table 6. Same: don't rename. |

### Figures

| Paper | Topic | LaTeX | Source data (`results/`) |
|---|---|---|---|
| **Fig 1** | Teaser — multi-view, fine-grained control, zero-shot, diverse scene | `CoRL-2026-heterogeneous/figures/teaser.pdf` | Built from `PAPER_READY/qualitative/multiview_consistency_selected/`, `qualitative/action_conditioning_sources/`, `qualitative/figure_obvious_motion_2026-05-07/`, `qualitative/paper_qualitative_curated/` |
| **Fig 2** | Architecture diagram | `CoRL-2026-heterogeneous/figures/method.pdf` | Custom diagram, no `results/` source |
| **Fig 3** | Continuous action following — response vs commanded displacement (held-out DROID, RPL) | `CoRL-2026-heterogeneous/figures/response.pdf` | `PAPER_READY/figures/xyz_command_response_A25750_20260512/` (canonical) |
| **Fig 4** | Multi-view consistency — same 3D action, two camera views, two scenes (pick can / pour coffee) | `CoRL-2026-heterogeneous/figures/mutli_view.pdf` | `PAPER_READY/qualitative/multiview_consistency_selected/`, `fig4_multiview/paper_xyz_multiview_six_direction_20260511_024800/` (annotation-free paired views — strong Fig 4 source candidate) |
| **Fig 5** | Qualitative baseline comparison — held-out DROID +y leftward + held-out Franka earbud-pickup, 5 methods (WorldGym, Ctrl-World, IntWorldSim, DreamDojo, Ours) | `CoRL-2026-heterogeneous/figures/baselines_vs_ours.pdf` | Left (DROID): `PAPER_READY/qualitative/paper_qualitative_curated/`, `table1_action_representation/baselines_external/`; Right (Franka earbud): `data/franka_box` rollouts under `table3_franka_zeroshot/franka_eval/box_zero_shot/`, `table3_franka_zeroshot/all_methods_franka_interaction_6ep_20260522_123937/` (5 internal + 3 external methods) |

## Appendix

The LaTeX appendix at `CoRL-2026-heterogeneous/appendix.tex` has 4 sections + 1 figure.

| Section | Topic | Source data (`results/`) |
|---|---|---|
| **A1** | Per-Direction Breakdown of Table 1 — passes, testable counts, conditional DirAcc | `PAPER_READY/tables/table1_per_direction_diracc/`, derived from raw per-trial JSONs under `table1_*_action_metrics/` |
| **A2** | WM-vs-Sim EE-Motion Correlation (Sanity Check) — Spearman ρ between WM and rule-based simulator | `PAPER_READY/tables/table_correlation_wm_sim/` (table_correlation_wm_sim.csv + pooled + with_cosmos variants) |
| **A3** | Excluded Episodes (extended) — testability criterion drops | `configs/eval_episodes_*.txt` plus per-eval `excluded_episodes_*.json` in each table1/2 source dir |
| **A4** | Policy Evaluator Supplementary — within-policy path-length agreement, 5-fold per-trial success classifier, 4-policy MuJoCo stand-in | `table5_wm_vs_mujoco/paper_table6_wm_critic_evidence/`, `table5_wm_vs_mujoco/autonomous_policy_eval_20260518/`, `table5_wm_vs_mujoco/pretrained_policy_eval_20260518_034139/`, `appendix_a4_supplementary/policy_eval_20h_run/` |
| (A4 figure) | WM vs sim policy means scatter | `CoRL-2026-heterogeneous/figures/wm_vs_sim_policy_means.pdf` (NOT generated from `PAPER_READY/figures/fig_wm_vs_sim_correlation/`, which was the old Spearman-ρ scatter for the dropped correlation table) |

## What was dropped from the paper but kept in `results/`

These supported earlier table/figure numbering plans and were moved to
`results/_archive/cleanup_2026-06-12-paper-aligned/paper_ready_orphans/`
on 2026-06-12:

- Tables: `table3_interaction_heavy/` (was Table 4 free-space-vs-interaction), `table4_proprio_robot_*` (proprio is now Table 2 columns), `table5_multiview_consistency/` (now Fig 4 only), and 6 unused stubs (`table4_dataaug`, `table5_safety`, `table6_bc_scaling`, `table7_dataaug_droid`, `table8_robocasa_replay`, `table9_cosmos_policy`, `table_a5c_kinematic_vs_wm`).
- Figures: `fig1_fidelity_panels.png`, `fig2_*`, `fig3_*`, `fig4_*`, `fig5_*`, `fig6_*`, `fig7_*`, `fig_a5c_*`, `fig_horizon_robot_{human,only}/`, `fig_wm_vs_sim_correlation/`, `v3_convergence_2026-05-09.png` — all from earlier figure numbering schemes.

## Notes on the legacy `MANIFEST.md`

`results/PAPER_READY/MANIFEST.md` (last updated 2026-06-02) was drafted while
the paper structure was still in flux. It uses table numbers 1–6 that no
longer match the submission (e.g., MANIFEST Table 3 = WM EE-motion critic was
moved to appendix A2; MANIFEST Table 4 = free-space-vs-interaction was dropped
entirely). Treat `MANIFEST.md` as historical and prefer this file as the
canonical mapping.
