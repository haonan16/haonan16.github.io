# RPL @ 1/2/4 cm multi-baseline sweep — launch log

**Launched:** 2026-05-19 ~02:10
**Episode:** RPL `2023-06-05_Mon_Jun__5_18_30_00_2023` (single episode)
**Magnitudes:** 0.01, 0.02, 0.04 m (1, 2, 4 cm)
**Axes:** x, y (consistent with internal baseline coverage; ours has z too but
will be filtered to x/y for the cross-baseline plot)

## Submitted jobs

| Job ID | Baseline | Wrapper | Expected wall time |
|---|---|---|---|
| 13783643 | adaLN (raw-vec) | `scripts/sbatch_RPL_124cm_raw_vector.sh BASELINE=adaln` | ~1 hr (3 mags × ~20 min) |
| 13783644 | Cross-attention (raw-vec) | `scripts/sbatch_RPL_124cm_raw_vector.sh BASELINE=crossattn` | ~1 hr |
| 13783645 | Ours (A25750 action-img proj) | `scripts/sbatch_RPL_124cm_ours.sh` | ~30 min (sweep handles 3 mags in one invocation) |
| 13783646 | WorldGym | `scripts/sbatch_RPL_124cm_external.sh BASELINE=worldgym` | ~2–4 hr (3 mags × external runtime) |
| 13783647 | Ctrl-World | `scripts/sbatch_RPL_124cm_external.sh BASELINE=ctrl_world` | ~3–4 hr |

All jobs requested 1× H200 on `seas_gpu`, 128 GB RAM.

## Output dirs (per baseline)

```
results/baselines_comparison/raw_vector_adaln_RPL_124cm/mag_{0p01,0p02,0p04}m/
results/baselines_comparison/raw_vector_crossattn_RPL_124cm/mag_{0p01,0p02,0p04}m/
results/baselines_comparison/ours_A25750_RPL_124cm/mag_{0p01,0p02,0p04}m/
results/baselines_comparison/worldgym_RPL_124cm/mag_{0p01,0p02,0p04}m/<EP_SLUG>/
results/baselines_comparison/ctrl_world_RPL_124cm/mag_{0p01,0p02,0p04}m/<EP_SLUG>/
```

External baselines also auto-run `eval_action_metrics_from_mp4.py` to produce `am.json`
in each magnitude dir.

## Monitoring

```bash
squeue -u haonan -j 13783643,13783644,13783645,13783646,13783647
tail -f slurm_logs/RPL_124cm_*/out_*.out
```

## After all jobs finish — aggregate

```bash
cd /net/holy-isilon/ifs/rc_labs/ydu_lab/Lab/haonan/video_world_model
python scripts/aggregate_baselines_RPL_124cm.py
```

Output:
- `results/baselines_comparison/multi_mag_line.csv` — long format (one row per baseline × magnitude)
- `results/baselines_comparison/multi_mag_line.tsv` — same, tab-delimited
- `results/baselines_comparison/multi_mag_line_paste.tsv` — wide format, paste-ready for Excel line chart

## If a job fails

Each magnitude subdir is independent. Re-run the same sbatch wrapper — the script
detects existing outputs (`summary.json` for internal, MP4s for external) and skips
completed magnitudes. So a partial failure can be resumed by re-submitting.
