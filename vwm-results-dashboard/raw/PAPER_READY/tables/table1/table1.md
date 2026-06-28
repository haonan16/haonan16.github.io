# Path A: Open-Loop Fidelity (single-bucket)

Per-ckpt mean across held-out RoboCasa demo episodes. Single horizon bucket: the eval runs one streaming clip per item (T_lat - 1 latent frames ~= 0.64s); legacy summaries do not retain per-frame metrics for horizon-decay analysis.

Columns:
- `lpips_mean` / `psnr_mean` / `ssim_mean`: GT vs prediction over the streaming clip.
- `copy_last_lpips_mean`: trivial baseline (repeat last conditioning frame).
- `vae_oracle_lpips_mean`: VAE encode-decode floor on the GT clip.
- `proprio_l2_mm_mean` / `proprio_cos_sim_mean`: only logged when the ckpt has a trained proprio head.

| ckpt | n_items | n_frames | sample_steps | has_trained_proprio_head | copy_last_lpips_mean | lpips_mean | proprio_cos_sim_mean | proprio_l2_mm_mean | psnr_mean | ssim_mean | vae_oracle_lpips_mean |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 00-47-41-step_30000 | 8 | 17 | 40 | True | 0.01261 | 0.01525 | 0.912 | 7.335 | 32.77 | 0.9674 | 0.006364 |
| 04-09-33-step_30000 | 8 | 17 | 40 | False | 0.01222 | 0.0191 |  |  | 31.57 | 0.9634 | 0.006376 |
| 04-58-25-step_25750 | 8 | 17 | 40 | False | 0.008367 | 0.6425 |  |  | 11.87 | 0.4929 | 0.006209 |
| 06-15-30-step_9000 | 8 | 17 | 40 | True | 0.01362 | 0.01803 | 0.9038 | 7.201 | 31.42 | 0.9636 | 0.006381 |
| 11-04-51-step_19875 | 8 | 17 | 40 | False | 0.01237 | 0.01264 |  |  | 33.41 | 0.9722 | 0.006364 |
