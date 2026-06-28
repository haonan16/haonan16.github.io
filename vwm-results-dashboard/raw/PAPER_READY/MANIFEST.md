# Paper-Ready Artifact Manifest

> **⚠ DEPRECATED 2026-06-12.** This file uses pre-submission table numbering
> (e.g., "Table 3" = WM EE-motion critic, "Table 6" = WM-vs-MuJoCo) that no
> longer matches the submitted paper. **The current canonical mapping is
> [`PAPER_FINAL_MAPPING.md`](./PAPER_FINAL_MAPPING.md)**, which maps the actual
> paper Tables 1–5 + Figures 1–5 + Appendix A1–A4 to result paths.
>
> Kept in place because earlier cleanup READMEs (`results/_archive/2026-05-27_cleanup/README.md`,
> `results/_archive/cleanup_2026-06-12-paper-aligned/README.md`,
> `results/_archive/cleanup_2026-06-12/empty_stubs_*` etc.) cite this file
> directly. Treat the content below as historical pre-submission planning.

Last updated: 2026-06-02

This is the navigation layer for `results/PAPER_READY/`. It cross-references
paper-facing claims in `docs/paper_tables_corl_2026-04-28.md` with curated
artifact folders. Treat videos as protected assets: do not delete or move them
unless a reviewed archive/delete list explicitly names the path.

## 2026-06-01 / 2026-06-02 path* grouping (complete)

Grouped 24 top-level `pathB_*` and `pathC_*` dirs into two paper-claim subdirs:

- `results/paper_table5_planner_evidence/` ← 10 `pathC_*` (Table 5 MPPI/WM-shoot/Random + Table 4 stitching)
- `results/paper_table6_wm_critic_evidence/` ← 14 `pathB_*` (Table 6 WM-vs-MuJoCo critic + appendix tables)

The 24 top-level `pathB_*`/`pathC_*` symlinks created on 2026-06-01 were dropped
on 2026-06-02 after patching all 35 referring scripts via
`scripts/patch_path_refs_2026_06_02.py` (95 in-file replacements). The
load-bearing `CoRL-2026-heterogeneous/scripts/aggregate_tab3_n12.py` was
verified to produce stable output post-patch with `PYTHONHASHSEED=0` pinned.

`pathA_baseline/` kept at top level (5+ doc references; rename would create
more breakage than benefit).

Result: top-level `results/` went from 135 entries (session start) to 30 entries,
with only `pathA_baseline/` and `_archive/` retaining legacy naming.

Also archived 1 stitching smoke dir (`pathC_stitch_smoke/` → `_archive/2026-05-27_cleanup/sanity_and_diagnostics/`).

`pathA_baseline/` kept at top level (5+ doc references; rename creates more breakage than benefit).

## 2026-05-27 results/ cleanup

Top-level `results/` went from 135 → 52 entries. 80 uncited folders moved
into `results/_archive/2026-05-27_cleanup/` (categorized by role); 5 empty
`franka_sim2real_*` skeleton dirs deleted; 5 superseded `_archive/` snapshots
deleted (~2.4 GB). Table 5 numbers were frozen into `tables/table5_mppi/`
before any moves so the load-bearing `_archive/pathC_mppi/` is no longer
on the critical path for re-running aggregation. See
`results/_archive/2026-05-27_cleanup/README.md` for the per-category restore
guide.

Note: the "Appendix A1 / Temporal context" entry below (originally pointing
into `_archive/paper_ready_cleanup_2026-05-07/`) is now stale — the current
`CoRL-2026-heterogeneous/appendix.tex` has no temporal-context section and
that snapshot was deleted in the 2026-05-27 slim pass.

## Folder Status

| Path | Role | Paper use | Status |
|---|---|---|---|
| `README.md` | Folder overview | all paper-ready artifacts | keep |
| `MANIFEST.md` | This cross-reference | all paper-ready artifacts | keep |
| `ARCHIVE_CANDIDATES.md` | Non-destructive cleanup notes | cleanup planning only | keep |
| `tables/` | Metric summaries, per-episode JSON, panels, rollout videos | Tables 1-5, Fig 5 sources | keep |
| `figures/` | Plot source data for horizon curves | Fig 4 | keep |
| `qualitative/` | Paper and appendix videos/images | Figs 1-3, Fig 6/appendix, supplemental videos | keep, curate by subfolder |
| `appendix/` | Step-matched and context/proprio appendix artifacts | appendix tables and diagnostics | keep |

## Paper Cross-Reference

| Paper item | Claim / use | Primary artifact path | Notes |
|---|---|---|---|
| Table 1 | Action representation comparison | `tables/table1_*` | Main "Ours" row uses matched proprio architecture, not the older no-proprio 0.931 checkpoint. |
| Table 2 | Robot-only vs robot+human heterogeneous training | `tables/table2_robot_human_*` | Same nf5 + proprio architecture for both rows. |
| Table 3 | WM EE-motion critic (Spearman ρ vs sim) | `tables/table_correlation_wm_sim/{table_correlation_wm_sim.csv,table_correlation_wm_sim_pooled.csv,table_correlation_wm_sim_with_cosmos.csv}` | Two pools: scripted/random (ρ=0.94, n=60) and incl. Cosmos-Policy 2B real-policy (ρ=0.63, n=382). The `_with_cosmos.csv` artifact was added 2026-05-17 for the Cosmos row. |
| Table 5 (main paper) — frozen | WM-policy closed-loop planner medians (`tab:wm_policy_eval`) | `tables/table5_mppi/` (aggregate_output.txt + README) | Captured 2026-05-27 by running `CoRL-2026-heterogeneous/scripts/aggregate_tab3_n12.py`. README lists the seven input paths the script reads. |
| Table 3 (legacy) | Visual-proprioceptive sampled rollout | `tables/table4_proprio_robot_only/`, `tables/table4_proprio_robot_human/` | Was Table 3 in the 2026-04-28 doc; in the current paper this content sits in Fig.~6 / appendix as proprio cosine-sim, not a numbered table. |
| Table 4 | Free-space vs interaction-heavy episodes | `tables/table3_interaction_heavy/` | Folder name is legacy; paper doc currently labels this as Table 4. |
| Table 5 / Appendix | Multi-view camera-aware response | `tables/table5_multiview_consistency/`, `qualitative/multiview_consistency/` | Same checkpoint, matched episode/action/seed, independent camera views. |
| Fig 1 | Method rendering and action-projection channels | `qualitative/paper_qualitative_curated/fig1_method/`, `qualitative/action_conditioning_sources/` | Use curated stills for the paper; videos are useful for talks/supplement. |
| Fig 2 | SSv2 / human-video examples | `qualitative/paper_qualitative_curated/fig5_ssv2_examples/` | Paper doc still marks clip selection as open. |
| Fig 3 | Robot rollout/action controllability grid | `qualitative/paper_qualitative_curated/fig3_action_controllability/`, `qualitative/interaction_slide_videos_clean_no_labels_all_actions/` | Prefer clean no-label variants for figures. |
| Fig 3 candidate | Six-direction robot+human controllability videos | `qualitative/six_direction_robot_human_bright_2026-05-06/` | Current paper-facing `+x/-x/+y/-y/+z/-z` videos. Use instead of older dark six-direction grids. |
| Fig 3 candidate | Six-direction 2D panels with motion tails | `qualitative/direction_tail_panels_A_step25750_2026-05-06/` | Current still-image figure source for converting video response into 2D paper panels. |
| Fig 3 candidate | Obvious-motion trace videos and stills | `qualitative/figure_obvious_motion_2026-05-07/` | Colored trajectory tails and arrowheads burned in for movement-legible figure drafts. |
| Fig 3 candidate | Larger-magnitude A-step25750 rollouts | `qualitative/obvious_motion_0p05_A_step25750_2026-05-07/` | Clean 0.05m command videos for stronger visible response: `AUTOLab`, `IRIS`, `REAL`, and `ILIAD`. |
| Fig 4 | Horizon control curves | `figures/fig_horizon_robot_only/`, `figures/fig_horizon_robot_human/`, `qualitative/horizon_nf_comparison/` | JSON in `figures/` is canonical for plotting. |
| Fig 5 | No-action vs action vs GT visual-quality comparison | `tables/table1_no_action_visual_quality/`, `tables/table1_ours_visual_quality/`, `tables/table2_robot_human_visual_quality/` | Use panels and summaries as figure sources. |
| Fig 6 / Appendix | Multi-view response | `qualitative/multiview_consistency/side_by_side/` | Best candidate in paper doc points to the paired multiview eval. |
| Paper working folder | Clean combined interaction videos | `qualitative/interaction_clean_combined_all_axes_2026-05-06/` | Self-contained copy of x/y/zero A-vs-CP videos plus CP-only z-axis strips. |
| Supplemental / Appendix | Z-axis interaction response | `qualitative/z_axis_cp_step30000_clean_no_labels/` | CP-only `+z/zero/-z` strips; no robot-only z-axis comparison is implied. |
| Appendix A1 | Temporal context / Markov order | `results/_archive/paper_ready_cleanup_2026-05-07/qualitative/_archive_misc_2026-05-06/` | Archived as older diagnostic material; restore only if these become appendix-facing again. |
| Appendix A2 | Paired multi-view camera-aware response | `tables/table5_multiview_consistency/`, `qualitative/multiview_consistency/` | Supports camera-aware response, not joint multi-view generation. |
| Appendix A4 | Baseline details | `tables/table1_no_action_visual_quality/`, `tables/table1_raw_vector_adaln_*`, `tables/table1_cross_attn_*` | Keep baseline folders even when videos look poor. |
| Appendix A5d | Additional pretrained-policy eval (BeingH05 2B + Pi0 4B) | `../autonomous_policy_eval_20260518/proposed_table4_5_cells.md`, `../pathB_rollouts_beingh05_*`, `../pathB_wm_scores_beingh05_*`, `../pathB_rollouts_pi0_robocasa/` | Added 2026-05-20. WM rows use fast-settings (`--sample_steps 10 --max_sim_steps 360`); see RUN_LOG in the autonomous_policy_eval dir for caveats. |
| Cross-arch baselines (A4 supplement) | Raw-vector AdaLN, raw-vector cross-attn, WorldGym, CtrlWorld controllability | `../baselines_comparison/{multi_mag_line.csv, RPL_2cm_bar.csv, raw_vector_adaln_RPL_124cm/, raw_vector_crossattn_RPL_124cm/, worldgym_RPL_124cm/, ctrl_world_RPL_124cm/}` | Added 2026-05-19. Use as A4 supplement for action-conditioning architecture comparison. |

## Size and Video Inventory

Observed after cleanup on 2026-05-07:

| Scope | Size / count |
|---|---:|
| `results/PAPER_READY/` | 1.6G |
| `qualitative/` | 1.1G |
| `tables/` | 450M |
| `appendix/` | 57M |
| `figures/` | 1.5M |
| Symlinks under `PAPER_READY` | 0 |
| Internal archive moved to `results/_archive/paper_ready_cleanup_2026-05-07/` | 1.4G |
| Superseded archive moved to `results/_archive/paper_ready_superseded_2026-05-07/` | 258M |

## Curation Rules

1. Keep any folder referenced by `docs/paper_tables_corl_2026-04-28.md`.
2. Keep any folder with a `manifest.json` or README that records matched
   episode, camera, start frame, action command, sampling protocol, and seed.
3. Keep raw/baseline artifacts that support a table, even when qualitatively bad.
4. Do not delete videos from `PAPER_READY` directly. First list them in
   `ARCHIVE_CANDIDATES.md`, review the list, then move to `results/_archive/`.
5. Prefer moving whole superseded folders over cherry-picking individual videos;
   this preserves provenance and makes rollbacks simple.

## Provenance Documents

- Paper-facing claims: `docs/paper_tables_corl_2026-04-28.md`
- Experiment record: `docs/record/record_experiments_corl_2026-04-20.md`
- Result layout overview: `results/README.md`
- Qualitative notes: `results/PAPER_READY/qualitative/README.md`
- Table notes: `results/PAPER_READY/tables/README.md`
