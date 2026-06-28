# Long-Context nf13 Preliminary Rollouts

Submitted: 2026-05-02

SLURM job:

- `9748840`: failed during shell startup before inference.
- `9749406`: failed before inference because the repo root was not on
  `PYTHONPATH`.
- `9755016`: active resubmission using `scripts/launch_nf13_long_rollouts.sbatch`,
  which now sets `cd "$PROJECT"` and `PYTHONPATH`.

Checkpoint:

- `outputs/2026-05-01/04-09-33/checkpoints/epoch=0-step=28375.ckpt`

Protocol:

- Model: A nf13, DROID-only visual action projection.
- Bootstrap: real 13-frame DROID history, not repeated first frame.
- Episode: RPL `Mon_Jun__5_18_30_00_2023`.
- Commands: 50 repeated steps for `+x`, `-x`, and `nop`.
- Inference: `sample_steps=12`, `step_m=0.01`, `n_frames=13`.
- Comparison control: all three videos use the same episode, camera view,
  start frame, real visual history, checkpoint, sampling protocol, and seed path.

Intended outputs:

- `A_nf13_step28375_RPL_50step_plusx.mp4`
- `A_nf13_step28375_RPL_50step_minusx.mp4`
- `A_nf13_step28375_RPL_50step_nop.mp4`

Interpretation:

These are preliminary qualitative slide videos only. Do not use them as a main paper
claim until the nf13/nf17 30K checkpoints complete and pass the standard 18-episode
DirAcc + LPIPS evaluation.
